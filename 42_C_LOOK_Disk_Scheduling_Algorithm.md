# C-LOOK Disk Scheduling Algorithm

## 1. Overview

**C-LOOK** stands for **Circular LOOK**. It is a disk scheduling algorithm related to LOOK and C-SCAN.

In the lecture's description:
1. The head services requests in one direction, following the given initial direction.
2. It goes as far as the last pending request in that direction, rather than the physical end of the disk.
3. It then jumps back to the lowest pending request on the other side without servicing requests during that return movement.
4. From there, it resumes servicing requests in the original direction.

The initial direction is important when solving C-LOOK numerical problems.

## 2. Worked Example

The lecture uses the following values:

- Total tracks: **200**
- Track numbers: **0–199**
- Initial head position: **50**
- Request queue: **82, 170, 43, 140, 24, 16, 190**
- Initial direction: toward **larger track numbers**

### Step 1: Service requests in the increasing direction

Starting at track 50, the requests above 50 are serviced in increasing order:

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\]

Track 190 is the last request in this direction. Unlike C-SCAN, C-LOOK does not continue to the physical end of the disk at track 199.

### Step 2: Return to the lowest pending request

After servicing track 190, the head moves to the lowest pending request, track 16:

\[
190 \rightarrow 16
\]

During this return movement, no requests are serviced according to the lecture's convention. The return movement is nevertheless included in the total head movement.

### Step 3: Service the remaining requests in increasing order

From track 16, service the remaining requests:

\[
16 \rightarrow 24 \rightarrow 43
\]

### Complete path

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\rightarrow 16 \rightarrow 24 \rightarrow 43
\]

## 3. Total Head Movement

The lecture groups the head movement into three parts:

1. From the initial position 50 to the highest request 190.
2. From 190 back to the lowest request 16.
3. From 16 to the final remaining request 43.

\[
\text{Total movement}=(190-50)+(190-16)+(43-16)
\]

\[
=140+174+27=\boxed{341\text{ tracks}}
\]

Therefore, following the lecture's convention, **C-LOOK total head movement = 341 tracks**.

## 4. Important Note About the Return Movement

The lecture explicitly includes the movement from track 190 to track 16:

\[
190-16=174\text{ tracks}
\]

Even though no requests are serviced during this return, the head travels across those tracks, so the movement is counted in the total. The lecture notes that some materials may omit this part, but the method used here includes it.

## 5. C-LOOK Compared with LOOK and C-SCAN

| Algorithm | Where the head goes before returning | Return movement |
|---|---|---|
| LOOK | Last pending request in the current direction | Reverses and services requests on the way back |
| C-SCAN | Physical end of the disk | Returns to the opposite physical end without servicing requests, then continues in the same direction |
| C-LOOK | Last pending request in the current direction | Jumps to the lowest pending request on the other side without servicing requests, then continues in the same direction |

For the lecture's example, C-LOOK avoids travelling to track 199 because the highest request in the initial direction is track 190. It also avoids travelling all the way to track 0 on the return, because the lowest pending request is track 16.

## 6. Exam Checklist

1. Record the initial head position and request queue.
2. Identify the initial direction.
3. Service requests in order along that direction.
4. Stop at the last pending request in that direction.
5. Count the return movement to the lowest pending request on the other side, if using the lecture's convention.
6. Continue servicing the remaining requests in the original direction.
7. Add the absolute distance for each segment.
8. Report the result as total head movement in tracks.

## 7. Quick Revision

- **C-LOOK = Circular LOOK.**
- Service requests in one direction, only as far as the last pending request.
- Return to the lowest pending request on the other side without servicing requests during the return.
- Continue in the original direction.
- For the lecture's example, including the return movement gives **341 tracks** of total head movement.
