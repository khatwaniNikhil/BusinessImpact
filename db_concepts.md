# Data Storage
1. Heap: No ordering of table records during storage. Will lead to table scan without any explicit index.
2. Clustered table/clustered index: Ordering of records during storage and implicit index over PK is created for fast lookup. Clustered index leaf nodes contains the actual table data. Therefore only one clustered index per table. Innodb created clustered index.

# INDEX
1. MySQL supports a few different index types. The most important are BTREE(balanced tree) and HASH. 
2. Clustered index already covered above.
3. Secondary index: Needed for ordering data other than physical ordering via clustered index. Secondary index can have its own order and is agnostic to base table order. Leaf nodes of secondary Index has references/pointers to base table data.
4. Balanced tree (tree depth is equal at every position; the distance between root node and leaf nodes is the same everywhere.)
    1. logic ordering of data is achieved in index (side effect is keeping reductant data)
        1. Layered 
            1. Root node 
            2. Intermediate nodes - points to largest value in the pointed leaf nodes for efficient searching.
            3. Leaf nodes (Leave nodes are connected to each other via doubly linked list)
        2. index construction
            1. Start from bottom/leaf nodes, create intermediate node with multiple values pointing to multiple leaf nodes(points to largest value in the pointed leaf nodes for efficient searching.
            2. Keep adding layers of intermediate node until reached to root node 
        3. Logarithmic scalability …with each depth no of elements grows very fast. In real world, upto five depth index tree contain million of rows
5. Clustered versus secondary indexes
    1. PK based clustered index already explained above
    2. Secondary index are “index on a clustered index”.
    3. Other indexes created later —each node contains indexed column as well as primary key values also. After first level search happens in this other index, second level search happens in clustering index basis PK to find actual data basis columns queries.
    4. That means that accessing a table via a secondary index searches two indexes: the secondary index once (INDEX RANGE SCAN), then the clustered index for each row found in the secondary index (INDEX UNIQUE SCAN).
    5. Index data structures which are not co-located with the data.
    6. Other indexes created later on table refer to this primary index for data table. Clustering index acts as table store (in contrast to heap tables). You can also treat is as index that happens to have all table columns.


# LINKS
1. https://use-the-index-luke.com
2. https://use-the-index-luke.com/blog/2014-01/unreasonable-defaults-primary-key-clustering-key
3. https://www.freecodecamp.org/news/database-indexing-at-a-glance-bb50809d48bd
4. https://dev.mysql.com/doc/refman/5.7/en/innodb-index-types.html
5. https://www.codeo.co.za/blog/database-performance-if-youre-not-first-youre-last
6. https://dev.mysql.com/doc/refman/5.6/en/mysql-indexes.html
7. https://stackoverflow.com/questions/9687230/how-to-enable-per-table-sql-statement-logging-in-mysql
8. https://eng.uber.com/postgres-to-mysql-migration/
9. https://www.sqlshack.com/clustered-index-vs-heap/
10. https://ducmanhphan.github.io/2020-04-12-Understanding-about-clustered-index-in-RDBMS/
11. https://asktom.oracle.com/pls/apex/f?p=100:11:0::::P11_QUESTION_ID:1032431852141
12. https://www.red-gate.com/simple-talk/databases/sql-server/learn/effective-clustered-indexes/
