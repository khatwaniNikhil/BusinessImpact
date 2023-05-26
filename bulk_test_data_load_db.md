1. "perl -E 'for($i=1;$i<=6000000;$i++){say ""$i,"",int rand 282 + 1, "",0,0,0""}' >> test.csv
2. perl -E 'for($i=6000001;$i<=7200000;$i++){say ""$i,"",int rand 282 + 1,"","",int rand 99 + 1,"","",int rand 99 + 1,"","",int rand 99 + 1}' >> test.csv
3. LOAD DATA INFILE '/home/nikhil.khatwani/test.csv' INTO TABLE uniware.item_type_inventory_test FIELDS TERMINATED BY ',' ENCLOSED BY '""' LINES TERMINATED BY '\n' IGNORE 1 ROWS;
4. ALTER TABLE item_type_inventory_test ADD COLUMN `type` enum('GOOD_INVENTORY','BAD_INVENTORY','QC_REJECTED','VIRTUAL_INVENTORY') DEFAULT NULL; 
5. update item_type_inventory_test set type = elt(floor(rand()*4) + 1, 'GOOD_INVENTORY', 'BAD_INVENTORY', 'QC_REJECTED','VIRTUAL_INVENTORY');
