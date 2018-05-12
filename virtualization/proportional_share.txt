Lottery Scheduling

    Proportional share

A pool of tickets are allocated to each process

Process A has 75 tickets, B has 25 tickets i.e. A has 75% chance of being selected

    Ticket Currency:

Process A can hand out more tickets to its child processes, e.g.
Process A1 and A2 are being handed 200 and 100 tickets respectively
converting to A original "currency", A1 has 50 tickets, A2 has 25 tickets.

    Ticket Transfer

Process can transfer tickets to another e.g.
when client process asks server process to do some work, 
client process gives its own tickets to server to improve chance of cpu working on it.
When work is done, server process returns the ticket to client

    Ticket inflation

a group of processes which trust each other can share their tickets to a process which needs more cpu time

    Unfairness metric U

U = T-job-A-duration / T-job-B-completion

The shorter the the jobs, the more unfair they get


    Stride Scheduling

Pass(A) Pass(B) Pass(C) Who Runs?
(stride=100) (stride=200) (stride=40)
0 0 0 A
100 0 0 B
100 200 0 C
100 200 40 C
100 200 80 C
100 200 120 A
200 200 120 C
200 200 160 C
200 200 200 ...

Start with three processes (A, B, and C), with stride values of 100, 200, and 40, 
and all with pass values initially at 0. 

Thus, at first, any of the processes might run, as their pass values are equally low.

Assume we pick A (arbitrarily; any of the processes with equal low pass
values can be chosen). A runs; when finished with the time slice, we
update its pass value to 100. 

Then we run B, whose pass value is then set to 200. 
Finally, we run C, whose pass value is incremented to 40. 
At this point, the algorithm will pick the lowest pass value, which is C’s, and
run it, updating its pass to 80 (C’s stride is 40, as you recall). 
Then C will run again (still the lowest pass value), raising its pass to 120. 

A will run now, updating its pass to 200 (now equal to B’s). Then C will run twice
more, updating its pass to 160 then 200. At this point, all pass values are
equal again, and the process will repeat, ad infinitum.

Summary

both lottery and stride have not achieved wide-spread adoption as CPU schedulers
for a variety of reasons. One is that such approaches do not particularly
mesh well with I/O [AC97]; another is that they leave open the hard problem
of ticket assignment, i.e., how do you know how many tickets your
browser should be allocated?

