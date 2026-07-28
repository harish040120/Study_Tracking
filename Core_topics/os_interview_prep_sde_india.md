# Operating Systems Interview Preparation Checklist for SDE Roles in India

Use this checklist to cover the Operating Systems concepts most commonly asked in software engineering interviews, especially around process management, memory, scheduling, synchronization, and file systems.[cite:4][cite:7]

## Core Foundations
-  What an operating system does
-  Types of operating systems: batch, time-sharing, distributed, real-time, multitasking
-  User mode vs kernel mode
-  Kernel basics: monolithic kernel, microkernel, modular kernel
-  System calls and traps
-  Interrupts and context switching
-  Boot process basics

## Processes and Threads
- [ ] Program vs process
- [ ] Process states and process lifecycle
- [ ] Process Control Block (PCB)
- [ ] Context switch: what happens and why it is costly
- [ ] Thread vs process
- [ ] User-level threads vs kernel-level threads
- [ ] Multithreading benefits and challenges
- [ ] Orphan process and zombie process
- [ ] Daemon process
- [ ] Dispatcher and scheduler basics

## CPU Scheduling
- [ ] Scheduling goals: throughput, turnaround time, waiting time, response time, fairness
- [ ] Preemptive vs non-preemptive scheduling
- [ ] First Come First Serve (FCFS)
- [ ] Shortest Job First (SJF)
- [ ] Shortest Remaining Time First (SRTF)
- [ ] Priority scheduling
- [ ] Round Robin scheduling
- [ ] Multilevel queue scheduling
- [ ] Multilevel feedback queue scheduling
- [ ] Starvation and aging
- [ ] Practical trade-offs between scheduling algorithms

## Synchronization and Concurrency
- [ ] Race condition
- [ ] Critical section problem
- [ ] Requirements of a correct critical section solution
- [ ] Mutex
- [ ] Semaphore: binary and counting
- [ ] Spinlock
- [ ] Monitor concept
- [ ] Producer-consumer problem
- [ ] Readers-writers problem
- [ ] Dining philosophers problem
- [ ] Busy waiting
- [ ] Atomic operations and test-and-set idea

## Deadlocks
- [ ] What deadlock is
- [ ] Four necessary conditions of deadlock
- [ ] Resource allocation graph
- [ ] Deadlock prevention
- [ ] Deadlock avoidance
- [ ] Banker's algorithm
- [ ] Deadlock detection
- [ ] Deadlock recovery
- [ ] Difference between deadlock, starvation, and livelock

## Memory Management
- [ ] Logical address vs physical address
- [ ] Address binding
- [ ] Contiguous memory allocation
- [ ] Fragmentation: internal vs external
- [ ] Paging
- [ ] Segmentation
- [ ] Segmentation with paging
- [ ] Virtual memory
- [ ] Demand paging
- [ ] Swapping
- [ ] Thrashing
- [ ] Locality of reference

## Page Replacement
- [ ] Page fault
- [ ] FIFO page replacement
- [ ] LRU page replacement
- [ ] Optimal page replacement
- [ ] Second chance / clock algorithm
- [ ] Belady's anomaly
- [ ] Working set idea

## Inter-Process Communication
- [ ] Shared memory
- [ ] Message passing
- [ ] Pipes and named pipes
- [ ] Sockets basics
- [ ] Signals basics
- [ ] IPC trade-offs and use cases

## File Systems and Storage
- [ ] File concept and file attributes
- [ ] File operations
- [ ] Directory structures
- [ ] File allocation methods
- [ ] Free space management
- [ ] Access methods
- [ ] Inode basics
- [ ] Journaling basics
- [ ] File system consistency
- [ ] Mounting basics
- [ ] RAID basics and RAID levels overview

## Disk Management and I/O
- [ ] Disk structure basics
- [ ] Disk scheduling goals
- [ ] FCFS disk scheduling
- [ ] SSTF disk scheduling
- [ ] SCAN and C-SCAN
- [ ] LOOK and C-LOOK
- [ ] Buffering
- [ ] Caching
- [ ] Spooling
- [ ] DMA basics

## Virtualization and Protection
- [ ] Protection vs security
- [ ] Access control basics
- [ ] Authentication vs authorization
- [ ] Memory protection
- [ ] Privileged instructions
- [ ] Virtual machines and hypervisor basics
- [ ] Containers vs virtual machines at a high level

## Important OS Terms Often Asked Directly
- [ ] Kernel
- [ ] Shell
- [ ] Throughput
- [ ] Turnaround time
- [ ] Waiting time
- [ ] Response time
- [ ] Multiprogramming
- [ ] Multitasking
- [ ] Multiprocessing
- [ ] Multithreading
- [ ] Reentrancy
- [ ] Trap
- [ ] Interrupt
- [ ] Buffer
- [ ] Cache
- [ ] Spooling
- [ ] Thrashing
- [ ] Overlay

## Linux-Oriented Basics for SDE Interviews
- [ ] Process IDs and parent-child relationship
- [ ] Common commands: ps, top, kill, grep, chmod, chown, ls, df, du
- [ ] File permissions basics
- [ ] Foreground vs background process
- [ ] Signals like SIGKILL and SIGTERM
- [ ] Basic shell redirection and pipes
- [ ] Difference between hard link and soft link

## Interview-Focused Practice
- [ ] Prepare definitions in 2-3 lines for core terms
- [ ] Practice process vs thread, mutex vs semaphore, paging vs segmentation, deadlock vs starvation, user thread vs kernel thread
- [ ] Solve scheduling numericals for FCFS, SJF, SRTF, and Round Robin
- [ ] Solve page replacement numericals for FIFO, LRU, and Optimal
- [ ] Practice deadlock and Banker's algorithm examples
- [ ] Be ready with real-world examples for synchronization and IPC
- [ ] Revise OS topics together with DBMS, CN, and OOP for placements

## High-Priority Revision Order
- [ ] Processes and threads
- [ ] CPU scheduling
- [ ] Synchronization, mutex, semaphore, critical section
- [ ] Deadlocks
- [ ] Paging, segmentation, virtual memory, thrashing
- [ ] Page replacement algorithms
- [ ] IPC
- [ ] File systems and disk scheduling
- [ ] Linux basics used in development environments

## How to Study This Efficiently
- [ ] First pass: learn definitions and differences
- [ ] Second pass: understand algorithms and problem-solving steps
- [ ] Third pass: practice interview questions aloud
- [ ] Fourth pass: solve previous interview and placement questions
- [ ] Final pass: create a one-page quick revision sheet
