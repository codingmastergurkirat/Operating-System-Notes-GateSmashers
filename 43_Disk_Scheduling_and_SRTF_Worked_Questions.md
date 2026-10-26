# Operating Systems: Disk Scheduling and CPU Scheduling Worked Questions

## 1. Question 1 — C-SCAN Disk Scheduling

### Given

- Track range: **0–199** (200 tracks)
- Initial read/write-head position: **50**
- Initial direction: toward **smaller track numbers**
- Request queue, in the order presented: **34, 11, 180, 123, 119, 95, 64, 62**
- Time to move between adjacent tracks: **2 ms**
- Time to move from one end of the disk to the other: **10 ms**

The task is to calculate the total seek time using C-SCAN.

### C-SCAN service path

The head starts at track 50 and moves toward lower track numbers. It services requests 34 and 11, then continues to track 0 because C-SCAN travels to the physical end of the disk in the current direction.

It then moves from track 0 to track 199. No requests are serviced during this return. Continuing in the original service direction (toward lower track numbers), it services the remaining requests in descending order:

\[
180 \rightarrow 123 \rightarrow 119 \rightarrow 95 \rightarrow 64 \rightarrow 62
\]

A compact path description is:

\[
50 \rightarrow 34 \rightarrow 11 \rightarrow 0
\rightarrow 199 \rightarrow 180 \rightarrow 123 \rightarrow 119
\rightarrow 95 \rightarrow 64 \rightarrow 62
\]

### Calculate movement

First sweep:

\[
50-0=50\text{ tracks}
\]

Return between disk ends:

\[
199-0=199\text{ tracks}
\]

From track 199 down to the last remaining request, track 62:

\[
199-62=137\text{ tracks}
\]

Total movement along ordinary track-to-track steps:

\[
50+137=187\text{ tracks}
\]

The question gives a separate time of 10 ms for moving from one disk end to the other, so use that for the 0-to-199 return rather than multiplying 199 by 2 ms.

### Calculate total time

Time for the two ordinary movement portions:

\[
(50\times2)+(137\times2)=100+274=374\text{ ms}
\]

Add the stated end-to-end return time:

\[
374+10=\boxed{384\text{ ms}}
\]

**Answer: 384 ms.**

**Important:** The lecture's calculation treats the end-to-end return separately at 10 ms. It does not calculate that return as 199 adjacent-track movements at 2 ms each.

---

## 2. Question 2 — SRTF CPU Scheduling and Context Switches

### Given

There are three CPU-intensive processes:

| Process | Arrival time | Burst time |
|---|---:|---:|
| P1 | 0 | 10 |
| P2 | 2 | 20 |
| P3 | 6 | 30 |

Scheduling algorithm: **Shortest Remaining Time First (SRTF)**, the pre-emptive version of Shortest Job First (SJF).

The question asks for the number of context switches. The lecture says not to count the initial dispatch at time 0 or the transition to idle at the end.

### Scheduling sequence described in the lecture

- At time 0, P1 is the only arrived process, so it starts running.
- At time 2, P2 arrives. P1 has 8 time units remaining, which is less than P2's burst time of 20, so P1 continues.
- At time 6, P3 arrives. P1 has 4 time units remaining, less than P2's remaining 20 and P3's 30, so P1 continues.
- P1 finishes at time 10.
- P2 runs from time 10 to time 30.
- P3 runs from time 30 to time 60.
- The CPU is idle after time 60.

### Gantt chart

```text
0          10                  30                              60
|    P1     |        P2         |              P3                |
```

### Count context switches

Following the lecture's counting convention:

1. **P1 → P2** at time 10
2. **P2 → P3** at time 30

Do not count:
- The initial dispatch from idle to P1 at time 0
- The transition from P3 to idle at time 60

Therefore, **number of context switches = 2**.

### Key concept

A context switch occurs when the CPU changes from running one process to running another. In this question, only process-to-process transitions in the middle of the schedule are counted, as specified in the lecture.

---

## 3. Quick Revision

### C-SCAN

- Move toward the specified end of the disk, servicing requests along the way.
- Continue to the physical end.
- Return to the opposite end without servicing requests during the return.
- Resume servicing in the original direction.
- For Question 1, use the special given 10 ms end-to-end time: **total seek time = 384 ms**.

### SRTF and context switches

- SRTF chooses the arrived process with the shortest remaining CPU time.
- A newly arrived process pre-empts the current one only if it has a shorter remaining time.
- In this example, P1 remains shortest until completion, then P2, then P3.
- With the lecture's rule excluding initial dispatch and final transition to idle: **2 context switches**.
