#### transaction:
a unit of work enacted on the data; one client contacts a database to perform one transaction, while simultaneously, another client contacts a database and perform another transaction.
It should be made sure that the transactions do not interfere with each other.

ACID (atomicity, consistency, isolation, durability)

* Atomicity: It’s an all or none concept which enables the user to be assured of incomplete transactions to be taken care of. The actions involving incomplete transactions are left undone in DBMS.

Note: why would a transaction get aborted/undone? 
1. One transaction may interfere with another transaction, which causes abortion. 
2. Power failure, or internal error of database itself.

* Consistency: every transaction should conform to all the defined rules (as specified in DDL, and key constraints and such on).

* Isolation: each transaction should be independent of each other, should not interfere with each other in any way. 
The issue of concurrency is when multiple clients acting on the same piece of data at the 
* 
