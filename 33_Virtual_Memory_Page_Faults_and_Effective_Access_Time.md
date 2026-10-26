# Virtual Memory, Page Faults, and Effective Memory Access Time

## 1. What Is Virtual Memory?

**Virtual memory** is a memory-management concept that gives a process the appearance of having a large address space, even when physical RAM is limited.

The lecture highlights two main benefits:

1. A process larger than physical RAM can execute because only the pages needed at a given time must be resident in memory.
2. Parts of multiple processes can reside in RAM at the same time, supporting a higher degree of multiprogramming than would be possible if every process had to be loaded in full.

Physical memory is finite, while programs and their address spaces can be larger. Virtual memory helps manage this difference.

## 2. Paging and Locality of Reference

In paging:

- A process's virtual address space is divided into fixed-size **pages**.
- Physical memory is divided into frames of the same size.
- Rather than loading an entire process, the operating system can keep only the required pages in RAM.

The lecture connects this approach to **locality of reference**: when a page is accessed, nearby or related pages may also be accessed soon. The lecture illustrates this with a teacher calling roll number 10 and expecting nearby numbers, such as 9 or 11, to be called around the same time.

The operating system uses page replacement when RAM is full and a different page must be brought in. The lecture names FIFO, LRU, and MRU as examples of page-replacement algorithms to be covered later.

## 3. Swap-In, Swap-Out, and Page Replacement

- **Swap-in / roll-in:** bringing a page or process into main memory.
- **Swap-out / roll-out:** removing a page or process from main memory to make room.
- **Page replacement:** choosing a resident page to remove when a needed page must be loaded and no suitable free frame is available.

The lecture describes virtual memory as allowing parts of several processes to reside in RAM, rather than requiring every process to be present in full.

## 4. What Is a Page Fault?

When the CPU references a page, the Memory Management Unit (MMU) and page table are used to determine whether the page is resident in RAM.

A **page fault** occurs when the referenced page is not currently resident in physical memory.

Page-table status bits indicate whether a page is present. The lecture refers to valid/invalid or present/absent status. In its example, a status value of 0 indicates that the requested page is absent.

### If the page is present

1. The page table identifies the physical frame.
2. The requested data is accessed from RAM.
3. Execution continues.

This is the normal page-hit case.

### If the page is absent

A page fault is raised, transferring control to the operating system so that the fault can be handled.

## 5. Page-Fault Service: Main Steps

The lecture outlines the following sequence:

1. **CPU reference:** The CPU generates a logical address that refers to a particular page, such as page 1 of process P1.
2. **Page-table check:** The system checks the page-table entry and finds that the page is absent.
3. **Trap to the OS:** A page-fault trap transfers control from the running user process to the operating system.
4. **Locate the page:** The operating system locates the required page in secondary storage.
5. **Load into RAM:** The page is brought into a free frame. If no suitable free frame exists, a resident page may need to be replaced.
6. **Update the page table:** The operating system records the page's new frame mapping and updates the relevant status bits.
7. **Resume execution:** Control returns to the process, which can retry the instruction and access the page.

The lecture's example places the requested page in frame 7 and updates the page table to record that mapping.

The operating system must also validate that the reference is legal for the process. A reference to an invalid address is not resolved simply by loading a page from storage.

## 6. Page-Fault Service Time vs. Main-Memory Access Time

The lecture contrasts two time scales:

- **Main-memory access time (`MA`)** is typically measured in nanoseconds.
- **Page-fault service time (`PFST`)** can be measured in milliseconds because it may involve an OS trap, bookkeeping, and reading the page from secondary storage.

A millisecond is \(10^6\) nanoseconds, so even an infrequent page fault can have a substantial effect on average memory-access time.

The page-fault service time often includes the work required to locate and load the page and update the necessary structures. Some formulas treat the final memory access after servicing the fault separately; use the convention specified by a question.

## 7. Effective Memory Access Time (EAT)

Let:

- \(p\) = probability that a memory reference causes a page fault
- \(1-p\) = probability that a reference does not cause a page fault
- \(PFST\) = page-fault service time
- \(MA\) = main-memory access time

Using the lecture's simplified model:

\[
\boxed{EAT = p \times PFST + (1-p)\times MA}
\]

This is a weighted average:

- With probability \(p\), the reference incurs page-fault service time.
- With probability \(1-p\), the reference requires a normal main-memory access.

**Important:** This simplified expression assumes that `PFST` represents the full cost assigned to the page-fault case. If a problem defines page-fault service time as excluding a subsequent memory access, include that extra access as required by the problem's convention.

### Example setup

If a question provides a page-fault probability, page-fault service time, and main-memory access time, substitute them into:

\[
EAT = p(PFST) + (1-p)(MA)
\]

Convert all times to the same unit before calculating. If a fault rate is given as a percentage, convert it to a probability; for example, 2% becomes \(p=0.02\).

## 8. Relationship Between Page Faults and Thrashing

Page faults are expected in a demand-paged virtual-memory system. The problem arises when they occur too frequently.

- More page faults mean more time spent servicing faults and moving pages.
- Less time remains for useful process execution.
- CPU utilization and system throughput may fall.
- Severe excessive paging is called **thrashing**.

The lecture emphasizes that reducing page faults and increasing page hits improves effective memory-access performance.

## 9. Advantages of Virtual Memory

As presented in the lecture:

- Processes larger than physical RAM can execute.
- Parts of multiple processes can be kept in RAM at once.
- The system does not need to load every process completely before execution.
- Demand paging and page replacement allow physical memory to be used for pages needed at the time.

These are practical capabilities rather than a literal absence of limits: a process's address space and the number of processes are still constrained by the operating system, architecture, storage, and available resources.

## 10. Exam Revision Table

| Term | Meaning |
|---|---|
| Virtual memory | Allows execution using a virtual address space larger than the currently resident physical memory |
| Page | Fixed-size block of virtual memory |
| Frame | Fixed-size block of physical memory, equal in size to a page |
| Locality of reference | Tendency for accesses to cluster in time or address space |
| Swap-in / roll-in | Bringing a page or process into memory |
| Swap-out / roll-out | Removing a page or process from memory |
| Page fault | Reference to a page that is not resident in RAM |
| Page-fault service time | Time to handle a page fault |
| Main-memory access time | Time for a normal access to RAM |
| Effective access time | Average access time weighted by the probabilities of the possible cases |
| Thrashing | Excessive paging that degrades system performance |

### Core formulas

\[
\boxed{EAT = p(PFST)+(1-p)(MA)}
\]

\[
\boxed{1\text{ millisecond}=10^6\text{ nanoseconds}}
\]

**Core idea:** Virtual memory lets the operating system keep only currently needed pages in RAM. When a referenced page is absent, a page fault triggers operating-system work to load it. Because page-fault handling is much slower than a normal RAM access, a high page-fault rate can sharply degrade performance.
