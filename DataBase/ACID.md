# ACID

### Transaction
It is a collection of queries.It considers all queries as 1 unit of work.

eg) deposit money (Select money, update - deduct money, update -  deposit)

TransactionLifespan
transaction begin
transaction commit
transaction rollback - when query in transaction fails

commits fastst in pgsql

when to use read only transaction - > when transaction needs a snapshot of data in time of start of transaction

even if we dont use transaction -> db will create a transaction and commits immediately

### Atomicity
### Isolation
### Consistency
### Durability 