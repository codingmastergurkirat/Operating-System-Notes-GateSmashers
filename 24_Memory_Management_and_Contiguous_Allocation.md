# Memory Management and Contiguous Allocation

## 1. Memory Management Overview

Memory management is the operating system function that tracks, allocates, protects, and reclaims primary memory (RAM). Registers and cache are faster but smaller; RAM holds currently executing processes and data; secondary storage such as an SSD or hard disk stores programs and files persistently.

Programs generally reside in secondary storage until the OS loads required instructions and data into RAM. The OS must provide allocation, deallocation, protection, isolation, address translation support, and coordination with multiprogramming.

## 2. Degree of Multiprogramming

The degree of multiprogramming is the number of processes kept in main memory. A process alternates between CPU bursts and I/O waits. When one process blocks for I/O, another ready process can use the CPU.

A simplified model assumes each process independently waits for I/O for fraction `K` of its time. The probability that all `n` processes wait simultaneously is `K^n`, so estimated CPU utilization is:

```text
U = 1 - K^n
```

For `K=0.70`:

| Processes | Calculation | Utilization |
|---:|---|---:|
| 1 | `1 - 0.7` | 30% |
| 2 | `1 - 0.7^2` | 51% |
| 4 | `1 - 0.7^4` | about 75.99% |

This is only an illustrative independence model. Real utilization depends on scheduling, workload, memory pressure, and I/O behavior.

## 3. Contiguous vs. Non-Contiguous Allocation

In contiguous allocation, each process occupies one continuous physical region. In non-contiguous allocation, parts of a process may occupy separate regions; paging and segmentation are examples.

This file focuses on contiguous allocation:

- **Fixed partitioning:** partitions are defined before allocation.
- **Variable partitioning:** regions are created at run time according to process requests.

## 4. Fixed Partitioning

Memory reserved for processes is divided into a fixed number of equal or unequal partitions. A process must fit completely into one partition.

Example partitions: 4 MB, 8 MB, 8 MB, and 16 MB.

- A 4 MB process fits in 4 MB.
- A 7 MB process fits in 8 MB.
- A 14 MB process fits in 16 MB.
- A 32 MB process cannot fit even if the total free memory in several partitions is at least 32 MB.

### Internal fragmentation

Unused space inside an assigned partition is internal fragmentation. A 7 MB process in an 8 MB partition wastes 1 MB that another process cannot use.

### Limitations

- Maximum process size is bounded by the largest partition.
- Number of resident processes is bounded by the number of partitions.
- Partition waste may be significant.
- Free regions may also be unsuitable for a contiguous request.

The main advantage is simple management.

## 5. Variable Partitioning

The OS creates a contiguous allocation sized according to each process's request. A 2 MB process receives about 2 MB, a 4 MB process receives about 4 MB, and so on.

### Advantages

- Avoids internal waste caused by fixed partition sizes when allocation matches the request.
- Does not impose a fixed partition-count limit on resident processes.
- Allows larger processes if a sufficiently large contiguous region exists.

### Holes and external fragmentation

When a process exits, its region becomes a free hole. Separate holes may total enough memory but not provide one large contiguous region. Two 4 MB holes cannot satisfy an 8 MB request that cannot be split.

This is external fragmentation: free memory is outside allocated blocks but separated into pieces.

### Compaction

Compaction relocates allocated processes so holes are combined into one larger region. It requires time, movement of memory, and coordination or process pauses. It is useful but costly.

### Tracking free memory

Dynamic allocation requires bookkeeping. Bitmaps and linked lists can track allocated regions and holes.

## 6. Fragmentation Comparison

| Property | Internal fragmentation | External fragmentation |
|---|---|---|
| Location | Inside an allocated region | Between allocated regions |
| Typical cause | Fixed-size partition larger than process | Variable allocations and released holes |
| Example | 7 MB process in 8 MB partition | Two 4 MB holes cannot satisfy one 8 MB request |
| Possible response | Better partition sizing or another scheme | Compaction or non-contiguous allocation |

## 7. Summary Comparison

| Feature | Fixed partitioning | Variable partitioning |
|---|---|---|
| Boundaries | Set in advance | Created at run time |
| Sizes | Predetermined | Based on requests |
| Main issue | Internal fragmentation | External fragmentation |
| Process limit | Largest partition | Largest available contiguous hole |
| Multiprogramming limit | Number of partitions | Available memory and holes |
| Management | Simpler | More complex |
| Compaction | Not the main remedy for partition waste | Can combine holes at a cost |

Memory protection is required in both schemes so one process cannot overwrite another process or the OS.