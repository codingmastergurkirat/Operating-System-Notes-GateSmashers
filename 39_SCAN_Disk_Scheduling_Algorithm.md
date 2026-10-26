# SCAN Disk Scheduling Algorithm

## 1. Overview

The **SCAN disk scheduling algorithm** decides the order in which pending disk requests are serviced. Its goal is to reduce read/write-head movement and therefore reduce seek cost.

SCAN is also called the **elevator algorithm** because the head moves in one direction, servicing requests along the way, continues to the end of the disk in that direction, and then reverses direction.

For this lesson, tracks are numbered from **0 to 199**.

## 2. How SCAN Works

1. Start at the current read/write-head position.
2. Follow the initial direction specified by the question.
3. Service every pending request encountered along that direction.
4. Continue to the physical end of the disk in that direction, even if the last request is encountered before the end.
5. Reverse direction.
6. Service the remaining requests on the return trip, stopping at the last request in that direction unless the disk end is itself requested.

The direction at the start matters. A question should specify whether the head initially moves toward higher or lower track numbers.

## 3. Worked Example

The lecture uses the same example as the earlier FCFS and SSTF lessons:

- Total tracks: **200**
- Track numbers: **0–199**
- Initial head position: **50**
- Request queue: **82, 170, 43, 140, 24, 16, 190**

The lecture first solves the case where the initial direction is toward **larger track numbers**.

### Case A: Initial direction toward larger track numbers

Starting at track 50, service the higher-numbered requests in order:

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190 \rightarrow 199
\]

SCAN continues to the disk's end at track 199 even though the last request in that direction is track 190. It then reverses and services the remaining lower-numbered requests:

\[
199 \rightarrow 43 \rightarrow 24 \rightarrow 16
\]

The complete head path is:

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\rightarrow 199 \rightarrow 43 \rightarrow 24 \rightarrow 16
\]

### Total head movement

Instead of adding each segment separately, group the travel into two straight sweeps:

- First sweep: track 50 to track 199
- Return sweep: track 199 to track 16

\[
\text{Total movement}=(199-50)+(199-16)
\]

\[
=149+183=\boxed{332\text{ tracks}}
\]

Therefore, total head movement is **332 tracks**.

### Why does the head travel to track 199?

This is the defining behavior of SCAN in the lecture: the head goes to the end of the disk in its initial direction before reversing, rather than turning around immediately at the last currently pending request.

## 4. Case B: Initial direction toward lower track numbers

If the initial direction is toward smaller track numbers, the head services the requests below 50 first:

\[
50 \rightarrow 43 \rightarrow 24 \rightarrow 16 \rightarrow 0
\]

It continues to track 0, the disk's end in that direction, and then reverses to service the higher-numbered requests:

\[
0 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\]

The complete path is:

\[
50 \rightarrow 43 \rightarrow 24 \rightarrow 16 \rightarrow 0
\rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\]

Total movement:

\[
(50-0)+(190-0)=50+190=\boxed{240\text{ tracks}}
\]

Thus, for the same request queue, the total is **240 tracks** when the initial direction is toward lower track numbers, following the lecture's SCAN convention.

## 5. Convert Head Movement to Time

If the problem states that moving between adjacent tracks takes a fixed amount of time, multiply the total track movement by that time.

For example, if one track movement takes **1 nanosecond**:

\[
\text{Total movement time}=332\times1\text{ ns}
=\boxed{332\text{ ns}}
\]

This is the movement time under the question's stated assumption. It does not include rotational latency or data-transfer time unless the question separately asks for those components.

## 6. SCAN: Advantages and Trade-off

### Advantage
- The head services requests while sweeping across the disk, and its movement follows a predictable directional pattern.

### Trade-off
- The head may travel to the disk's end even when there are no pending requests near that end, adding movement compared with an algorithm that reverses at the last request.

The lecture also notes that requests may arrive dynamically while the head is moving. Under a changing request queue, whether a newly arrived request can be serviced on the current sweep depends on its position and the direction of travel. The worked numerical example, however, treats the request queue as fixed.

## 7. Quick Revision

- **SCAN = elevator algorithm.**
- Service requests in the current direction.
- Continue to the physical end of the disk in that direction.
- Reverse and service the remaining requests.
- With the given example and initial direction toward larger tracks: **332 tracks** of total movement.
- With the given example and initial direction toward smaller tracks: **240 tracks** of total movement.
- If each track movement takes 1 ns, the 332-track path takes **332 ns** under that assumption.
