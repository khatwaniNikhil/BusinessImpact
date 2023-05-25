# Handy Commands
1. show variables like 'query_cache_%' ;

| Variable_name | Value           |
| ------------- |:-------------:|
| query_cache_limit            | 524288   |
| query_cache_min_res_unit     | 4096     |
| query_cache_size             | 33554432 |
| query_cache_type             | ON       |
| query_cache_wlock_invalidate | OFF      |


2. query_cache_limit --- 0.5M
maximum size of individual query results that can be cached  

3. query_cache_min_res_unit -- 4KB
MySQL does not handle cached data in one big chunk; instead it is handled in blocks. The minimum amount of memory allocated to each block is determined by the query_cache_min_res_unit variable

4. query_cache_size -- 33.55 MB
 total amount of memory allocated to the query cache

5.  query_cache_type ---
	Setting this value to 0 or OFF prevents caching or retrieval of cached queries. 
  You can also set it to 1 to enable caching for all queries except for ones beginning with the SELECT SQL_NO_CACHE statement. 
  A value of 2 tells MySQL to only cache queries that begin with SELECT SQL_CACHE command.

6. query_cache_wlock_invalidate --
	 whether MySQL should retrieve results from the cache if the table used on the query is locked.	
