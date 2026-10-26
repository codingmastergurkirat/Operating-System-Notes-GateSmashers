# Page Replacement Algorithms and Belady’s Anomaly

## 1. Virtual Memory and Page Replacement

Virtual memory allows a process to execute without keeping all of its pages in RAM simultaneously. The operating system loads pages as they are needed. If a requested page is absent, a **page fault** occurs and the operating system must bring the page into memory.

If all physical frames are occupied, the operating system may need to remove a resident page to make room. The rule used to choose that page is called a **page-replacement algorithm**.

- **Page hit:** The requested page is already in a frame.
- **Page fault (miss):** The requested page is not in RAM and must be loaded.
- **Reference string:** The sequence of page numbers requested by the CPU.
- **Frame:** A fixed-size slot in physical memory that can hold one page.

For a given reference string:

\[
\text{Hit ratio}=\frac{\text{Number of hits}}{\text{Total references}}\times100\%
\]

\[
\text{Page-fault ratio}=\frac{\text{Number of page faults}}{\text{Total references}}\times100\%
\]

Initially, all frames are assumed to be empty, so the first reference to each distinct page causes a fault unless it has already been loaded.

## 2. FIFO (First-In, First-Out)

**Rule:** Replace the page that has been in memory the longest—the page that was loaded first among the currently resident pages.

FIFO follows the order in which pages entered memory. A hit does not change the replacement order.

### Example: 3 frames

Reference string:

`1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5`

| # | Page requested | Frame 1 | Frame 2 | Frame 3 | Result | Replaced |
|---:|---:|---:|---:|---:|---|---:|
| 1 | 1 | 1 | — | — | Fault | — |
| 2 | 2 | 1 | 2 | — | Fault | — |
| 3 | 3 | 1 | 2 | 3 | Fault | — |
| 4 | 4 | 4 | 2 | 3 | Fault | 1 |
| 5 | 1 | 4 | 1 | 3 | Fault | 2 |
| 6 | 2 | 4 | 1 | 2 | Fault | 3 |
| 7 | 5 | 5 | 1 | 2 | Fault | 4 |
| 8 | 1 | 5 | 1 | 2 | Hit | — |
| 9 | 2 | 5 | 1 | 2 | Hit | — |
| 10 | 3 | 5 | 3 | 2 | Fault | 1 |
| 11 | 4 | 5 | 3 | 4 | Fault | 2 |
| 12 | 5 | 5 | 3 | 4 | Hit | — |

**Result with 3 frames:** 9 page faults and 3 hits.

### Same reference string: 4 frames

| # | Page requested | Frame 1 | Frame 2 | Frame 3 | Frame 4 | Result | Replaced |
|---:|---:|---:|---:|---:|---:|---|---:|
| 1 | 1 | 1 | — | — | — | Fault | — |
| 2 | 2 | 1 | 2 | — | — | Fault | — |
| 3 | 3 | 1 | 2 | 3 | — | Fault | — |
| 4 | 4 | 1 | 2 | 3 | 4 | Fault | — |
| 5 | 1 | 1 | 2 | 3 | 4 | Hit | — |
| 6 | 2 | 1 | 2 | 3 | 4 | Hit | — |
| 7 | 5 | 5 | 2 | 3 | 4 | Fault | 1 |
| 8 | 1 | 5 | 1 | 3 | 4 | Fault | 2 |
| 9 | 2 | 5 | 1 | 2 | 4 | Fault | 3 |
| 10 | 3 | 5 | 1 | 2 | 3 | Fault | 4 |
| 11 | 4 | 4 | 1 | 2 | 3 | Fault | 5 |
| 12 | 5 | 4 | 5 | 2 | 3 | Fault | 1 |

**Result with 4 frames:** 10 page faults and 2 hits.

## 3. Belady’s Anomaly

Usually, adding more frames is expected to reduce or leave unchanged the number of page faults. However, **FIFO can exhibit Belady’s anomaly**: increasing the number of frames can increase the number of page faults for some reference strings.

In the example above:

| Number of frames | Page faults | Hits |
|---:|---:|---:|
| 3 | 9 | 3 |
| 4 | 10 | 2 |

