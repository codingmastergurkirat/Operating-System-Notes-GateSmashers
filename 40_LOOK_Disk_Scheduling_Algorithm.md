# LOOK Disk Scheduling Algorithm

## 1. Overview

The **LOOK algorithm** is a disk scheduling algorithm similar to SCAN. Both algorithms service requests while moving in a chosen direction and then reverse direction. The key difference is where the head reverses:

- **SCAN** continues to the physical end of the disk in the current direction.
- **LOOK** reverses at the last pending request in that direction, without travelling to the disk's physical end unnecessarily.

The initial direction must be known for LOOK, just as it must be for SCAN.

## 2. Worked Example

The lecture uses these values:

- Total tracks: **200**
- Track numbers: **0–199**
- Initial read/write-head position: **50**
- Request queue: **82, 170, 43, 140, 24, 16, 190**
- Initial direction: toward **larger track numbers**

### Case A: Moving toward larger track numbers first

From track 50, service requests in increasing order:

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\]

Track 190 is the last pending request in that direction, so LOOK reverses there. It does **not** continue to track 199.

On the return sweep, service the remaining requests:

\[
190 \rightarrow 43 \rightarrow 24 \rightarrow 16
\]

The complete path is:

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\rightarrow 43 \rightarrow 24 \rightarrow 16
\]

### Total head movement

The movement can be calculated as two straight sweeps:

- From 50 to the last request in the upward direction, 190
- From 190 back to the last remaining request, 16

\[
\text{Total movement}=(190-50)+(190-16)
\]

\[
=140+174=\boxed{314\text{ tracks}}
\]

Therefore, **LOOK total head movement = 314 tracks** for this starting direction.

## 3. Case B: Moving toward lower track numbers first

If the initial direction is toward smaller track numbers, service the lower requests first:

\[
50 \rightarrow 43 \rightarrow 24 \rightarrow 16
\]

Track 16 is the last pending request in that direction. LOOK reverses there rather than continuing to track 0:

\[
16 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\]

The complete path is:

\[
50 \rightarrow 43 \rightarrow 24 \rightarrow 16
\rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\]

Total head movement:

\[
(50-16)+(190-16)=34+174=\boxed{208\text{ tracks}}
\]

## 4. LOOK Compared with SCAN

| Feature | SCAN | LOOK |
|---|---|---|
| Initial direction needed? | Yes | Yes |
| Services requests along the current direction? | Yes | Yes |
| Where does it reverse? | At the physical end of the disk | At the last pending request in that direction |
| Unnecessary travel to disk boundary? | May occur | Avoided |
| Example, direction toward higher tracks first | 332 tracks | 314 tracks |

For the lecture's example, LOOK saves 18 tracks compared with SCAN when the initial direction is toward higher track numbers:

\[
332-314=18\text{ tracks}
\]

This happens because SCAN travels from track 190 to track 199 before reversing, while LOOK reverses at track 190.

## 5. Convert Head Movement to Time

If the question states that moving between adjacent tracks takes **1 nanosecond**, multiply total head movement by 1 ns per track.

For the higher-track-first case:

\[
314\text{ tracks}\times1\text{ ns/track}=\boxed{314\text{ ns}}
\]

This is the movement time under the question's stated assumption. It excludes rotational latency and data-transfer time unless those are separately included.

## 6. How to Solve LOOK Numericals

1. Note the initial head position.
2. Note the pending request queue.
3. Identify the initial direction.
4. Service requests in order along that direction.
5. Reverse at the last pending request in that direction—not at the physical disk boundary.
6. Service the remaining requests on the return sweep.
7. Add the absolute differences between consecutive head positions, or group the path into straight sweeps.
8. If a time per track movement is given, multiply the total movement by that time.

## 7. Quick Revision

- **LOOK is similar to SCAN**, but it turns around at the last request rather than at the disk's physical end.
- Initial direction matters.
- With the given example and direction toward higher tracks first: **314 tracks**.
- With the given example and direction toward lower tracks first: **208 tracks**.
- At 1 ns per track movement, the higher-track-first case takes **314 ns** under the stated assumption.
