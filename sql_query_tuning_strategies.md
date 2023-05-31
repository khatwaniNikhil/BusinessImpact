# Tooling & Metrics 
## Pt-querydigest - mysql slow query logs
1. Top 20 results(query fingerprints-similar queries grouped) ranked by exec time analysed over business heavy saas mysql db server:
   1. total query count
   2. total exec time
   3. row locks
   4. rowexamined versus rows sent
   5. exec. time distribution

## Query Explain
1. Index related
  1. Look for index used or not in “Extra” column
  2. Look for which index used in “Key” column
  3. Look for Index scan details in “type” column
     1. “Const” and “system”, single row scan(fastest)
     2. "eq_ref", during join one row of table1 is mapped to only one row of table2
     3. "ref", during join one row of table1 is mapped to many rows of table2
  4. Candidate indexes using “possible_keys” column 
2. Where clause evaluation (https://use-the-index-luke.com/sql/explain-plan/mysql/access-filter-predicates)
  1. "access predicate" fastest due to start and stop conditions around the leaf node traversal -  check (“key_len”, “ref” columns)
  2. "Index filter predicate", do not contribute to the start and stop conditions and do not narrow the scanned range - check (“Using index condition”, since MySQL 5.6)
  3. "Table level filter predicate" - Predicates on columns which are not part of the index are evaluated on the table level, the database must load the row from the table first - (“Using where” in the “Extra” column)

# Tuning Strategies
1. Strive for usage of clustered index, fallback to secondary index for based data retrieval. “Extra” column of explain showing “Using Index”.
2. Strive for same index that is used for the where clause must also cover the order by clause.
3. Same column for order by as used for group by - otherwise index won't be used.
4. reduce no. of joins leading to more optimised indexed being choosen by query plan
5. order by auto increment indexed id column instead of created column
6. Look for adding functional indexes/index over generated columns like UPPER(FIRST_NAME)
7. Join tables to use proper index usage —columns compared should be of same sql type and size
8. In order to avoid disk based base table lookup, extend the existing indexes to cover all columns from the where clause, even if they do not narrow the scanned index range.

## Order By Tuning
    1. If possible, use the index ordering, no need of explicit sort logic to run
    2. If index sorting can't be used, filesort (quick sort over chunks of data followed by merge sort). If sorted chunks can't be stored in memory, temp. file is used.
## Group By Tuning
    1. Consider group by smaller data and then join other tables"" over ""join all tables and then group by""
    2. Two group by algo's used internally
        1. the hash algorithm --> aggregates the input records in a temporary hash table
        2. the sort/group algorithm -->first all records are sorted by grouping key and then groups are created on top of it.
    3. In case of 2.2 if we should try to leverage index sorting and avoid sort overhead and make group by pipelines operation(intermediate results can be processed). Oracle confirms the skipping of sort step via ""SORT GROUP BY NOSORT"" comments in explain"
    4. https://www.percona.com/blog/2015/06/15/speed-up-group-by-queries-with-subselects-in-mysql/
    5. https://use-the-index-luke.com/sql/sorting-grouping/indexed-group-by


# Performance Schema
https://vladmihalcea.com/mysql-query-profiling-performance-schema/?unapproved=110496&moderation-hash=180cb3e57f6cea69aff596bfa5d924ed#comment-110496
