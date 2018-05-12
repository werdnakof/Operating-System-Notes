HOW TO CREATE AND CONTROL PROCESSES

What interfaces should the OS present for process creation and control?
How should these interfaces be designed to enable ease of use as
well as utility?

                                        fork() system call


see p1.c

The process calls the fork() which the OS provides as a way to create a new process. 

The process created is an (almost) exact copy of the calling process.

It now looks like there are two copies of the program p1 running, and both are about to return from the fork() system call.

The child isn’t an exact copy. It now has its own copy of the address space i.e. its own private memory, its own registers, its own PC, etc. 

The value it returns to the caller of fork() is different. 

While the parent receives the PID of the newly-created child, the child receives a return code of zero.

This differentiation is useful, because it is simple then to write the code that handles the two different cases (as above).

The CPU scheduler then decides which process to run first.


                                        wait() System Call

see p2.c

A parent can wait for a child process to finish what it has been doing. 

This task is accomplished with the wait() or waitpid() system call.

So if child is prefered to run first before parent, in the case when parent was schedulered to
run first, parent can call wait(), then the parent process waits, until child has run and finished;


                                        exec() System Call


fork() is only useful when you want keep running copies of the same program,

To start a different program, use exec() or its variants such as execvp()
https://stackoverflow.com/questions/5769734/what-are-the-different-versions-of-exec-used-for-in-c-and-c

What it does: 

Given the name of an executable (e.g., wc),and some arguments (e.g., p3.c), 

It loads code and static data from that executable and overwrites its current code segment (and current static data) with it; 

The heap and stack and other parts of the memory space of the program are re-initialized. 

Then the OS simply runs that program, passing in any arguments as the argv of that process. 
Thus, it does not create a new process; rather, it transforms the currently running program (formerly p3) into a different running program (wc).

The separation of fork() and exec() is essential in building a UNIX shell, because it lets the shell run code after the call to fork() but before the call to exec(); this code can alter the
environment of the about-to-be-run program, and thus enables a variety of interesting features to be readily built.


