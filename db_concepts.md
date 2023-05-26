# INDEX
1. Balanced tree (tree depth is equal at every position; the distance between root node and leaf nodes is the same everywhere.)
    1. logic ordering of data is achieved in index (side effect is keeping reductant data)
        1. Layered 
            1. Root node 
            2. Intermediate nodes - points to largest value in the pointed leaf nodes for efficient searching.
            3. Leaf nodes (Leave nodes are connected to each other via doubly linked list)
        2. index construction
            1. Start from bottom/leaf nodes, create intermediate node with multiple values pointing to multiple leaf nodes(points to largest value in the pointed leaf nodes for efficient searching.
            2. Keep adding layers of intermediate node until reached to root node 
        3. Logarithmic scalability …with each depth no of elements grows very fast. In real world, upto five depth index tree contain million of rows
2. Clustered versus secondary indexes
    1. PK based clustered index already explained above
    2. Secondary index are “index on a clustered index”.
    3. Other indexes created later —each node contains indexed column as well as primary key values also. After first level search happens in this other index, second level search happens in clustering index basis PK to find actual data basis columns queries.
    4. That means that accessing a table via a secondary index searches two indexes: the secondary index once (INDEX RANGE SCAN), then the clustered index for each row found in the secondary index (INDEX UNIQUE SCAN).
    5. Index data structures which are not co-located with the data.
    6. Other indexes created later on table refer to this primary index for data table. Clustering index acts as table store (in contrast to heap tables). You can also treat is as index that happens to have all table columns.

# POWERS OF INDEXING (use-the-index-luke.com)
1. B-Tree Very efficient balanced binary tree traversal
    1. Balanced —access all nodes in same no of steps
    2. logarithmic growth of the tree depth —
        1. (the tree depth grows very slowly compared to the number of leaf nodes)
        2. Real world indexes with millions of records have a tree depth of four or five
2. Clustering - the index leaf nodes store the indexed columns in an ordered fashion
    1. index leaf nodes store the indexed columns in an ordered fashion so that similar values are stored next to each other
    2. index clustering gotchas(effect of physical distribution)
        1. But in case of too many leaf nodes scan, similar rows look over disk might have diff cost associated to them depending upon they are located on single disk block or diff blocks.
        2. Adding more columns from where clause to index will avoid separate disk blocks lookup (filter predicate will become an access predicate)
3. Pipelined order by
    1. ordering of data via index leaf nodes make “order by”  clause execution fast (no need to explicit sort)
    2. Pipelined —it is also able to return the first results without processing all input data, very fast in case of pagination queries

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
