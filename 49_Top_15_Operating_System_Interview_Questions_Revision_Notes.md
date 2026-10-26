# Top 15 Operating System Interview Questions — Revision Notes

These notes organize the topics covered in the supplied lecture transcript into a concise interview-revision guide. The transcript presents these as commonly useful topics to review; it does not establish that any particular question will appear in an interview.

## 1. What Is an Operating System?

An **operating system (OS)** manages computer hardware and provides services to applications. Typical responsibilities include:
- Process and thread management
- CPU scheduling
- Memory management
- File-system management
- Device and I/O management
- Protection and security
- Providing system-call interfaces

The transcript also recommends knowing the basic purpose of common operating systems such as Windows, Linux, and Android.

## 2. Process States and the Process Control Block (PCB)

### Common process states
The five commonly taught states are:
- **New:** the process is being created.
- **Ready:** the process is waiting for CPU time.
- **Running:** the process is executing on a CPU.
- **Waiting/Blocked:** the process is waiting for an event, often I/O completion.
- **Terminated:** the process has finished or has been stopped.

Some models also include **suspended-ready** and **suspended-blocked** states.

### Process Control Block (PCB)
A **PCB** is an operating-system data structure that stores information needed to manage a process. It commonly includes:
- Process ID
- Process state
- CPU register and program-counter information
- Scheduling information
- Memory-management information
- Accounting and I/O status information

The OS uses PCB information when scheduling and switching between processes.

### Thread states and TCB
The transcript describes thread states as **running, ready, and blocked**. A **Thread Control Block (TCB)** stores information used to manage an individual thread.

## 3. Mutex vs. Binary Semaphore

Both are synchronization mechanisms, but their intended roles differ.

| Mutex | Binary semaphore |
|---|---|
| Primarily used for mutual exclusion around a critical section. | A signaling mechanism whose value is typically 0 or 1. |
| Usually has ownership: the thread that locks it is expected to unlock it. | Does not generally enforce the same ownership rule; one thread can signal another. |
| Used to protect shared data/resources. | Often used for signaling or coordination, and can also be used for mutual exclusion. |

The transcript's central distinction is: a mutex acts as a lock, while a binary semaphore can be used to signal that an event has occurred or that a resource is available. Exact APIs and rules vary by operating system and programming environment.

## 4. Process vs. Thread

A **process** is a running program with its own process-level resources and address space. A **thread** is an execution unit within a process.

| Process | Thread |
|---|---|
| Has its own process address space and resources. | Threads in the same process generally share the process address space and resources. |
| Process creation and context switching can have greater overhead. | Thread creation and switching are often less costly, though this depends on the system. |
| Processes are more strongly isolated from one another. | Threads can communicate through shared memory, but must synchronize access to shared data. |

Multithreading can allow different tasks within a process to make progress concurrently and, on suitable hardware, execute in parallel.

## 5. Monolithic Kernel vs. Microkernel

The **kernel** is the core of the operating system that manages privileged operations and system resources.

### Monolithic kernel
- Many operating-system services run in kernel space.
- This can provide efficient communication between services.
- A fault in a kernel component can have broad consequences.

### Microkernel
- Keeps a relatively small set of essential mechanisms in the kernel.
- More services, such as some drivers or system services, can run in user space.
- This can improve isolation, but communication between components may introduce overhead.

Hybrid designs also exist. The transcript emphasizes the distinction between a larger kernel containing many services and a smaller kernel that moves more services to user space.

## 6. System Calls

A **system call** is the interface through which a user-level program requests a service from the operating-system kernel.

Examples mentioned in the transcript include:
- `open()` — open a file or resource
- `read()` — read data
- `write()` — write data
- `fork()` — create a process in Unix-like systems
- `sleep()` — suspend execution for a specified period, where supported

System calls can be grouped by purpose, such as process control, file management, device management, information maintenance, and communication.

## 7. Demand Paging

**Demand paging** loads a page into main memory when it is needed rather than loading every page of a process in advance.

Basic sequence:
1. A process references a page.
2. If the page is resident, execution continues.
3. If the page is absent, a page fault occurs.
4. The OS brings the page into memory, updates the page table, and resumes execution.

Demand paging can reduce unnecessary memory use, but page faults require additional work and can be expensive.

## 8. Virtual Memory

**Virtual memory** allows a process to operate within a virtual address space that need not all be resident in physical memory at once.

The OS and hardware translate virtual addresses to physical addresses. Pages not currently resident may be stored in secondary storage and brought into memory when required.

Virtual memory:
- Supports address-space isolation and process organization.
- Allows programs to use address spaces larger than the available physical memory, subject to system limits.
- Does not make disk storage as fast as RAM; frequent paging can significantly reduce performance.

## 9. Linux Command: `chmod`

The `chmod` command changes file or directory permissions on Unix-like systems.

The common permission classes are:
- **u** — owner/user
- **g** — group
- **o** — others

The common permission bits are:
- **r** — read
- **w** — write
- **x** — execute/search, depending on whether the object is a file or directory

Examples:

```bash
chmod u+x script.sh
chmod 640 report.txt
```

- `chmod u+x script.sh` adds execute permission for the owner.
- `chmod 640 report.txt` sets owner permissions to read/write, group permissions to read, and no permissions for others.

