# pcheck
Linux performance diagnostic tool

download and run ./pcheck.sh

it will output similar to this:


=====================================================================
      Comprehensive CentOS Performance Diagnostic Script
=====================================================================
Timestamp: Tue Jun 10 09:16:36 EDT 2025
Interpreting Colors: [0;31mRed (Critical),[1;33m Yellow (Warning),[0;36m Cyan (Info)[0m

## Part 1: System-Wide Overview ##

[0;36m## 1. Uptime and Load Average ##[0m
-> Checking system load against the number of CPU cores.
   Number of CPU cores on this system: 2
 09:16:36 up  1:36,  2 users,  load average: 0.02, 0.01, 0.00
   INFO: Load average is within a normal range.

[0;36m## 2. Kernel Ring Buffer (dmesg) ##[0m
-> Checking for critical kernel-level errors (I/O, Out of Memory).
[ 1674.261421] vboxsf: SHFL_FN_MAP_FOLDER [01;31m[Kfail[m[Ked for '/mnt/shared': share not found

[0;36m## 3. Overall Memory Usage (free) ##[0m
-> Checking available memory and swap usage.
              total        used        free      shared  buff/cache   available (values in MB)
Mem:    7697    362     6679    8       656     7083   
Swap:   2047    0 2047

## Part 2: Resource Bottleneck Identification ##
-> Identifying *what* kind of resource is under pressure.

[0;36m## 4. Virtual Memory & I/O Wait (vmstat) ##[0m
-> High 'wa' (wait I/O) means CPU is waiting for disk. High 'si'/'so' means swapping.
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 6840104   3164 668600    0    0    60     3  120  109  0  2 98  0  0
 0  0      0 6839868   3164 668600    0    0     0     0  189  176  0  1 99  0  0
 0  0      0 6839868   3164 668600    0    0     0     0  300  200  0  1 99  0  0
 2  0      0 6839868   3164 668600    0    0     0     0  161  216  0  1 99  0  0
 1  0      0 6839868   3164 668600    0    0     0     0  123  159  0  1 100  0  0

[0;36m## 5. CPU and I/O Wait (mpstat) ##[0m
-> High '%iowait' indicates an I/O bottleneck. Low '%idle' indicates a CPU bottleneck.
Linux 4.18.0-553.6.1.el8.x86_64 (vbox) 	06/10/2025 	_x86_64_	(2 CPU)

09:16:40 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
09:16:41 AM  all    0.00    0.00    0.00    0.00    1.00    0.00    0.00    0.00    0.00   99.00
09:16:41 AM    0    0.00    0.00    0.00    0.00    1.98    0.00    0.00    0.00    0.00   98.02
09:16:41 AM    1    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00

09:16:41 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
09:16:42 AM  all    0.00    0.00    0.00    0.00    1.01    0.00    0.00    0.00    0.00   98.99
09:16:42 AM    0    0.00    0.00    0.00    0.00    1.01    0.00    0.00    0.00    0.00   98.99
09:16:42 AM    1    0.00    0.00    0.00    0.00    1.00    0.00    0.00    0.00    0.00   99.00

09:16:42 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
09:16:43 AM  all    0.00    0.00    0.00    0.00    1.02    0.00    0.00    0.00    0.00   98.98
09:16:43 AM    0    0.00    0.00    0.00    0.00    1.02    0.00    0.00    0.00    0.00   98.98
09:16:43 AM    1    0.00    0.00    0.00    0.00    1.01    0.00    0.00    0.00    0.00   98.99

Average:     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
Average:     all    0.00    0.00    0.00    0.00    1.01    0.00    0.00    0.00    0.00   98.99
Average:       0    0.00    0.00    0.00    0.00    1.34    0.00    0.00    0.00    0.00   98.66
Average:       1    0.00    0.00    0.00    0.00    0.67    0.00    0.00    0.00    0.00   99.33

[0;36m## 6. Disk I/O Saturation (iostat) ##[0m
-> High '%util' means the disk is saturated. High 'await' means high disk latency.
Linux 4.18.0-553.6.1.el8.x86_64 (vbox) 	06/10/2025 	_x86_64_	(2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.14    0.01    1.65    0.06    0.00   98.14

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
scd0             0.01    0.00      0.01      0.00     0.00     0.00   0.00   0.00    1.23    0.00   0.00     2.37     0.00   2.52   0.00
sda              3.18    0.28    117.61      4.94     0.02     0.03   0.63  10.91    3.30    7.83   0.01    37.02    17.84   2.22   0.77
dm-0             2.93    0.31    108.59      4.58     0.00     0.00   0.00   0.00    3.39    7.75   0.01    37.10    14.94   2.21   0.72
dm-1             0.02    0.00      0.38      0.00     0.00     0.00   0.00   0.00   20.80    0.00   0.00    21.35     0.00  10.44   0.02

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    1.00    0.00    0.00   99.00

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    1.50    0.00    0.00   98.50

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util


## Part 3: Culprit Identification (pidstat) ##
-> Identifying *which specific processes* are causing the bottlenecks.

[0;36m## 7. Top Processes by CPU Usage (pidstat -u) ##[0m
-> High '%usr' is application load. High '%system' is kernel/driver load.
Average:        0      3680    0.00    0.66    0.00    0.00    0.66     -  pidstat

[0;36m## 8. Top Processes by Memory Faults (pidstat -r) ##[0m
-> Focus on [0;31m'majflt/s'[0m (Major Faults). This indicates a process is causing disk reads (swapping) to get memory.

[0;36m## 9. Top Processes by Disk I/O (pidstat -d) ##[0m
-> This shows which processes are reading from and writing to disk.
09:16:53 AM UID PID [0;31mkB_rd/s[0m [0;31mkB_wr/s[0m kB_ccwr/s iodelay Command
09:16:54 AM UID PID [0;31mkB_rd/s[0m [0;31mkB_wr/s[0m kB_ccwr/s iodelay Command
Average: UID PID kB_rd/s [0;31mkB_wr/s[0m [0;31mkB_ccwr/s[0m iodelay Command

[0;36m## 10. Final Summary: Top Processes by CPU & Memory ##[0m
-> A snapshot of the most resource-intensive processes running right now.
top - 09:16:55 up  1:36,  2 users,  load average: 0.08, 0.02, 0.00
Tasks: 140 total,   1 running, 139 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni, 96.8 id,  0.0 wa,  3.2 hi,  0.0 si,  0.0 st
MiB Mem :   7697.8 total,   6679.0 free,    362.8 used,    656.0 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   7082.8 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   3694 root      20   0  264032   4304   3732 R   6.2   0.1   0:00.01 top
      1 root      20   0  175792  14068   9100 S   0.0   0.2   0:05.08 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.02 kthreadd
      3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_gp
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 slub_flushwq
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-events_highpri
     10 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 mm_percpu_wq
     11 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tasks_rude_
     12 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tasks_trace
     13 root      20   0       0      0      0 S   0.0   0.0   0:00.30 ksoftirqd/0
     14 root      20   0       0      0      0 I   0.0   0.0   0:01.49 rcu_sched
     15 root      rt   0       0      0      0 S   0.0   0.0   0:00.05 migration/0
=====================================================================
                         Diagnostic Script Finished
=====================================================================

