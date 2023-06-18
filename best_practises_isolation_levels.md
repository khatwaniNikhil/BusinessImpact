# References
https://planetscale.com/blog/pitfalls-of-isolation-levels-in-distributed-databases#case-b-retail

# Use Cases
1. customer refers to changing exchange rates and places order(assume that - we are not be concerned if exchange rate changes behind the scene while he is placing the order).
2. bank balance deduction:  read customer balance->add activity log->check sufficient balance->deduct balance

# Why "serializable" isolation level will not scale/impractical for contentious workload
1. frequent deadlocks: 
	refer bank balance deduction, two parallel flows took shared read lock and read balance and later both tried to upgrade to exclusive write lock and waiting for other to release the lock.

2. unintended dependency, scaling issues:
	if order txn is slow/complex and exchange rate lookup is made within txn, exchange rate update module has to wait till customer order is complete.

3. same challenges arises even we use lock based or lock free impl. of  "serializable" isolation level.

# RepeatableRead
1 ambiguous, differentiates point selects from searches(Phantom reads are possible when you are using queries or indexes because locks are not acquired for ranges of data)
2. for above use cases, RepeatableRead offers the same guarantees as Serializable and consequently inherits the same problems

# SnapshotRead isolation level/MVCC
1. not an ANSI standard but popular
2. contention-free: creates a snapshot at the beginning of the transaction
	2.1 for use case 1 - exchange rate are read from snapshot and exchange rate update module is not blocked and can update independently.
3. useful for read only txn: works without locks(using snapshot)
4. it is no better than ReadCommitted for write workloads
5. change Data Capture tools can leverage snapshots
6. ability to perform on-demand Serializable read  using "lock in share mode' 
7. "select for update" helps in two racing transactions to successfully complete without encountering a deadlock

# ReadCommitted
1.  it continuously returns the latest view of the database, no confusion like repeatable read range queries behaviour
2. least contentious of the isolation levels
3. always returns the latest view of the database(you may get a different value every time you read a row)
4. ability to perform on-demand Serializable read using "lock in share mode' 
5. considered unsafe and is not recommended for distributed or non-distributed settings - you may read data that might have later been rolled back 

# Rule of thumb
1. try mainly relying on the atomic guarantees of transactions, and stay at the lowest isolation level the database system supports.
2. an application should avoid relying on any advanced isolation features of a database
	 If you can write an application to work with ReadCommitted isolation level, then moving to SnapshotRead should be discouraged
	 Serializable or RepeatableRead are almost always a bad idea.
3. For distributed databases, avoid distributed transactions/2 Phase commit. This can be achieved by keeping related rows within the same shard. 






