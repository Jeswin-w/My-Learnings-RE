# Backend Connection Patterns

1. 1 process does all listen, accept, read

This won't scale when there are too many connections

eg) node js

2.  1 listener, 1 acceptor, n readers

the acceptor acccepts the connection and assigns it to the n threads readers
cons: The thread might not be load balanced. 

eg) memcached (redis like db)