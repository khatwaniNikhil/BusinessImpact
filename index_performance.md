Index is
1. redundancy, copy of data
2. sorted for faster search
3. B Tree(balanced tree with doubly linked list(for connecting leaf nodes))
   1. Tree depth is same for each root to leaf node.
5. Index leaf node
   1. each leaf node is stored in db block/page and contain multiple index entries
	 2. each leaf node points to actual table row(rowID)
	3. why doubly linkedlist to connect leaf nodes - as new data is inserted, new leaf node can be easily added without moving two much data.

6. Index branch node
    1. Branch node contains multiple entries with entry corresponds to the biggest value in the respective leaf node.
![fig01_02_tree_structure en fR0L0Q0f](https://github.com/khatwaniNikhil/RDBMS_learning/assets/3686308/4922f6de-4c73-4439-a3ba-3c548916ac8c)

5. Index leaf nodes level - Two levels of ordering 
   1. order among index entries within each leaf node
   2. order among leaf nodes via doubly linked list
      
6. No ordering in table data storage - neither among physical block/page level data and not among physical page blocks
![fig01_01_index_leaf_nodes en R2811XpS](https://github.com/khatwaniNikhil/RDBMS_learning/assets/3686308/d95bae97-b949-4fcb-be69-90bbabc4bdec)

7. Index traversal
   1. Tree traversal ...bounded to depth of the tree(generally 4/5 depth trees can contain millions of enteries)
   2. one or more leaf nodes traversal (not bounded, could be slow fetch)
   3. actual physical tables block fetch (random disk seeks could be slow and in proportion to count of leaf nodes matching the current query)

8. Different index scan types and latency impact
1. INDEX UNIQUE SCAN(in oracle)/type=const(in mysql)  == Tree scan but no multi leaf node scan
2. INDEX RANGE SCAN(in oracle)/type=ref(in mysql) ==  Tree scan with leaf chain scan
3. TABLE ACCESS BY INDEX ROWID == post one of the above scans, this operation performs actual data lookup from physical table 

9. Slow Query
   1. Where clause

	

