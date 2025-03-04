# Review

??? note "Review Lesson"

    ## Computer Architecture

    ### Von Neumann Architecture

    CPU, Memory, I/O devices

    ## OS

    mode bit:

    - unprivileged mode: user mode
    - privileged mode: kernel mode, run any instruction

    ### Timer

    interrupt the computer regularly

    ### OS Structure

    #### System Calls

    interface between user and kernel

    store and restore user context

    #### System Service

    Loader call syscall to setup library

    ## Process

    Process is resource and execution isolated unit.

    Thread is execution isolated unit.

    ### Process Control Block (PCB)

    store process information

    ### Process State

    Context Switch

    ## Scheduling

    Gantt Chart:

    - Average Waiting Time: Start Time - Arrival Time
    - Average Turnaround Time: End Time - Arrival Time

    ## IPC

    - Shared Memory
    - Message Passing

    ## Thread

    pc, sp, page table

    One-to-One Model: one thread to one kernel thread

    ## Synchronization

    ### Peterson's Algorithm

    ### Hardware Instruction

    - test and set
    - compare and swap

    ### Mutex Lock

    ### Semaphore

    ### Examples

    - Bounded Buffer Problem

    ### Deadlock

    ## Memory Management

    ### Partition

    Base address, limit address

    - Fixed Partition: internal fragmentation
    - Variable Partition: external fragmentation

    ### Segmentation

    multiple set of sections with same privilege

    Segment Table: Base and Limit

    ### Paging

    - page: virtual memory
    - frame: physical memory

    Page Table: Physical Frame Number + Perms + Valid Bit

    ## Virtual Memory

    ### Demand Paging

    ### Copy-on-Write

    ### Page Replacement

    ## IO System

    ### Polling

    ### Interrupt

    ## File System

    ### File Control Block

    ### Virtual File System

    Define common interface for different file system

    Directory Entry: Path to inode

    ### Disk Block Allocation

    - Contiguous Allocation
    - Linked Allocation
    - Indexed Allocation

    ext2: store index in inode (direct, indirect, double indirect, triple indirect)