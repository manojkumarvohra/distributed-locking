# distributed-locking
 This project describes ways to create a distributed lock to handle inter process communication.

# Abstract
Given that now we have scalable architectures which follow horizontal scaling rather vertical often we come accross scenarios where we have to synchronise some routines with in different servers. 

The solutions differs from being non-blocking using SAGA like patterns to traditional blocking patterns.

Here we would cover the latter. 

We would choose a datastore where response times are quick and which is fault tolerant.
We considered Redis cluster for this but we can also go with zookeeper quorum for same.

# Design/Approach
To establish a distributed lock we choose an atomic variable which is maintained outside the different processes invloved but the same value is referred from all the processes.


# Approach-1
We maintain binary values as 0 and 1 to denote **lock_not_aquired** and **lock_aquired** respectively.

Atomic updates (compareAndSet(0,1)) ensure that only one of the process is able to lock the variable at one time while rest go in sleep and retry after some time.

![Alt text](/way1.jpg?raw=true "approach-1")

# Approach-2
![#f03c15](https://placehold.it/15/f03c15/000000?text=+) We observed that Redis along with Jedis in cluster mode doesn't supports compareAndSet operation.

So we designed locks ourselves.

We maintain binary states (any value > 1) and 1 to denote **lock_not_aquired** and **lock_aquired** respectively.

We used incrementAndGet atomic operation in this approach.
- If the returned value comes to be 1, we consider this as *lock_aquired*
- If the returned value is > 1, we consider this as *lock_not_aquired*
    - In this case we will call a subsequent decrementAndGet to return the variable to its previous state.
- The process which aquired lock, for release would call decrementAndGet operation.    
    
![Alt text](/way2.jpg?raw=true "approach-2")

