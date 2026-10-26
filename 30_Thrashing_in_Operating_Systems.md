# Thrashing in Operating Systems

## 1. What Is Thrashing?

**Thrashing** occurs when the operating system spends an excessive amount of time servicing page faults and bringing pages from secondary storage into RAM, leaving comparatively little time for useful process execution.

As page faults become frequent, system performance and throughput decline. CPU utilization can also fall sharply.

## 2. Relationship to Degree of Multiprogramming

The **degree of multiprogramming** is the number of processes resident in main memory.

Increasing the degree of multiprogramming can initially improve CPU utilization:

- If a running process requests an I/O operation, it may move from the **Running** state to the **Blocked** state.
- The CPU can then run another process from the **Ready** state.
- Having more ready processes available makes it less likely that the CPU will remain idle while another process waits for I/O.

However, RAM has a limited capacity. The operating system cannot keep every process entirely in memory at once.

## 3. Paging and Memory Use

With **paging**, a process is divided into pages, and the operating system loads the pages needed into physical memory frames.

This allows the system to keep parts of many processes in RAM rather than requiring every process to be loaded in its entirety. As a result, the system can support a higher degree of multiprogramming.

### The risk of keeping too many processes resident

Suppose the system has many processes, but only one page from each process is currently in RAM. If a process accesses a page that is not resident, a **page fault** occurs.

For example:

- Page 1 of process P1 is in RAM, but P1 requests page 2.
- Page 1 of process P2 is in RAM, but P2 requests page 2.
- Other processes similarly request pages that are not resident.

Each such access may require the operating system to fetch a page from secondary storage. Page-fault service takes much longer than accessing a page already in RAM.

If page faults occur too frequently, the operating system can spend much of its time moving pages rather than executing processes. This is thrashing.

> Having more processes resident can improve CPU utilization at first, but increasing the degree of multiprogramming beyond the system's capacity to keep the needed pages resident can cause thrashing.

## 4. CPU Utilization vs. Degree of Multiprogramming

The lecture describes the typical relationship using a graph:

- **X-axis:** Degree of multiprogramming
- **Y-axis:** CPU utilization

The curve has three broad phases:

1. **Initial increase:** Adding processes gives the scheduler more work to choose from when another process blocks for I/O. CPU utilization tends to rise.
2. **Peak:** CPU utilization reaches a high point at some degree of multiprogramming.
3. **Decline due to thrashing:** If the degree of multiprogramming becomes too high for available memory, page faults can become frequent. The operating system spends more time servicing them, and CPU utilization and throughput can drop sharply.

The lecture labels the peak with \(\lambda\). This represents the point on the illustrated curve where CPU utilization is at its maximum; it is not the definition of the degree of multiprogramming.

## 5. How Can Thrashing Be Controlled?

The lecture gives two approaches.

### A. Increase the size of main memory

More RAM can provide more frames for the pages that processes need, reducing pressure on memory. The lecture notes that increasing RAM may not always be convenient or practical.

### B. Reduce the degree of multiprogramming

The **long-term scheduler** controls the admission of processes into the system. If too many processes are admitted, the system may have insufficient memory for their active pages.

Reducing the rate at which processes are admitted—or otherwise limiting the number of processes competing for memory—can reduce the pressure that leads to thrashing.

The lecture distinguishes three scheduler types:

- **Long-term scheduler:** Controls which processes are admitted to the system.
- **Short-term scheduler:** Selects a ready process to run on the CPU.
- **Medium-term scheduler:** Can temporarily remove processes from memory and later bring them back.

In the lecture's proposed approach, the long-term scheduler should avoid pushing the degree of multiprogramming to an excessive level.

## 6. Key Terms

| Term | Meaning |
|---|---|
| Degree of multiprogramming | Number of processes resident in main memory |
| Page | Fixed-size unit into which virtual memory is divided |
| Page fault | Occurs when a process references a page that is not currently resident in RAM |
| Page-fault service time | Time needed by the operating system to handle a page fault, including obtaining the required page |
| CPU utilization | Fraction of time the CPU is busy |
| Throughput | Number of processes completed per unit of time |
| Thrashing | Excessive paging activity that leaves too little time for useful execution |

## 7. Exam Summary

- Increasing the degree of multiprogramming can initially increase CPU utilization.
- RAM is limited, so each process may have only some of its pages resident.
- If processes repeatedly request non-resident pages, page faults become frequent.
- Excessive time spent servicing page faults is called **thrashing**.
- Thrashing can cause CPU utilization and throughput to decrease.
- The lecture's two main control methods are **increasing RAM** and **reducing the degree of multiprogramming**.

**Core idea:** The goal is not to maximize the number of resident processes at any cost. The system needs enough processes to keep the CPU busy, but not so many that memory pressure causes excessive paging.