## 10. Paging vs. Segmentation

| Paging | Segmentation |
|---|---|
| Divides virtual memory into fixed-size pages. | Divides a program's address space into logical, variable-sized segments. |
| Page size is fixed by the architecture/system configuration. | Segment sizes vary according to logical units such as code, data, or stack. |
| Address translation uses a page number and offset. | Address translation commonly uses a segment identifier and offset, with bounds checks. |
| Can cause internal fragmentation, especially in the last page of an allocation. | Can cause external fragmentation in systems that allocate variable-sized segments contiguously. |

Paging is primarily a fixed-size memory-management scheme; segmentation reflects logical divisions of a program.

## 11. Internal vs. External Fragmentation

### Internal fragmentation
Wasted space **inside** an allocated block.

Example: if a fixed-size block is 10 KB and a process uses 8 KB, 2 KB of the allocated block is unused.

### External fragmentation
Free space exists, but it is split into separate non-contiguous holes, so a sufficiently large contiguous allocation cannot be made.

Example: three separate 2 KB holes provide 6 KB in total, but cannot satisfy a request for one contiguous 5 KB block.

The transcript associates internal fragmentation with fixed partitioning and external fragmentation with variable partitioning. These are useful introductory associations, though the exact occurrence depends on the allocation scheme.

## 12. Preemptive vs. Non-Preemptive Scheduling

| Preemptive scheduling | Non-preemptive scheduling |
|---|---|
| The OS can take the CPU away from a running process, for example when its time quantum expires or a higher-priority process should run. | Once scheduled, a process generally keeps the CPU until it terminates, blocks, or voluntarily yields it. |
| Supports responsive time-sharing. | Can be simpler, but a long-running process may delay others. |
| Round Robin is a common example. | FCFS and non-preemptive SJF are common examples. |

A process may block for I/O under either approach. The key distinction is whether the scheduler can preempt a process that is still running.

## 13. Deadlock

A **deadlock** occurs when a set of processes is permanently blocked because each is waiting for a resource or event that can only be supplied by another process in the set.

Example:
- P1 holds resource R1 and waits for R2.
- P2 holds resource R2 and waits for R1.
- Neither process can proceed until the other releases its resource.

The four necessary conditions traditionally associated with deadlock are:
1. **Mutual exclusion:** at least one resource is non-shareable.
2. **Hold and wait:** a process holds resources while waiting for more.
3. **No preemption:** resources cannot be forcibly taken away under the model.
4. **Circular wait:** a circular chain of processes exists, each waiting for a resource held by the next.

The transcript mentions the Banker's algorithm and circular wait as related topics.

## 14. Multiprogramming vs. Multitasking

### Multiprogramming
Several programs are kept in memory so the CPU can switch to another ready program when the current one waits, improving CPU utilization.

### Multitasking
The operating system allows multiple tasks to make progress over time, commonly by rapidly switching CPU execution among them. On multicore systems, tasks or threads may also run in parallel.

These terms overlap in modern systems, but they emphasize different goals: multiprogramming is traditionally associated with keeping the CPU busy, while multitasking emphasizes supporting multiple active tasks.

## 15. Spatial vs. Temporal Locality

Programs tend to access memory in patterns known as **locality of reference**.

- **Spatial locality:** when a memory location is accessed, nearby locations are likely to be accessed soon. Sequential instruction execution is a common example.
- **Temporal locality:** a recently accessed location is likely to be accessed again soon, such as a loop repeatedly using the same variable.

Locality helps explain why caches and hierarchical memory systems can improve performance.

## 16. Virus vs. Worm vs. Trojan Horse vs. Spyware

The transcript includes malware concepts as a final interview-revision topic.

| Type | Main characteristic |
|---|---|
| **Virus** | Attaches to a host file or program and spreads when the infected host is activated. |
| **Worm** | Can self-propagate between systems, often through networks, without needing to attach to a host file in the same way as a virus. |
| **Trojan horse** | Disguises itself as legitimate or useful software; it is not defined by self-replication. |
| **Spyware** | Collects information about a user or system without appropriate consent. |

**Malware** is the broad category for software designed to cause harm, gain unauthorized access, or perform unwanted actions. Different malware categories can overlap in real-world incidents.

## 17. Additional General Preparation Mentioned in the Lecture

The transcript also recommends reviewing:
- Basic responsibilities of an operating system
- Common operating systems, including Windows, Linux, and Android
- Basic processor terms such as dual-core and octa-core
- Common Linux commands and their purposes

These are broad preparation suggestions rather than separate detailed explanations in the transcript.

## Quick Interview Revision Checklist

- Define an OS and list its responsibilities.
- Name the process states and explain what a PCB stores.
- Explain the difference between a mutex and a binary semaphore.
- Compare processes and threads.
- Compare monolithic and microkernels.
- Define a system call and name common examples.
- Explain demand paging and page faults.
- Define virtual memory.
- Explain `chmod` and basic permission bits.
- Compare paging with segmentation.
- Distinguish internal and external fragmentation.
- Compare preemptive and non-preemptive scheduling.
- Define deadlock and list its four necessary conditions.
- Compare multiprogramming with multitasking.
- Define spatial and temporal locality.
- Distinguish viruses, worms, Trojan horses, and spyware.
