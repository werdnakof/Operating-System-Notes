Lets start with a simplified model of processes with unrealistic assumptions,

1. Each job runs for the same amount of time.
2. All jobs arrive at the same time.
3. Once started, each job runs to completion.
4. All jobs only use the CPU (i.e., they perform no I/O)
5. The run-time of each job is known.

Performance Metrics: 
1. T-turnaround = T-completion - T-arrival
2. T-response = T-firstrun - T-arrival

                                        Scheduling Policies

3 jobs: A, B and C. Each take ten seconds complete, and arrvies at the same time.


    First In First Out (FIFO)

The average T-turnaround is ((10-0) + (20-0) + (30-0))/3

Relaxing assumption 1 - job A now takes 100 seconds to complete, 
now the average T-turnaround  is 110. ((100-0) + (110 - 0) + (120 - 0)) / 3

This convoy effect where a number of relatively-short potential consumers of a resource get queued
behind a heavyweight resource consumer


    Shortest Job First (SJF)

The average T-turnaround is ((10-0) + (20-0) + (120-0)) / 3 = 50

Relaxing assumption 2 - job A arrives at time 0, B and C arrives at 10.
now the average T-turnaround is 103.33 ((100-0) + (110 - 10) + (120 - 10)) / 3


    Shortest Time-to-Completion First (STCF)

Adding preemption to SJF, known as the Shortest Time-to-Completion First (STCF)

The STCF scheduler determines which of the remaining jobs (including the new job) has the least time left, and schedules that one first, and return to others later.

now the average T-turnaround is 50 ((120-0) + (20 - 10) + (30 - 10)) / 3


    Round Robin (RR)

SJF and STCF suffers from bad response time i.e. 2nd performance metics.

RR runs a job for a time slice, and switches to next job.

Given 3 jobs: A, B and C. 
Each take 5 seconds complete, and arrvies at the same time.

Response times:
SJF - ((5-0) + (10 - 0) + (15 - 0)) / 3 = 10
STCF - ((5-0) + (10 - 0) + (15 - 0)) / 3 = 10
RR with a time slice of 1 second - (0 + 1 + 2) / 3

Turn around Time:
SJF - ((5-0) + (10 - 0) + (15 - 0)) / 3 = 10
STCF - ((5-0) + (10 - 0) + (15 - 0)) / 3 = 10
RR - ((13-0) + (14 - 0) + (15 - 0)) / 3 = 14

RR gives bad turn around time.

Making time slice shorter gives better response time, but cost of context switch will dominate

Deciding on the length of the time slice presents a trade-off to a system designer,
making it long enough to amortize the cost of switching without making it so long that the system is no longer responsive.


    multi-level feedback queue (MLFQ)

Relax assumption 4 and 5

MIFQ consists number of queues with dcreasing priorities, and rules:

Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t).
Rule 2: If Priority(A) = Priority(B), A & B run in RR.
Rule 3: When a job enters the system, it is placed at the highest priority (the topmost queue).
Rule 4: Once a job uses up its time allotment at a given level 
(regardless of how many times it has given up the CPU), its priority is reduced (i.e., it moves down one queue).
Rule 5: After some time period S, move all the jobs in the system to the topmost queue.

when an I/O is called, cpu automatically goes to the job with the same or next priority,
and when a process uses up its time slice, its priority drops,
however, a process can issue a fake short running I/O which takes little time before the time slice ends,
then it can stay in the same priority! Rule 4 solves this issue.