The page-fault count increases from **9 to 10** when the number of frames increases from 3 to 4. This is Belady’s anomaly.

**Exam point:** FIFO can exhibit Belady’s anomaly. The standard optimal algorithm and LRU do not exhibit this anomaly under the usual stack-algorithm model.

## 4. Optimal Page Replacement

**Rule:** When a page fault occurs and all frames are full, replace the resident page whose next use is farthest in the future. If a page will never be used again, it is an ideal replacement candidate.

Optimal replacement is useful as a benchmark because it minimizes the number of page faults for a known reference string. In a real system, the operating system cannot generally know future page references exactly.

### Example: 4 frames

Reference string:

`7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1`

| # | Page requested | Frame 1 | Frame 2 | Frame 3 | Frame 4 | Result | Replaced |
|---:|---:|---:|---:|---:|---:|---|---:|
| 1 | 7 | 7 | — | — | — | Fault | — |
| 2 | 0 | 7 | 0 | — | — | Fault | — |
| 3 | 1 | 7 | 0 | 1 | — | Fault | — |
| 4 | 2 | 7 | 0 | 1 | 2 | Fault | — |
| 5 | 0 | 7 | 0 | 1 | 2 | Hit | — |
| 6 | 3 | 3 | 0 | 1 | 2 | Fault | 7 |
| 7 | 0 | 3 | 0 | 1 | 2 | Hit | — |
| 8 | 4 | 3 | 0 | 4 | 2 | Fault | 1 |
| 9 | 2 | 3 | 0 | 4 | 2 | Hit | — |
| 10 | 3 | 3 | 0 | 4 | 2 | Hit | — |
| 11 | 0 | 3 | 0 | 4 | 2 | Hit | — |
| 12 | 3 | 3 | 0 | 4 | 2 | Hit | — |
| 13 | 2 | 3 | 0 | 4 | 2 | Hit | — |
| 14 | 1 | 1 | 0 | 4 | 2 | Fault | 3 |
| 15 | 2 | 1 | 0 | 4 | 2 | Hit | — |
| 16 | 0 | 1 | 0 | 4 | 2 | Hit | — |
| 17 | 1 | 1 | 0 | 4 | 2 | Hit | — |
| 18 | 7 | 1 | 0 | 7 | 2 | Fault | 4 |
| 19 | 0 | 1 | 0 | 7 | 2 | Hit | — |
| 20 | 1 | 1 | 0 | 7 | 2 | Hit | — |

**Result:** 8 page faults and 12 hits.

When two or more resident pages will not be used again, any of them may be selected; the resulting frame contents may differ, but the optimal fault count can remain the same.

## 5. LRU (Least Recently Used)

**Rule:** Replace the page that has gone the longest without being used, based on past references.

LRU looks **backward** in the reference string. At a fault, compare the pages currently in memory and identify which one was used least recently. A hit updates that page’s recency.

### Example: 4 frames

Use the same reference string:

`7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1`

| # | Page requested | Frame 1 | Frame 2 | Frame 3 | Frame 4 | Result | Replaced |
|---:|---:|---:|---:|---:|---:|---|---:|
| 1 | 7 | 7 | — | — | — | Fault | — |
| 2 | 0 | 7 | 0 | — | — | Fault | — |
| 3 | 1 | 7 | 0 | 1 | — | Fault | — |
| 4 | 2 | 7 | 0 | 1 | 2 | Fault | — |
| 5 | 0 | 7 | 0 | 1 | 2 | Hit | — |
| 6 | 3 | 3 | 0 | 1 | 2 | Fault | 7 |
| 7 | 0 | 3 | 0 | 1 | 2 | Hit | — |
| 8 | 4 | 3 | 0 | 4 | 2 | Fault | 1 |
| 9 | 2 | 3 | 0 | 4 | 2 | Hit | — |
| 10 | 3 | 3 | 0 | 4 | 2 | Hit | — |
| 11 | 0 | 3 | 0 | 4 | 2 | Hit | — |
| 12 | 3 | 3 | 0 | 4 | 2 | Hit | — |
| 13 | 2 | 3 | 0 | 4 | 2 | Hit | — |
| 14 | 1 | 3 | 0 | 1 | 2 | Fault | 4 |
| 15 | 2 | 3 | 0 | 1 | 2 | Hit | — |
| 16 | 0 | 3 | 0 | 1 | 2 | Hit | — |
| 17 | 1 | 3 | 0 | 1 | 2 | Hit | — |
| 18 | 7 | 7 | 0 | 1 | 2 | Fault | 3 |
| 19 | 0 | 7 | 0 | 1 | 2 | Hit | — |
| 20 | 1 | 7 | 0 | 1 | 2 | Hit | — |

