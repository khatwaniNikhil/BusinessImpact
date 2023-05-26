# Scheduled Index Stats check
1. query basis last_update threshold
SELECT database_name, 
       table_name, 
       n_rows, 
       last_update 
FROM   mysql.innodb_table_stats 
WHERE  database_name = 'uniware'; 

2. control over the quality of the statistics estimate using - "innodb_stats_transient_sample_pages".

# Stats - Persistence and Computation
1. Since 5.6.6, stats are persisted by default over disk, not lost on restarts. 
    1. show variables like '%innodb_stats_persistent%';
2. stats recalculation (default on) 
    1. show variables like '%innodb_stats_auto_recalc%';
    2. calculated automatically when a table undergoes changes to more than 10% of its rows
    3. asynchronous nature of automatic statistics recalculation,
    4. For sync, need to run analyse table
    5. When an index is added to an existing table, or when a column is added or dropped, index statistics are calculated and added to the innodb_index_stats table regardless of the value of innodb_stats_auto_recalc


