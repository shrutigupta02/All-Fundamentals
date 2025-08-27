What is OS: System software that serves as an interface between hardware and software

###### Types:
1. Batch: groups similar tasks in batches and batch wise execution
2. Multi-programming OS: multiple programs run in memory , cpu is never left idle. doesnt execute simultaneously but as soon as program 1 needs to wait for input, cpu transfers to another program and so on
3. Multi-tasking / Time sharing OS: every task is run in round robin manner, used IRL
4. Multi-processing: uses multiple cpus to execute program
5. Distributed: multiple autonomous integrated computers are spread over the world
6. RTOS: the time-interval called quantum is very small in these. two types: hard -> critical systems and soft: gaming etc
###### OS Structures Types:
1. SImple / Monolithic: one single large kernel. eg: linus
2. Microkernel : multiple small kernels. eg: MINMX
3. Hybrid: mix of both. eg: windows mac
4. Modular: Loadable kernel modules
5. Hierarchal: layered kernels

System calls: interface between software programs and OS kernel
Eg: malloc(), read(), write()
##### Processes and Threads:

Programs: Set of instructions
**Process: program in execution is a process**

Process Control Blocks: used to maintain all the necessary info about each process like their number, counter, i/o state

Process States:
1. New
2. Ready
3. Waiting
4. Running
5. Terminated

Three types of Queue:
1. Job queue: all jobs in OS
2. Ready queue: all processes ready to be run
3. Devices queue: list of processes waiting to access that i/o device

###### Threads:
called lightweight processes because they share similarities with processes but are smaller, take less time and resources

threads are usually a part of a process

threads share data with each other

can be user defined

three states: ready, running, blocked

##### Process Scheduling:
What is a scheduler: schedulers decide which manage the transfer of processes from one queue to another

3 types:
1. Long term: job to ready
2. Medium term: Swaps from ready to job to make room for other processes whose I/O resources etc are better available
3. Short term: ready queue to execution

Dispatcher: actually **facilitates** the transfer of jobs from queues 

Types of processes: cpu bound and i/o bound
###### Types of proc scheduling:
1. Preemptive: can pause the execution of current process if a higher priority process comes or assigned time expires (as in round robin)
2. Non preemptive: only pauses the execution if either process ends or process goes to waiting state for i/o
###### Scheduling Concepts
- **CPU Burst**: Time process uses CPU
- **I/O Burst**: Time process waits for I/O
- **Dispatcher**: Gives CPU control to selected process
- **Context Switch**: Overhead of switching between processes

###### Scheduling criteria:
1. cpu usage
2. total turnaround time (waiting time + execution time)
3. response time
4. wait time
5. throughput

##### Scheduling Algo:
1. FCFS: fifo queue, non preemptive
2. Shortest Job First: preemptive (SRT) and non preemptive both, but not realistic because irl we do not know time of every process
3. Priority Based: preemptive and non preemptive, can cause starvation
4. Round Robin: assigns a quantum/interval for each process. preemptive
5. Multilevel queues: multiple different queues for each process types and diff sched algos per queue. preemptive non preemptive
6. Multilevel feedback queues: same as above but processes can move between queues after their quantum

Aging: If a process remains in a low-priority queue for an extended period, it may be moved to a higher-priority queue to prevent starvation

##### Synchronisation:

Critical Section: code segment in a process which needs to access shared resources

Race Condition: when the output / data depends on the order of execution. changing the order of execution of processes changes the output.

Sync:
1. Initial section: process is accessing private resources only
2. Entry Section: process asks for permission to enter critical section
3. Critical Section: accessing shared resources
4. Exit section: releases permission over shared resources
5. Remainder section: remaining code

Criteria for sync:
1. Mutual Exclusion: no two processes should enter critical section at the same time
2. Progress: if no processes are in critical section then only those processes that are not in remainder section be sent to critical section
3. Bounded wait (optional) : a max number allotted where that number of processes are allowed to enter the critical section after another process has requested access 

