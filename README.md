# Facebook-Production-Engineer

## Mindset
Try to quantify the syndrome or confirm issues with the customers before deep diving into analysis.

1. Problem Statement Method
   - what makes you **think** there is a performance problem?
   - Has this system **ever** performed well?
   - What has **changed** recently? (Software? Hardware? Load?)
   - Can the performance degradation be expressed in terms of **latency** or runtime?
   - Does the problem affect other people or applications? (Or is it just you?)
   - What is the **environment**?
2.


## Understanding Linux
### Linux Performance Analysis in 60 seconds
1. Check load average
   ```sh
   $ uptime
   ```
2. Check kernel errors
   ```sh
   $ dmesg -T | tail
   ```
3. Check overall stats by time
   ```sh
   $ vmstat 1
   ```
4. Check CPU balance
   ```sh
   $ mpstat -P ALL 1
   ```
5. Check process usage
   ```sh
   $ pidstat  1
   ```
6. Check disk I/O
   ```sh
   $ iostat -xz 1
   ```
7. Check memory usage
   ```sh
   $ free -m
   ```
8. Check network I/O
   ```sh
   $ sar -n DEV 1
   ```
9. Check TCP stats
    ```sh
    $ sar -n TCP,ETCP 1
    ```
10. Overview
    ```sh
    $ top
    ```


## System Performance, Enterprise, and the Cloud
### Chapter 3 Operating System
1. Answer the following questions about OS terminology.
    + What is the difference between a process, a thread, and a task?
    + What is a context switch?
        + In a multitasking context, it refers to the process of storing the system state for one task, so that task can be paused and another task resumed.
    + What is the difference between paging and swapping?
        + In traditional UNIX, **swapping** moves entire processes between main memory and secondary storage, while **paging** moves small units of memory called pages. In Linux, the term swapping is used to refer to paging. The Linux kernel does not support the conventional UNIX-style swapping of entire threads and processes.
    + What is the difference between I/O-bound and CPU-bound workloads?

2. Answer the following conceptual questions:
    + Describe the role of the kernel.
    + Describe the role of system calls.
    + Describe the role of VFS and its location in the I/O stack.

3. Answer the following deeper questions:
    + List the reasons why a thread would leave the CPU.
    + Describe the advantages of virtual memory and demand paging.

### Chapter 4 Observability Tools
1. Answer the following questions about observability tool terminology:
    + What is counter?
        + Kernels maintain various statistics, called counters, for counting events. They are usually implemented as unsigned integers that are incremented when events occur. Many kinds of counter monitors are supported in Linux. These tools typically read statistics from the `/proc` file system.
        + System-Wide:
            + **vmstat**: Virtual and physical memory statistics, system-wide.
            + **mpstat**: Per-CPU usage.
            + **iostat**: Per-disk I/O usage, reported from the **block device** interface.
            + **netstat**: Network interface statistics, TCP/IP stack statistics, and some per-connection statistics.
            + **sar**: Various statistics; can also archive them for historical reporting.
        + Per-Process:
            + **ps**: Process status, shows various process statistics, including memory and CPU usage.
            + **top**: Shows top processes, sorted by one of the statistics such as CPU usage.
            + **pmap**: Lists process memory segments with usage statistics.

    + What is tracing?
        + Tracing collects **per-event data** for analysis. Tracing frameworks are not typically enabled by default, since tracing incurs CPU overhead to capture the data and can require significant storage to save it. These overheads can slow the target of tracing and need to be accounted for when interpreting measured times.
        + System-Wide:
            + **tcpdump**: network packet tracing (uses libpcap).
            + **perf**: Linux Performance Events, tracing static and dynamic probes.
        + Per-Process:
            + **strace**: system call tracing for Linux-based systems.

    + What is the difference between static and dynamic tracing?

    + What is profling?
        + Profiling characterizes the target by collecting **a set of samples or snapshots** of its behavior. CPU usage is a common example, where samples are taken of the program counter or stack trace to characterize the code paths that are consuming CPU cycles. These samples are usually collected at a fixed rate, such as 100 or 1,000 Hz (cycles per second) across all CPUs.
        + System-Wide and Per-Process:
            + **perf**: A Linux performance toolkit, which includes profiling subcommands.

