

HOW TO SCHEDULE JOBS ON MULTIPLE CPUS?

A fundamental difference between single-CPU hardware and multi-CPU hardware centers
around the use of 'hardware caches'.

with a single CPU it contains small memory-size caches, fast memories that (in general) 
hold copies of popular data that is found in the main memory of the system.

Main-memory, in contrast, holds all of the data, but access to this larger memory is slower. 

By keeping frequently accessed data in a cache, the system can make the large,
slow memory appear to be a fast one.

The CPU anticipates that the data from Main-memory may be reused, 
puts a copy of the loaded data into the CPU cache.

Caches are thus based on the notion of locality: 

1. Temporal Locality - when a piece of data is accessed, it is likely to be
accessed again in the near future. (E.g. variables being accessed over and over again in a loop)

2. Spatial Locality - if a program accesses a data item at address x, 
it is likely to access data items near x as well; here, think of a program
streaming through an array, or instructions being executed one after the
other.

Cache Coherence - when a value x (retrived from main-memory orignally) is modified x' in cpu's cache but not updated to main-memory, and another cpu has same copy of x (but now it is out of date)

Bus Snooping - A bus-based system; each cache pays attention to
memory updates by observing the bus that connects them to main memory.
When a CPU then sees an update for a data item it holds in its cache,
it will notice the change and either invalidate its copy or update it.

As the number of CPUs grows, access to a synchronized shared data
structure becomes quite slow.

Cache Affinity - A process runs on a particular CPU, builds up a fair bit of state in the caches. 
The next time the process runs, it is often advantageous to run it on the same CPU, as some of its state is already present in the caches on that CPU, to avoid reloading the state each time it runs (note it will run correctly on a different CPU with cache coherence protocols of the hardware)

Single-Queue Scheduling (SQMS)

Queue: A -> B -> C -> D -> E

CPU 0: A E D C B (repeat...)
CPU 1: B A E D C (repeat...)
CPU 2: C B A E D (repeat...)
CPU 3: D C B A E (repeat...)

Problems with SQMS:
- locking required of the shared queue between multiple cpus, unscalable with more cpus because more time is spent locking queue than work being done.
- cache affinity, job A is run, stopped and re-run in different cpus, need to load and reload cached values


Multi-Queue Scheduling (MQMS)

consists of multiple scheduling queues. 
Each queue will follow a particular scheduling discipline, such as round robin.
When a job enters the system, it is placed on exactly one scheduling
queue, according to some heuristic (e.g., random, or picking one with
fewer jobs than others). Then it is scheduled essentially independently,
thus solving the problems of information sharing and synchronization
found in the SQMS.

Queue 1: A -> B
Queue 2: C -> D

CPU 0: A A B B A A (repeat...)
CPU 1: C C D D C C (repeat...)

Pros:
- More scalable, less locking on queues
- provides cache affinity

Cons:
- load inbalance, for example

Queue 1: A
Queue 2: C -> D

CPU 0: A A A A A A (repeat...)
CPU 1: C C D D C C (repeat...)

Solving load inbalance:
A technique called 'migration' continuous migrates of one or more jobs to another queue to balance the load. Another technique called "work stealing" for deciding which queue to have a new job. A queue may detect another queue to see if it is more full than itself, it yes then it will take a job from the target queue. However, there needs to be a balance between the frequency of migrations and load balancing, as migration has overhead of transfering reloading caches.

Linux Multiprocessor Schedulers
3 different schedulers arose: the O(1) scheduler, the Completely Fair Scheduler
(CFS), and the BF Scheduler (BFS)

Both O(1) and CFS use multiple queues, whereas BFS uses a single
queue, showing that both approaches can be successful. The
O(1) scheduler is a priority-based scheduler (similar to the MLFQ discussed
before), changing a process’s priority over time and then scheduling
those with highest priority in order to meet various scheduling objectives;
interactivity is a particular focus. CFS, in contrast, is a deterministic
proportional-share approach (more like Stride scheduling, as discussed
earlier). BFS, the only single-queue approach among the three, is also
proportional-share, but based on a more complicated scheme known as
Earliest Eligible Virtual Deadline First (EEVDF) 




