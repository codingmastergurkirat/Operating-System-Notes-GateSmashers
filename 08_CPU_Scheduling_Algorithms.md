# CPU Scheduling Algorithms

## Overview

CPU scheduling selects a process from the ready queue and allocates the CPU to it. **Non-pre-emptive** scheduling lets a process keep the CPU until its CPU burst finishes or it blocks. **Pre-emptive** scheduling can interrupt a running process because of a time-quantum expiration, a higher-priority arrival, or a process with shorter remaining time.

| Algorithm | Selection rule | Typical mode |
|---|---|---|
| FCFS | Earliest arrival | Non-pre-emptive |
| SJF | Shortest next CPU burst | Non-pre-emptive |
| SRTF | Shortest remaining time | Pre-emptive |
| LRTF | Longest remaining time | Pre-emptive |
| Round Robin | Ready order and time quantum | Pre-emptive |
| Priority | Highest-priority ready process | Either |
| HRRN | Highest response ratio | Usually non-pre-emptive |
| Multilevel queue | Queue class and policy | Depends on design |
| MLFQ | Queue level and feedback | Depends on design |

Priority numbering must be specified: a smaller or larger number may represent higher priority.

## Scheduling Measurements

- **Arrival time (AT):** when a process enters the ready queue.
- **Burst time (BT):** CPU time required for a burst.
- **Completion time (CT):** when the process finishes.
- **Turnaround time:** `TAT = CT - AT`.
- **Waiting time:** for one simple CPU burst, `WT = TAT - BT`.
- **Response time:** `RT = first CPU start - AT`.

For pre-emptive scheduling, response time is generally not total waiting time. If I/O bursts occur, calculate waiting time from actual ready-queue intervals.

## Solving Numericals

1. Record AT, BT, and priority if given.
2. Identify the algorithm and whether it is pre-emptive.
3. Show CPU idle time when no process is ready.
4. At every decision, consider only arrived ready processes.
5. Apply the selection rule and state tie-breaking assumptions.
6. Draw the Gantt chart.
7. Update remaining burst times after pre-emption.
8. Use the final finish time as CT.
9. Calculate TAT, WT, RT, and averages.

## Main Algorithms

### FCFS

First Come, First Served selects the earliest arriving ready process and normally runs it until completion or blocking. It is simple but can cause a convoy effect. In a non-pre-emptive one-burst example, response time equals waiting time.

### SJF

Shortest Job First chooses the ready process with the shortest CPU burst. Standard SJF is non-pre-emptive. It can minimize average waiting time when burst lengths are known, but longer jobs may wait.

### SRTF

Shortest Remaining Time First is the pre-emptive version of SJF. When a process arrives, compare its burst with the current process's remaining time and pre-empt if the new process is shorter. Completion time is the end of the final Gantt-chart interval.

### Priority Scheduling

The highest-priority ready process runs. It may be pre-emptive or non-pre-emptive. A tie commonly uses FCFS. Repeated higher-priority arrivals can starve lower-priority processes.

### Round Robin

Round Robin gives each ready process a fixed time quantum. An unfinished process is placed at the end of the ready queue. Track both the Gantt chart and queue order. Simultaneous-arrival and re-queue conventions must be stated.

### LRTF and HRRN

LRTF selects the ready process with the largest remaining time. HRRN selects the highest response ratio:

`Response ratio = (Waiting time + Burst time) / Burst time`

### Multilevel Queue and MLFQ

Multilevel queue scheduling separates process categories, such as system, interactive, and batch processes. Fixed queues can cause starvation. Multilevel feedback queues allow processes to move between queues based on behavior or waiting, which can improve responsiveness and reduce starvation when properly designed.

### CPU and I/O Bursts

When a process begins I/O, it becomes blocked. The scheduler runs another ready process; when I/O finishes, the process returns to the ready queue. CPU utilization is busy CPU time divided by total elapsed time, and idle ratio is idle time divided by total elapsed time.