### Chapter 5 Applications
1. Answer the following questions about terminology:
    + What is a cache?
        + Operating systems use caches to improve file system read performance and memory allocation performance; applications often use caches for a similar reason.

    + What is a buffer?
        + To improve write performance, data may be coalesced in a buffer before being sent to the next level. This increases the I/O size and efficiency of the operation.

    + What is a ring buffer?
        + A type of fixed buffer that can be used for continuous transfer between components, which act upon the buffer asynchronously.

    + What is a spin lock?
        + Spin locks allow the holder to operate, while others requiring the lock spin on-CPU in a tight loop, checking for the lock to be released.

    + What is an adaptive mutex lock?

    + What is the difference between concurrency and parallelism?
        + Concurrency is when two or more tasks can start, run, and complete in overlapping time periods. It doesn't necessarily mean they'll ever both be running at the same instant. For example, multitasking on a single-core machine. Parallelism is when tasks literally run at the same time, e.g., on a multicore processor.

    + What is CPU affinity?
        + The ability in Linux to bind one or more processes to one or more processors. This can improve the memory locality of the application, reducing the cycles for memory I/O and improving overall application performance.

2. Answer the following conceptual questions:
    + What are the general pros and cons of using a large I/O size?
        + Initialization tax is paid for small and large I/O alike. For efficiency, the more data transferred by each I/O, the better. For example, it’s usually much more efficient to transfer 128 Kbytes as a single I/O than as 128 x 1 Kbyte I/O.
    + Explain the role of garbage collection and how it can affect performance.

### Chapter 7 Memory
1. Answer the following questions about memory terminology:
    + What is a page of memory?
        + A unit of memory, as used by the OS and CPUs. Historically it is either 4 or 8 Kbytes. Modern processors have multiple page size support for larger sizes.
    + What is resident memory?
        + Memory that currently resides in main memory.
    + What is virtual memory?
        + An abstraction of main memory that is (almost) infinite and noncontended.
    + Using Unix terminology, what is the difference between paging and swapping?
    + Using Linux terminology, what is the difference between paging and swapping?



## Operating System Knowledge
### Process Control Block
Information associated with each process
+ Process state: running, waiting, etc.
+ Program counter: location of instruction to next execute.
+ CPU registers: contents of all process-centric registers.
+ CPU scheduling information: priorities, scheduling queue pointers.
+ Memory-management information: memory allocated to the process.
+ Accounting information: CPU used, clock time elapsed since start, time limits.
+ I/O status information: I/O devices allocated to process, list of open files.

### Zombie and Orphan
+ Zombie: A process that has terminated, but whose parent has not yet called `wait()`. when a child exists, some process must wait on it to get its exit code. ​zombies​ only occupy space in the process table, take no memory or CPU. However, process table is a finite resource, excessive zombine can fill it up and no other processes can launch.

+ Orphan: A process that is running, but whose parent has terminated. Orphans are adopted by `init`.

### Swap Space
The primary function of swap space is to substitute disk space for RAM memory when real RAM fills up and more space is needed.

### Trashing
Thrashing can occur when total virtual memory, both RAM and swap space, become nearly full. The system spends so much time paging blocks of memory between swap space and RAM and back that little time is left for real work. The typical symptoms of this are obvious: The system becomes slow or completely unresponsive, and the hard drive activity light is on almost constantly.

### RAID
RAID stands for **Redundant Array of Independent Disks**. It's idea is to  spread data over multiple drives in parallel to get higher throughput while using parity for robustness.
+ **Raid 0**: Stripe data across drives for improved throughput.
    + No extra redundancy, elevated risk.
+ **Raid 1**: Mirroring, parallel data to multiple devices for robustness.
    + No extra throughput.
+ **Raid 2**: Use Hamming codes for parity.
    + Requires log2 parity bits.
    + Really expensive.
+ **Raid 3**: Bitwise parity on parity disk.
    + Requires only one parity disk for N storage disks.
    + Bitwise parity is slow, dedicated parity disk is bottleneck.
+ **Raid 4**: Similar to Raid 3, but blockwise parity improves performance.
+ **Raid 5**: Rotating parity block among disks relieves bottleneck.
+ **Raid 6**: Raid5 with dual parity.
    + Supports up to 2 HDD failures.
    + Slow rebuild.

## Bash Knowledge
## Special Variable
| Variable | Description |
|---|---|
|$!| PID (process ID) of last job run in background. |
|$?| Exit status of a command, function, or the script itself. |
|$$| PID (Process ID) of the script itself. |
