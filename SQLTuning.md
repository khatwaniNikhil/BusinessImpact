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
3. Same column for order by as used for group by
4. reduce no. of joins leading to more optimised indexed being choosen by query plan
5. order by auto increment indexed id column instead of created column
6. Look for adding functional indexes/index over generated columns like UPPER(FIRST_NAME)
7. Join tables to use proper index usage —columns compared should be of same sql type and size 