Peterson Solution: devised a solution for 2 processes, satisfying all three criteria and uses flag variables

**Semaphores** gives n-process solution, send "wait()" before entering critical section and "signal()" after exiting critical section. Doesn't guarantee bounded wait

###### Problems:
1. Producer-Consumer problem: one producer adds items in DS, one consumer removes, use three semaphores to solve this problem
2. Readers-Writers: multiple readers and multiple writers, use mutually exclusive locks
3. Dining Philosopher: there is a round table where many philosophers are sitting. each philosopher share a chopstick from their adjacent philosopher and require both the chopsticks to eat. they can either eat or think. they release chopsticks after they're done eating. so chopsticks are like shared resources. we use this problem to check deadlock prevention. can be solved using semaphores

##### Deadlock:
situation in computing where two or more processes are unable to proceed because each is waiting for the other to release resources.

Conditions necessary for a deadlock: all four simult result in deadlock
1. Mutually exclusive resouces: ek hee process ek baar me le sakte hai
2. Hold and wait
3. No preemption
4. Circular wait: set of processes are waiting in circular manner

###### Deadlock prevention:
1. Prevention: 
   - maintain shareable resources without mutual exclusion eg read-only lock
   - disable hold and wait, when a process requests for a resource it needs to release all held resources
   - start preemption
   - disable circular wait by ordering of resource acquisition 
2.  Avoidance: bankers algo, but the disadvantage is that we need to know all the processes and resources required by them in advance
3. Detection: let the deadlock occur as there is little chance of deadlock happening so why make our system slow trying to prevent/avoid it. let the system enter a deadlock and when it does, detect it using banker's algo and perform recovery using methods like process termination, manual intervention etc
4. Ignorance: If a deadlock is very rare, then let it happen and reboot the system. this is what UNIX and Windows use

##### Memory

Hierarchy:
1. Register
2. Cache
3. Main memo / RAM
4. Magnetic Disk

###### Space Allocation
1. Variable Sized Partitioning: jitna chahiye utna assign kardo
2. Fixed Sized Partitioning: divide memo in fixed parts and assign most suitable one

Allocation policies:
1. First fit
2. Best fit
3. Worst fit

Fragmentation: unusable small segments of memory
1. Internal: Internal fragmentation occurs when there is unused space within a memory block. For example, if a system allocates a 64KB block of memory to store a file that is only 40KB in size, that block will contain 24KB of internal fragmentation.
2. External: Lack of contiguous memory space for large processes because memory is divided into multiple parts. Eg: a hotel has four tables of 4 people each hence can store 4 processes of 4 people each but if one large process comes of 16 people then, it cannot be allocated even tho hotel has space for 16

###### Paging: 
page: small parts of a program
paging is the process of moving pages from secondary memory to main memory

paging helps avoid external fragmentation by eliminating the need for contiguous memory

frame in main memo refered by a page in secondary memo

###### Segmentation:
divides program into variable-size logical segments instead of pages
suffers from external fragmentation
###### SEGMENTATION + PAGING
hybrid approach combining benefits of both segmentation and paging.
How:
Program divided into segments (logical view)
Each segment divided into pages (physical implementation)
Two-level address translation: Segment → Page → Frame

###### VIRTUAL MEMORY
What: Memory management technique that gives illusion of larger main memory by using secondary storage as extension of RAM.
How:
Only portions of program loaded in memory
Pages/segments loaded on demand
Swapping between main memory and disk

**Demand Paging**: Virtual memory implementation where pages are loaded into memory only when accessed (on-demand).

Uses lazy-loading strategy
Process:
1. Page fault occurs when accessing non-resident page
2. OS handles interrupt, loads page from storage
3. Updates page table, restarts instruction

###### Page Replacement Algos:
1. FIFO: remove oldest page in memory
2. Optimal: Replace page that will be used farthest in future
3. LRU: least recently used

**Thrashing:** performance degradation due to excessive page faults
- Insufficient memory allocated to processes
- Processes spend more time paging than executing
- High page fault rate, low CPU utilization

