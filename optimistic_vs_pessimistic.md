# Optimistic Locking
allow conflict to occur, detect and retry via versions)

#### when to use
applications that do many more reads than updates or deletes. No rollbacks at the time of commit due to version mismatch will be less.

# Pesimistic Locking(avoid conflict altogether)
involves locking entities on the database level.

#### when to use
when cost of retry is high then use pessimistic  — long running txn
when parallelism is so high that if optimistic is used then most will rollback
cons
affects liveliness
