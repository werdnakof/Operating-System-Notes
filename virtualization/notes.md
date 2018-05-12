Role Operating system

Takes physical resources, such as a CPU, memory, or disk, and virtualizes them. 
It handles tough and tricky issues related to concurrency. 
And it stores files persistently, thus making them safe over the long-term.

Designing and implementing an operating system is to minimize the overheads of the OS.

Takes a physical resource (CPU, memory or disk) and transforms it into a more general, powerful, and easy-to-use virtual form of itself.

Provides interfaces (APIs) for running programs, accessing memory and devices, and other related actions

Allows multiple programs to run concurrently, and hence act as a "Resource Manager" for each program i.e. handling access to shared memory and devices

CPU, memory, disk are Resouces of the system, OS manages them efficiently

OS virtualizes many CPUs to be used for each program.

###

Virtualizing Memory

Physical Memory is just an array of bytes; to read memory, 
one must specify an address to be able to access the data stored there; 
to write (or update) memory, one must also specify the data to be written to the given address

Each process has its own virtual address space, OS maps it onto physical memory of the machine

So even though two process have the same virtual address space, it doesn't matter

Concurrency

managing resource concsistency between processes

Persistence

File System - manages the disk

Unlike the abstractions provided by the OS for the CPU and memory,
the OS does not create a private, virtualized disk for each application.

###

                           HOW TO PROVIDE THE ILLUSION OF MANY CPUS?

  Time Sharing

By running one process, then stopping it and running another, and so forth, the OS can
promote the illusion that many virtual CPUs exist when in fact there is
only one physical CPU (or a few).


  Space sharing

A resource is divided (in space) among those who wish to use it. For example, disk space is naturally a spaceshared resource; once a block is assigned to a file, it is normally not assigned
to another file until the user deletes the original file.


  Policies 

They are algorithms for making some kind of decision within the OS. 
For example, given a number of possible programs to run on a CPU, which program should the OS run? 

A scheduling policy in the OS will make this decision, likely using historical information (e.g., which program has run more over the last minute?), workload knowledge (e.g., what types of programs are run), and performance metrics (e.g., is the system optimizing for interactive performance, or throughput?) to make its decision.


  Process

The abstractions provided by the OS of a running program is called a process.

Within each process consists of a Machine State - what resources it can access
- memory and its addresses
- registers
- stack pointers and associated frame pointers (used to manage the stack for function parameters, local variables, and return addresses)
- IO devices

https://softwareengineering.stackexchange.com/questions/304244/i-understand-what-a-stack-pointer-is-but-what-is-it-used-for


  Process Creation

Programs i.e. code and static data initially reside on disk

OS loads its code and any static data from disk into memory, into the address space of the process.

Some memory must be allocated for the program’s run-time stack

C programs use the stack for local variables, function parameters, and return addresses; 

OS also initializes the stack with arguments; specifically, it will fill in the parameters to the main() function, i.e., argc and the argv array.

OS allocates memory for the program’s heap. 

In C programs, the heap is used for explicitly requested dynamically-allocated data; programs request such space by calling malloc() and free it explicitly by calling free(). The heap is needed for data structures such as linked lists, hash tables, trees, and other interesting data structures.

in UNIX systems, each process by default has three open file descriptors, for standard input, output, and error; these descriptors let programs easily read input from the terminal
as well as print output to the screen.


  Process States

Running (process is running on a processor.)
Ready (ready to run, OS might not run it right now)
Blocked (a process has stalled after an blocking operation until some other event takes place)
etc

  Data Structures

OS use data structures to keep track of process states, below is an example

// the registers xv6 kernel will save 
// and restore to stop and subsequently restart a process
struct context {
  int eip;
  int esp;
  int ebx;
  int ecx;
  int edx;
  int esi;
  int edi;
  int ebp;
};

// the different states a process can be in
enum proc_state { UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE };

// the information xv6 tracks about each process including its register context and state
struct proc {
  char *mem; // Start of process memory
  uint sz; // Size of process memory
  char *kstack; // Bottom of kernel stack for this process
  enum proc_state state; // Process state
  int pid; // Process ID
  struct proc *parent; // Parent process
  void *chan; // If non-zero, sleeping on chan
  int killed; // If non-zero, have been killed
  struct file *ofile[NOFILE]; // Open files
  struct inode *cwd; // Current directory
  struct context context; // Switch here to run process
  struct trapframe *tf; // Trap frame for the current interrupt
};



