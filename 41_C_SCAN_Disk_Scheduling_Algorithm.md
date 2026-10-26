# C-SCAN (Circular SCAN) Disk Scheduling Algorithm

## 1. Overview

**C-SCAN** stands for **Circular SCAN**. It is a disk scheduling algorithm that services requests while moving in one direction. After reaching the physical end of the disk, the head returns to the beginning and continues servicing requests in the same direction.

For the lecture's example:
- Total tracks: **200**
- Track numbers: **0–199**
- Initial head position: **50**
- Request queue: **82, 170, 43, 140, 24, 16, 190**
- Initial direction: toward **larger track numbers**

The objective in the example is to calculate total read/write-head movement.

## 2. How C-SCAN Works

1. Start at the current head position.
2. Move in the specified direction, servicing requests encountered along the way.
3. Continue to the physical end of the disk in that direction.
4. Return to the opposite end of the disk. In the lecture's convention, this return movement is counted, but no requests are serviced during it.
5. Continue in the original direction, servicing the remaining requests.

This circular behavior distinguishes C-SCAN from SCAN: after reaching one end, C-SCAN returns to the other end and continues in the same service direction rather than servicing requests on a normal reverse sweep.

## 3. Worked Example: Initial Direction Toward Larger Track Numbers

### Step 1: Move from track 50 toward track 199

Service the requests greater than 50 in increasing order:

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190 \rightarrow 199
\]

The requests serviced on this sweep are 82, 140, 170, and 190. Track 199 is the physical end of the disk.

### Step 2: Return from track 199 to track 0

Move to the beginning of the disk:

\[
199 \rightarrow 0
\]

Under the lecture's stated calculation convention, this movement is included in the total head movement. No pending request is serviced during this return.

### Step 3: Service the remaining requests from track 0 upward

The remaining requests are 16, 24, and 43. Continue in the original direction:

\[
0 \rightarrow 16 \rightarrow 24 \rightarrow 43
\]

### Complete path

\[
50 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\rightarrow 199 \rightarrow 0 \rightarrow 16 \rightarrow 24 \rightarrow 43
\]

## 4. Total Head Movement

The lecture groups the travel into three parts:

1. From initial position 50 to the high end, 199.
2. From 199 to 0.
3. From 0 to the last remaining request, 43.

\[
\text{Total movement}=(199-50)+(199-0)+(43-0)
\]

\[
=149+199+43=\boxed{391\text{ tracks}}
\]

Therefore, using the lecture's convention, **C-SCAN total head movement = 391 tracks**.

## 5. Important Convention About the Return Movement

The lecture emphasizes that the movement from track 199 to track 0 is counted:

\[
199-0=199\text{ tracks}
\]

The head physically moves across this range, even though it does not service requests during the return. The transcript notes that some explanations or materials may omit this return movement, but the method taught in this lecture includes it. Follow the convention specified by the question or course material.

## 6. C-SCAN Compared with SCAN and C-LOOK

| Algorithm | Where it goes before changing the service sweep | Return behavior |
|---|---|---|
| SCAN | Physical end in the current direction | Reverses and services requests on the way back |
| C-SCAN | Physical end in the current direction | Returns to the opposite physical end without servicing requests, then continues in the same direction |
| C-LOOK | Last pending request in the current direction | Returns to the last pending request at the opposite side, then continues in the same direction |

This table provides a brief distinction. The worked numerical example in this note covers C-SCAN only.

## 7. Exam Checklist

1. Write the track range and initial head position.
2. Identify the initial direction.
3. Service requests in increasing or decreasing order along that direction.
4. Continue to the physical disk boundary.
5. Include the boundary-to-boundary return movement if required by the question's convention.
6. Service the remaining requests in the original direction.
7. Add the absolute distances for each segment.
8. Report the result as total head movement in tracks.

## 8. Quick Revision

- **C-SCAN = Circular SCAN.**
- Service requests in one direction.
- Travel to the physical end of the disk.
- Return to the opposite end without servicing requests during the return.
- Continue servicing requests in the original direction.
- For the lecture's example, counting the 199-to-0 return gives **391 tracks** of total head movement.
