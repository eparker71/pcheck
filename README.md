# pcheck
Linux performance diagnostic tool

download and run ./pcheck.sh

it will output similar to this:

=====================================================================
      Comprehensive CentOS Performance Diagnostic Script
=====================================================================
Timestamp: Tue Jun 10 09:11:53 EDT 2025
Interpreting Colors: Red (Critical), Yellow (Warning), Cyan (Info)

## Part 1: System-Wide Overview ##

## 1. Uptime and Load Average ##
-> Checking system load against the number of CPU cores.
   Number of CPU cores on this system: 2
 09:11:53 up  1:31,  2 users,  load average: 0.00, 0.02, 0.00
   INFO: Load average is within a normal range.

## 2. Kernel Ring Buffer (dmesg) ##
-> Checking for critical kernel-level errors (I/O, Out of Memory).
[ 1674.261421] vboxsf: SHFL_FN_MAP_FOLDER failed for '/mnt/shared': share not found

## 3. Overall Memory Usage (free) ##
-> Checking available memory and swap usage.
              total        used        free      shared  buff/cache   available (values in MB)
Mem:    7697    362     6679    8       656     7083
Swap:   2047    0 2047

## Part 2: Resource Bottleneck Identification ##
-> Identifying *what* kind of resource is under pressure.

## 4. Virtual Memory & I/O Wait (vmstat) ##
-> High 'wa' (wait I/O) means CPU is waiting for disk. High 'si'/'so' means swapping.
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 6839900   3164 668632    0    0    63     3  122  111  0  2 98  0  0
 0  0      0 6839788   3164 668632    0    0     0     0  450  308  1  3 97  0  0
 0  0      0 6839724   3164 668632    0    0     0     0  219  186  0  1 99  0  0
 0  0      0 6839724   3164 668632    0    0     0     0  268  182  0  1 99  0  0
 0  0      0 6839724   3164 668632    0    0     0     0  128  168  0  1 99  0  0

## 5. CPU and I/O Wait (mpstat) ##
-> High '%iowait' indicates an I/O bottleneck. Low '%idle' indicates a CPU bottleneck.
Linux 4.18.0-553.6.1.el8.x86_64 (vbox) 	06/10/2025 	_x86_64_	(2 CPU)

09:11:57 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
09:11:58 AM  all    0.50    0.00    0.00    0.00    0.50    0.00    0.00    0.00    0.00   99.00
09:11:58 AM    0    0.99    0.00    0.00    0.00    0.99    0.00    0.00    0.00    0.00   98.02
09:11:58 AM    1    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00

09:11:58 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
09:11:59 AM  all    0.00    0.00    0.50    0.00    1.50    0.00    0.00    0.00    0.00   98.00
09:11:59 AM    0    0.00    0.00    0.00    0.00    2.00    0.00    0.00    0.00    0.00   98.00
09:11:59 AM    1    0.00    0.00    1.00    0.00    1.00    0.00    0.00    0.00    0.00   98.00

09:11:59 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
09:12:00 AM  all    0.00    0.00    0.00    0.00    0.50    0.00    0.00    0.00    0.00   99.50
09:12:00 AM    0    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00
09:12:00 AM    1    0.00    0.00    0.00    0.00    0.99    0.00    0.00    0.00    0.00   99.01

Average:     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
Average:     all    0.17    0.00    0.17    0.00    0.83    0.00    0.00    0.00    0.00   98.84
Average:       0    0.33    0.00    0.00    0.00    1.00    0.00    0.00    0.00    0.00   98.67
Average:       1    0.00    0.00    0.33    0.00    0.66    0.00    0.00    0.00    0.00   99.00

## 6. Disk I/O Saturation (iostat) ##
-> High '%util' means the disk is saturated. High 'await' means high disk latency.
Linux 4.18.0-553.6.1.el8.x86_64 (vbox) 	06/10/2025 	_x86_64_	(2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.14    0.01    1.69    0.06    0.00   98.10

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
scd0             0.01    0.00      0.01      0.00     0.00     0.00   0.00   0.00    1.23    0.00   0.00     2.37     0.00   2.52   0.00
sda              3.34    0.29    123.68      5.19     0.02     0.04   0.63  10.98    3.30    7.83   0.01    37.02    17.94   2.21   0.80
dm-0             3.08    0.32    114.19      4.81     0.00     0.00   0.00   0.00    3.39    7.75   0.01    37.10    15.01   2.21   0.75
dm-1             0.02    0.00      0.40      0.00     0.00     0.00   0.00   0.00   20.80    0.00   0.00    21.35     0.00  10.44   0.02

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    1.01    0.00    0.00   98.99

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    1.01    0.00    0.00   98.99

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util


## Part 3: Culprit Identification (pidstat) ##
-> Identifying *which specific processes* are causing the bottlenecks.

## 7. Top Processes by CPU Usage (pidstat -u) ##
-> High '%usr' is application load. High '%system' is kernel/driver load.
09:12:04 AM     0      1056    1.00    0.00    0.00    1.00    1.00     0  tuned
Average:       81       781    0.00    0.33    0.00    0.00    0.33     -  dbus-daemon
Average:        0      1056    0.33    0.00    0.00    0.33    0.33     -  tuned
Average:        0      3625    0.00    0.98    0.00    0.00    0.98     -  pidstat

## 8. Top Processes by Memory Faults (pidstat -r) ##
-> Focus on 'majflt/s' (Major Faults). This indicates a process is causing disk reads (swapping) to get memory.

## 9. Top Processes by Disk I/O (pidstat -d) ##
-> This shows which processes are reading from and writing to disk.
09:12:09 AM UID PID kB_rd/s kB_wr/s kB_ccwr/s iodelay Command
09:12:10 AM UID PID kB_rd/s kB_wr/s kB_ccwr/s iodelay Command
Average: UID PID kB_rd/s kB_wr/s kB_ccwr/s iodelay Command

## 10. Final Summary: Top Processes by CPU & Memory ##
-> A snapshot of the most resource-intensive processes running right now.
top - 09:12:11 up  1:31,  2 users,  load average: 0.00, 0.02, 0.00
Tasks: 142 total,   2 running, 140 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  3.1 sy,  0.0 ni, 96.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7697.8 total,   6678.9 free,    362.8 used,    656.1 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   7082.8 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   1025 root      20   0  504740   1176   1064 S   6.2   0.0   0:05.41 VBoxDRMClient
      1 root      20   0  175792  14068   9100 S   0.0   0.2   0:05.07 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.02 kthreadd
      3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_gp
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 slub_flushwq
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-events_highpri
     10 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 mm_percpu_wq
     11 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tasks_rude_
     12 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tasks_trace
     13 root      20   0       0      0      0 S   0.0   0.0   0:00.30 ksoftirqd/0
     14 root      20   0       0      0      0 I   0.0   0.0   0:01.45 rcu_sched
     15 root      rt   0       0      0      0 S   0.0   0.0   0:00.05 migration/0
=====================================================================
                         Diagnostic Script Finished
=====================================================================