**Result:** 8 page faults and 12 hits.

LRU often performs better than FIFO on examples like this, but it requires tracking recent use. Implementations may need hardware support or approximations to manage that information efficiently.

## 6. MRU (Most Recently Used)

**Rule:** Replace the resident page that was used most recently in the past.

MRU also looks **backward**, but unlike LRU it selects the page with the most recent use. This can be useful for certain access patterns, although it is not generally the default choice for typical demand-paging workloads.

### Example: 4 frames

Use the same reference string:

`7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1`

| # | Page requested | Frame 1 | Frame 2 | Frame 3 | Frame 4 | Result | Replaced |
|---:|---:|---:|---:|---:|---:|---|---:|
| 1 | 7 | 7 | — | — | — | Fault | — |
| 2 | 0 | 7 | 0 | — | — | Fault | — |
| 3 | 1 | 7 | 0 | 1 | — | Fault | — |
| 4 | 2 | 7 | 0 | 1 | 2 | Fault | — |
| 5 | 0 | 7 | 0 | 1 | 2 | Hit | — |
| 6 | 3 | 7 | 3 | 1 | 2 | Fault | 0 |
| 7 | 0 | 7 | 0 | 1 | 2 | Fault | 3 |
| 8 | 4 | 7 | 4 | 1 | 2 | Fault | 0 |
| 9 | 2 | 7 | 4 | 1 | 2 | Hit | — |
| 10 | 3 | 7 | 4 | 1 | 3 | Fault | 2 |
| 11 | 0 | 7 | 4 | 1 | 0 | Fault | 3 |
| 12 | 3 | 7 | 4 | 1 | 3 | Fault | 0 |
| 13 | 2 | 7 | 4 | 1 | 2 | Fault | 3 |
| 14 | 1 | 7 | 4 | 1 | 2 | Hit | — |
| 15 | 2 | 7 | 4 | 1 | 2 | Hit | — |
| 16 | 0 | 7 | 4 | 1 | 0 | Fault | 2 |
| 17 | 1 | 7 | 4 | 1 | 0 | Hit | — |
| 18 | 7 | 7 | 4 | 1 | 0 | Hit | — |
| 19 | 0 | 7 | 4 | 1 | 0 | Hit | — |
| 20 | 1 | 7 | 4 | 1 | 0 | Hit | — |

**Result:** 12 page faults and 8 hits.

## 7. Comparison of the Algorithms

| Algorithm | Which page is replaced? | Uses information from | Can exhibit Belady’s anomaly? |
|---|---|---|---|
| FIFO | Page loaded earliest among resident pages | Load order / past | Yes |
| Optimal | Page whose next use is farthest in the future | Future references | No |
| LRU | Page least recently used | Past references | No |
| MRU | Page most recently used | Past references | It is not a stack algorithm; anomaly behavior depends on the reference string |

### Memory aid

- **FIFO:** oldest page loaded.
- **Optimal:** farthest next use in the future.
- **LRU:** least recent use in the past.
- **MRU:** most recent use in the past.

## 8. Solving Any Page-Replacement Numerical

1. Write the reference string in order.
2. Draw one column for each available frame.
3. Start with all frames empty.
4. For each requested page:
   - If it is already present, mark **Hit** and leave the frame contents unchanged.
   - If it is absent, mark **Fault**.
   - If a free frame exists, load the page there.
   - Otherwise, apply the chosen algorithm to select a victim page.
5. Count hits and faults.
6. If asked for ratios, divide each count by the total number of references and multiply by 100%.

**Important distinction:** A page fault is not always a replacement. When an empty frame is available, the missing page can be loaded without removing another page. Replacement is needed only when no suitable free frame is available.
