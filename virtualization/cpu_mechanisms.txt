
                               Limited Direct Execution Protocal

    "Direct Excution"

When the OS runs program, it creates a process entry for it in a process list, 
allocates some memory for it, loads the program code into memory (from disk), locates its entry point (i.e., the main() routine or something similar), and later to free memory, and remove it from process list

    "Limited" - restricting operations in programs

2 modes:

user mode - code that runs in user mode is restricted in what it can do. 
(E.g. when running in user mode, a process can’t issue I/O requests)

kernel mode - code that runs can do what it likes

In user mode, to perform privileged operations e.g. reading from disk, 
all modern hardware provides the ability for user programs to perform a system-call.

    System-Call

A System-Call is a programmatic way for programs to request a service from kernel of OS.

A Kernel is core component within an OS which handles various resource management, 
(hence in kernel mode it is free to configure machine hardware as need be.)

To execute a system-call, a program must execute a "Trap"

A system-call number is usually assigned by the usto specify the exact system-call. 

User program code is responsible for placing a system-call number!

Then it jumps into the kernel and raises the privilege level to kernel mode,
then the system can now perform privileged operations are needed (if allowed).

When finished, the OS calls a "return-from-trap" instruction, 

This returns into the calling user's program and simultaneously reduce the privilege level back to user mode.

On x86, the processor will push the program counter, flags, and a few other registers onto a "per-process" kernel stack; the "return-fromtrap" will pop these values off the stack and resume execution of the usermode program

    How does the trap know which code to run? 

When the machine boots up, it does so in privileged (kernel) mode

Kernel also tells the hardware various locations of "trap handlers".

Trap-handler informd the hardware what code to run when certain exceptional events occur. 


            How to regain control of the CPU so it can switch between processes?

  
  A Cooperative Approach: Wait For System Calls

Most processes, as it turns out, transfer control of the CPU to the OS quite frequently by making system calls, e.g. to open a file and subsequently read it, or to send a message to another
machine, or to create a new process.


   A Non-Cooperative Approach: The OS Takes Control

A timer interrupt can be used in the case when all processes refuse to give up control.

A timer device can be programmed to raise an interrupt every
so many milliseconds; when the interrupt is raised, the currently running
process is halted, and a pre-configured interrupt handler in the OS runs.

OS must inform the hardware of which code to run when the timer interrupt occurs; thus,
at boot time, the OS does exactly that. Second, also during the boot sequence, the OS must start the timer, which is of course a privileged operation. Once the timer has begun, the OS can thus feel safe in that control will eventually be returned to it

When OS regains control, whether cooperatively via a system
call, or more forcefully via a timer interrupt, a decision has to be
made by the scheduler: whether to continue running the currently-running process, or
switch to a different one.

If the decision is made to switch, the OS then executes a low-level
piece of code which we refer to as a context switch. A context switch is
conceptually simple: all the OS has to do is save a few register values
for the currently-executing process (onto its kernel stack, for example)
and restore a few for the soon-to-be-executing process (from its kernel
stack)




