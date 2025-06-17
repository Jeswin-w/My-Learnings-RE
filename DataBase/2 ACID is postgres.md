# ACID in postgres

post gres transaction are atomic and isolated by default

**atomic** -> error / crash in a transaction rollsback evertything.

**isolation** -> read commited by default, the transactions are isolated untill commited

if snapshot level isolation is needed 

```sql
begin transaction isolation level repeatable read;
```
Consistant -> when transactions are completely isolated then the txns are consistent.

Durability-> if the transaction is commited, and db crased immediately then the data is stored

    In case when pg writes to disk but OS writes to lache and the machine restarts pg uses wal or fsync to be durable.

## prevent phantom reads
using repeatable read or serializable isolation
will prevent this

but in other DBs usually we need to use serializable.

eg)
```
create table test (id integer)
insert into test (2);
 
begin transaction 1
t1 - set isolation level read committed
begin transaction 2
t2 - set isolation level repeatable read
 
t1 - select * from test;
t2 - insert into test (4);
t1 - select * from test;
t1 - insert into test (5);
t2 - select * from test;
t1 - commit;
t2 - select * from test;
t2 - commit;
```



in postgres and mysql it is 2 | 2 | 2-4 | 2-4

in other dbs 2 | 2 | 2-4 | 2-4-5

## Serializable vs non repeatable isolations

Imagine a scenario

table has 2 values a , b

txn 1 converts a to b
txn 2 converts b to a

in non readable it will be b,a

in serializable pg throws an error

as we are reading and updating. we need to try this again