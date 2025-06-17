# Eventual Consistency

### Connsistency in data 
when we have multiple views/ collections the data in view should be consistent

relational data is great in this

 1. Defined by user
 2. Referential Integrity (foreign key)
 3. Atomicity
 4. Isolation

### consistency in reads

after update, the next read should get the updated value.

in which case does this not happen?
    when we add caches, reads or slave DB.

Master DB will be updated first and then replicated to other DBs. 

This is eventual Consistancy


