# References
1. https://vladmihalcea.com/optimistic-vs-pessimistic-locking/
2. https://www.linkedin.com/pulse/read-committed-pessimistic-locking-distributed-sql-databases-pachot/

# Concepts
1. Isolation Level(I in ACID) == concept
2. locking(pessimistic or optimistic) == implementation
3. Serializable(pessimistic  locking) ==  prevents all race condition inconsistencies + application to implement a retry logic
4. Read Committed(pessimistic locking) == do avoid race conidtion inconsistencies on need basis - SELECT FOR UPDATE
5. Repeatable Read(pessimistic locking) == do avoid race conidtion inconsistencies on need basis - handling for new rows
6. Snapshot Isolation(optimistic locking)

# Optimistic Locking
1. allow conflict to occur, detect and retry via versions)
2. during the whole txn -- work is based on initial txn start state which at commit time if changed lead to rollback.
3. retry policy at client side needed - also how many times retry ?

![](https://vladmihalcea.com/wp-content/uploads/2021/03/LostUpdateOptimisticLocking.png)

## when to use
1. applications that do many more reads than updates or deletes. less rollbacks at the time of commit as version mismatch will be less.
2. avoids lost updates in long conversation(multiple application level txn’s like search product followed by buy product)


# Pesimistic Locking(avoid conflict altogether)
1. involves locking entities on the database level.
2. during the whole txn -- work is based on final commit txn start state as locks were taken at the start only

![](https://vladmihalcea.com/wp-content/uploads/2021/03/LostUpdatePessimisticLocking.png)

## when to use
1. cost of retry is high
2. write parallelism is high and don’t want many rollbacks
3. Exclusive locks are acquired on everything you write, to prevent concurrent reads and writes on an intermediate state. And shared locks are acquired on everything you read, to prevent concurrent writes changing the state you have read.  Only Serializable  isolation level that can achieve the same.

# Application-level transactions(long conversations style communication b/w client and server)
1. Only optimistic locking can avoid lost updates 



