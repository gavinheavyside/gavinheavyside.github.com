---
layout: post
title: "LIMIT 1 and performance in a Postgres query"
description: ""
category:
tags: []
---
{% include JB/setup %}

I've just been debugging a slow-running report, and reduced the runtime from
over 3 hours to under 5 minutes. The problem turned out to be the impact of
`LIMIT 1` on a simple SQL query on a Postgres DB.

The report is fairly straightforward, and the queries it executes are also
pretty simple, so it was a surprise to see how slowly it ran in production.

The script is written in Ruby, running against one of our Rails applications.
Instrumenting the script showed that one particular query, which only used one
table, was often taking several seconds. It wasn't obvious to me why it should
be so slow. The table has several million rows, but sensible indexes are in
place, and we don't have any problems in production. I restored a backup of the
DB to a isolated box, and started to experiment.

I took the generated SQL of the query, and looked at the query plan and
performance. `EXPLAIN ANALYZE` is your friend here, as you get both the query
plan and the actual runtime of the query:

```sql
EXPLAIN ANALYZE SELECT field1 FROM table WHERE field2 = value ORDER BY field3 DESC LIMIT 1;

QUERY PLAN
--------------------
  Limit  (cost=0.00..524.25 rows=1 width=34) (actual time=5572.027..5572.028 rows=1 loops=1)
  ->  Index Scan Backward using index_table_on_field3 on table  (cost=0.00..569861.98 rows=1087 width=34) (actual time=5572.018..5572.018 rows=1 loops=1)
Filter: (field2 = value)
  Total runtime: 5572.056 ms
```

A runtime of over five seconds. How many rows does are we working with?

```sql
SELECT COUNT(*) FROM table WHERE field2 = value;

  count
-------
  1020
```

5.5 seconds to pick a single row from just 1020 records? Something isn't right.
How long would it take to get ALL THE RECORDS? I dropped the `LIMIT 1` from the
query:

```sql
EXPLAIN ANALYZE SELECT field1 FROM table WHERE field2 = value ORDER BY field3 DESC;

QUERY PLAN
--------------------
Sort  (cost=1080.91..1083.63 rows=1087 width=34) (actual time=46.318..47.194 rows=1020 loops=1)
  Sort Key: field3
  Sort Method: quicksort  Memory: 104kB
  ->  Index Scan using index_table_on_field2 on table  (cost=0.00..1026.09 rows=1087 width=34) (actual time=0.245..44.305 rows=1020 loops=1)
Index Cond: (field2 = value)
  Total runtime: 48.199 ms
```

WTF? 48ms to return all the records, but 5.5s for just one? Also note that the
query planner is now using the index on field2 (i.e. 1020 rows) instead of the
index on field3 (several million rows). I hit the internets, and found that
adding a `LIMIT` can often confuse the query planner. [This
post](http://dba.stackexchange.com/a/19744) suggested adding conditions to the
`ORDER BY`. I added `field1` to the ordering and the results were startling:

```sql
EXPLAIN ANALYZE SELECT field1 FROM table WHERE field2 = value ORDER BY field3 DESC, field1 LIMIT 1;

QUERY PLAN
--------------------
Limit  (cost=1031.53..1031.53 rows=1 width=34) (actual time=3.856..3.857 rows=1 loops=1)
->  Sort  (cost=1031.53..1034.24 rows=1087 width=34) (actual time=3.853..3.853 rows=1 loops=1)
  Sort Key: field3, field1
  Sort Method: top-N heapsort  Memory: 25kB
  ->  Index Scan using index_table_on_field2 on table  (cost=0.00..1026.09 rows=1087 width=34) (actual time=0.028..2.667 rows=1020 loops=1)
Index Cond: (field2 = value)
  Total runtime: 3.890 ms
```

So we've gone from 5.5 seconds to just 3.9ms, by adding an irrelevant condition
to the `ORDER` clause. Adding `.order(:field1)` to my Rails script got that
added to the generated SQL, and the run time of the whole report reduced from
over 3 hours to under 5 minutes.
