# Disk Access Time

## 1. Overview

Disk access time is the time required to locate and transfer requested data on a disk. In introductory Operating Systems problems, it is usually calculated from three main components:

1. **Seek time**
2. **Rotational latency** (also called rotational delay)
3. **Transfer time**

Some questions also provide **controller time** and **queue time**. Include these when the question specifies them.

## 2. Seek Time

**Seek time** is the time taken by the read/write head to move to the required track.

- The actuator moves the heads radially across the disk, between outer and inner tracks.
- The head may already be on the required track (best case), or it may need to move across many tracks (worst case).
- For many numerical problems, average seek time is supplied directly. If the question asks for a rough average movement and assumes tracks are equally likely, the lecture describes it as approximately half the number of tracks.

Seek time is an important contributor to access delay because the head must reach the correct track before the desired sector can be accessed.

## 3. Rotation Time and Rotational Latency

### Rotation time

**Rotation time** is the time required for the disk to complete one full revolution (360°).

If the disk speed is \(R\) revolutions per minute (RPM), then:

\[
\text{Time per revolution} = \frac{60}{R} \text{ seconds}
\]

### Rotational latency

After the head reaches the required track, the disk may need to rotate until the required sector is positioned under the head. The time spent waiting is called **rotational latency** or **rotational delay**.

For a uniformly distributed target sector:
- Best-case latency: approximately 0 (the desired sector is already at the head).
- Worst-case latency: nearly one full rotation.
- Average rotational latency: half a rotation.

Therefore:

\[
\text{Average rotational latency} = \frac{1}{2}\times\text{Time per revolution}
\]

\[
\text{Average rotational latency} = \frac{30}{R} \text{ seconds}
\]

**Terminology note:** The lecture sometimes calls this “rotational time.” In calculations, distinguish the time for a complete revolution from the average wait for the desired sector; the latter is half a revolution under the average-case assumption.

## 4. Transfer Time

**Transfer time** is the time required to transfer the requested amount of data once it is available for reading or writing.

\[
\boxed{\text{Transfer time}=\frac{\text{Amount of data to transfer}}{\text{Transfer rate}}}
\]

Use consistent units. For example, if data is measured in MB and the transfer rate in MB/s, the result is in seconds.

The amount of data to transfer may be specified as a number of sectors, a number of tracks, or a byte quantity. If it is given in sectors, calculate the data amount from the sector size when needed.

### Estimating transfer rate from disk parameters

The lecture gives the following introductory relationship:

\[
\text{Transfer rate} =
\text{Number of heads}
\times \text{Capacity of one track}
\times \text{Rotations per second}
\]

Here:
- **Number of heads** is treated in the lecture as the number of recording surfaces/heads used in parallel.
- **Capacity of one track** is the data stored on one track, calculated as:

\[
\text{Track capacity} =
\text{Sectors per track}
\times \text{Bytes per sector}
\]

- **Rotations per second** is obtained from RPM:

\[
\text{Rotations per second} = \frac{\text{RPM}}{60}
\]

For example, at 3000 RPM:

\[
\frac{3000}{60}=50 \text{ rotations per second}
\]

**Assumption:** The transfer-rate formula above is a simplified model based on the lecture's explanation of parallel heads and track capacity. Real HDD transfer rates depend on the drive's recording format, data density, zone layout, and interface; use the formula only when the problem's assumptions support it.

## 5. Controller Time

Controller time is the time attributed to the disk controller's processing or control operations.

- If controller time is explicitly given in a question, add it to the total.
- If it is not given and the question does not ask you to estimate it, do not add an extra term.

## 6. Queue Time

Queue time is the time a request waits in the disk's request queue before the disk begins servicing it.

- Disk requests may wait while earlier requests are handled.
- If queue time is given, include it in total elapsed response/access time.
- If it is not given, do not add it.

## 7. Total Disk Access Time

The basic formula is:

\[
\boxed{\text{Disk access time} =
\text{Seek time}
+\text{Average rotational latency}
+\text{Transfer time}}
\]

If the question provides additional components:

\[
\boxed{\begin{aligned}
\text{Total time} ={}&
\text{Seek time}\\
&+\text{Rotational latency}\\
&+\text{Transfer time}\\
&+\text{Controller time (if given)}\\
&+\text{Queue time (if given)}
\end{aligned}}
\]

If a component is not part of the question, omit it rather than inventing a value.

## 8. Quick Formula Sheet

| Quantity | Formula / meaning |
|---|---|
| Seek time | Time for the head to reach the required track |
| Time per revolution | \(60/\text{RPM}\) seconds |
| Average rotational latency | \(30/\text{RPM}\) seconds |
| Rotations per second | \(\text{RPM}/60\) |
| Track capacity | Sectors per track × bytes per sector |
| Transfer time | Data to transfer ÷ transfer rate |
| Simplified transfer-rate estimate | Heads × track capacity × rotations per second |
| Basic disk access time | Seek time + average rotational latency + transfer time |
| Extended total time | Basic access time + controller time (if given) + queue time (if given) |

## 9. Numerical Problem Checklist

1. Identify whether the question gives RPM or time per rotation.
2. Convert RPM to time per revolution using \(60/\text{RPM}\).
3. For average rotational latency, divide the revolution time by 2.
4. Calculate the requested data size in consistent units.
5. Calculate transfer time as data size divided by transfer rate.
6. Add seek time, rotational latency, and transfer time.
7. Add controller time and/or queue time only if specified.
8. Check that all terms use compatible time units (seconds, milliseconds, etc.).

## 10. Key Points

- **Seek time:** head movement to the correct track.
- **Rotational latency:** waiting for the correct sector to rotate under the head.
- **Transfer time:** time spent transferring the requested data.
- Average rotational latency is half of one revolution when the target sector is assumed uniformly distributed.
- Controller and queue times are additional terms when explicitly supplied.
