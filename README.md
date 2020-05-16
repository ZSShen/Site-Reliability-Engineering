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
[Writeup of the Book: System Performance, Enterprise, and the Cloud](./contents/SystemPerformance.md)


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
