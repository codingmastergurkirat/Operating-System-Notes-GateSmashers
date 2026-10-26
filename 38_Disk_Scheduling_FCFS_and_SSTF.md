# Disk Scheduling Algorithms: FCFS and SSTF

## 1. Purpose of Disk Scheduling

Disk scheduling algorithms determine the order in which pending disk I/O requests are serviced. In the lecture, the main goal is to reduce **seek time** by reducing the distance the read/write head travels between tracks.

### Disk-structure refresher

The lecture uses this hierarchy:

**Platter → Surface → Track → Sector → Data**

- A disk contains one or more platters mounted on a spindle.
- Each platter has an upper and a lower surface; a surface may contain tracks.
- Tracks are concentric circles, and each track is divided into sectors.
- Data is stored in sectors.
- The actuator arm moves the read/write heads between tracks.

**Seek time** is the time taken for the read/write head to move from its current track to the requested track. For the scheduling examples in this lecture, seek cost is represented by the absolute difference between track numbers:

\[
\text{Head movement} = |\text{destination track} - \text{source track}|
\]

The total head movement is used as a proxy for total seek distance/time in these numerical problems. Rotational latency and transfer time are not calculated in these examples.

## 2. Algorithms Covered

The lecture names these disk scheduling algorithms:
- First Come, First Served (FCFS)
- Shortest Seek Time First (SSTF)
- SCAN
- LOOK
- Circular SCAN (C-SCAN)
- Circular LOOK (C-LOOK)

The provided examples explain FCFS and SSTF.

---

## 3. First Come, First Served (FCFS)

### Principle

FCFS services requests in the exact order in which they appear in the request queue. It does not reorder requests based on their distance from the current head position.

### Example

A disk has 200 tracks numbered **0–199**.

- Initial read/write head position: **50**
- Request queue: **82, 170, 43, 140, 24, 16, 190**

The head follows this sequence:

\[
50 \rightarrow 82 \rightarrow 170 \rightarrow 43 \rightarrow 140 \rightarrow 24 \rightarrow 16 \rightarrow 190
\]

### Calculate total head movement

Add the absolute movement between each consecutive pair:

| Movement | Distance |
|---|---:|
| 50 → 82 | \(|82-50|=32\) |
| 82 → 170 | \(|170-82|=88\) |
| 170 → 43 | \(|43-170|=127\) |
| 43 → 140 | \(|140-43|=97\) |
| 140 → 24 | \(|24-140|=116\) |
| 24 → 16 | \(|16-24|=8\) |
| 16 → 190 | \(|190-16|=174\) |
| **Total** | **642 tracks** |

Therefore, **FCFS total head movement = 642 tracks**.

### Advantages

- Simple to understand and implement.
- Services requests in arrival order.
- Does not cause starvation due to the algorithm's request-order policy: every queued request is eventually reached, assuming the disk continues servicing requests and the queue is finite.

### Disadvantage

- It can produce substantial head movement because it does not consider request proximity.
- The head may repeatedly travel back and forth across the disk, increasing the total seek distance.

---

## 4. Shortest Seek Time First (SSTF)

### Principle

SSTF selects the pending request whose track is closest to the head's **current** position. After servicing a request, it repeats the selection using the new head position.

Do not select the numerically smallest track automatically. Select the request with the smallest absolute distance from the current head position.

### Example

Use the same disk and requests:

- Tracks: **0–199**
- Initial head position: **50**
- Request queue: **82, 170, 43, 140, 24, 16, 190**

At each step, select the nearest unserviced request:

1. From 50, the nearest request is 43 (distance 7).
2. From 43, the nearest is 24 (distance 19).
3. From 24, the nearest is 16 (distance 8).
4. From 16, the nearest remaining request is 82 (distance 66).
5. From 82, the nearest remaining request is 140 (distance 58).
6. From 140, the nearest remaining request is 170 (distance 30).
7. From 170, the remaining request is 190 (distance 20).

The service sequence is:

\[
50 \rightarrow 43 \rightarrow 24 \rightarrow 16 \rightarrow 82 \rightarrow 140 \rightarrow 170 \rightarrow 190
\]

### Calculate total head movement

| Movement | Distance |
|---|---:|
| 50 → 43 | \(|43-50|=7\) |
| 43 → 24 | \(|24-43|=19\) |
| 24 → 16 | \(|16-24|=8\) |
| 16 → 82 | \(|82-16|=66\) |
| 82 → 140 | \(|140-82|=58\) |
| 140 → 170 | \(|170-140|=30\) |
| 170 → 190 | \(|190-170|=20\) |
| **Total** | **208 tracks** |

Therefore, **SSTF total head movement = 208 tracks**.

### Advantages

- Often reduces head movement compared with FCFS.
- Requests near the current head position can be serviced quickly, often improving average response time.

### Disadvantages

- **Starvation is possible.** A request far from the current head position may wait a long time if nearer requests continue to arrive.
- The scheduler must repeatedly compare pending requests with the current head position, creating selection overhead.
- SSTF often reduces seek distance, but it does not guarantee a globally optimal total for every workload.

### Starvation illustration from the lecture

Suppose the head is at track 50 and requests arrive around tracks 40, 61, 30, 20, and 10. If the head moves from 50 to 40, SSTF may choose 30, then 20, then 10 before servicing the farther request at 61. That farther request can wait while closer requests are selected first.

---

## 5. FCFS vs. SSTF

| Feature | FCFS | SSTF |
|---|---|---|
| Selection rule | Original queue order | Nearest pending track to current head |
| Reorders requests? | No | Yes |
| Example service sequence | 82, 170, 43, 140, 24, 16, 190 | 43, 24, 16, 82, 140, 170, 190 |
| Total movement in this example | 642 tracks | 208 tracks |
| Main benefit | Simple and fair by arrival order | Usually reduces head movement |
| Main limitation | May travel long distances | Can cause starvation and selection overhead |

## 6. How to Solve Disk-Scheduling Numericals

1. Write down the initial head position.
2. Write the request queue.
3. Apply the algorithm's selection rule to determine the service sequence.
4. Calculate the absolute difference between each consecutive head position.
5. Add all movement distances.
6. Report the total in **tracks of movement**. It is a seek-distance measure; it is not a time value unless a model for time per track movement is also provided.

### Common mistakes

- Forgetting to include movement from the initial head position to the first request.
- Using signed subtraction and allowing negative distances. Use absolute differences.
- For SSTF, choosing the smallest track number instead of the nearest request.
- Reusing the original head position at every SSTF step instead of updating it after each request.
- Confusing total head movement (tracks) with seek time (seconds or milliseconds).

## 7. Quick Revision

- **Disk scheduling goal in this lecture:** reduce head movement and seek cost.
- **FCFS:** service requests in arrival order; example total = **642 tracks**.
- **SSTF:** service the nearest pending request at each step; example total = **208 tracks**.
- **FCFS:** no starvation from request reordering under a finite queue, but may have high movement.
- **SSTF:** often lower movement, but starvation is possible.
- SCAN, LOOK, C-SCAN, and C-LOOK are named in the lecture but are not worked through in the provided material.
