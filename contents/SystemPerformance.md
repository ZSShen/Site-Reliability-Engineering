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

2. Answer the following conceptual questions:
    + What is the purpose of demand paging?
      + For performance consuer. In a system that uses demand paging, the operating system copies a disk page into physical memory only if an attempt is made to access it and that page is not already in memory.
    + Describe memory utilization and saturation.
      + Memory utilization can be calculated as used memory versus total memory. If demands for memory exceed the amount of main memory, main memory becomes saturated.
    + What is the purpose of the MMU and the TLB?
      + The memory management unit (MMU) is responsible for virtual-to-physical address translations. The translation lookaside buffer (TLB) is a small set-associative hardware cache in MMU that speeds up virtual-to-physicall address mapping.
    + What is the role of the page-out daemon?
    + What is the role of the OOM killer?
      + The out-of-memory killer will free memory by finding and killing a sacrificial process, found using `select_bad_process()` and then killed by calling `oom_kill_process()`. This may be logged in the system log (`/var/log/messages`) as an "Out of memory: Kill process" message.