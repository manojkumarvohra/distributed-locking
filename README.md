# distributed-locking
 ..... ways to create a distributed lock to handle inter process communication

#Abstract
Given that now we have scalable architectures which follow horizontal scaling rather vertical often we come accross scenarios where we have to synchronise some routines with in different servers. 

The solutions differs from being non-blocking using SAGA like patterns to traditional blocking patterns.

Here we would cover the latter. 

#Design/Approach
To establish a distributed lock we choose an atomic variable which is maintained outside the different processes invloved but the same value is referred from all the processes.

We maintain binary values as 0 and 1 to denote lock_not_aquired and lock_aquired respectively.

Atomic updates ensure that only one of the process is able to lock the variable at one time while rest go in sleep and retry after some time.

