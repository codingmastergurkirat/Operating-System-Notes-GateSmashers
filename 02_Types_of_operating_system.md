**For Better Understanding of Notes, First watch the video lectures :**
- [Batch Operating System and Types of OS](https://www.youtube.com/watch?v=povNcHSasgs&list=PLxCzCOWd7aiGz9donHRrE9I3Mwn6XdP8p&index=3)
- [Multiprogramming and Multitasking Operating Systems](https://www.youtube.com/watch?v=3MqyDWDpZoI&list=PLxCzCOWd7aiGz9donHRrE9I3Mwn6XdP8p&index=4)
- [Real-Time, Distributed, Clustered, and Embedded Operating Systems](https://www.youtube.com/watch?v=YQZbIT9FcUk&list=PLxCzCOWd7aiGz9donHRrE9I3Mwn6XdP8p&index=5)

# Types of Operating Systems

The main types of operating systems covered in these notes are:

- Batch Operating System
- Multiprogramming Operating System
- Multitasking Operating System
- Real-Time Operating System
- Distributed Operating System
- Clustered System
- Embedded Operating System

Multiprogramming and multitasking are particularly important because they explain how the CPU manages multiple processes efficiently.

---

# Batch Operating System

## Definition

A **Batch Operating System** groups similar jobs into batches and executes them with little or no direct user interaction during execution. Users submit their jobs to an operator, who organizes them into batches for processing.

## Historical Background

Batch systems were widely used during the early days of computing, particularly around the 1950s and 1960s. Computers were expensive and mainly available at large organizations, universities, and research institutions. Organizations such as NASA and ISRO needed computers for scientific calculations and research.

## Job Submission

Early batch jobs were prepared using physical media such as:

- **Punch cards:** Cards containing holes that represented data or program instructions.
- **Paper tape:** Paper containing encoded information.
- **Magnetic tape:** Magnetic storage used to store programs and data.

The operator collected jobs, grouped similar jobs into batches, submitted them for execution, and made the results available to users when processing was complete.

Users generally could not interact directly with their programs while they were running and might have to wait a long time for results.

## How Batch Processing Works

1. The operator or operating system selects a job from a batch.
2. The job is loaded and executed by the CPU.
3. If the job requests an input/output (I/O) operation, such as reading data from a disk, it must wait for that operation to complete.
4. In a simple batch system, the CPU may remain idle while the job waits for I/O.
5. When the I/O operation finishes, the job continues.
6. Once the job completes, the system proceeds to the next job.

For example, a batch might contain jobs to calculate student results, generate employee salary reports, and process bank transactions.

## Main Disadvantage: CPU Idle Time

In a simple batch system, when a job waits for an I/O operation, the CPU may remain idle instead of executing another waiting job. This leads to:

- Low CPU utilization.
- Inefficient use of computer resources.
- Longer overall processing time for the workload.

**Multiprogramming helps reduce this problem** by keeping multiple jobs in memory and switching to another ready job while one job waits for I/O.

## Resident Monitors

As batch systems developed, **resident monitors** were introduced to automate parts of job processing.

A resident monitor is a program that remains in memory and helps control job execution. It can load jobs, transfer control between jobs, and start the next job when the previous one finishes. This reduced the need for operators to manage every step manually.

FORTRAN systems are also mentioned in the lecture as part of the historical development of early computing.

## Advantages

- Suitable for processing large numbers of similar jobs.
- Reduces the need for continuous user interaction.
- Allows jobs to be organized and processed systematically.
- Useful for repetitive tasks such as payroll processing and report generation.

## Disadvantages

- Users cannot directly interact with jobs during execution.
- Results may take a long time to become available.
- Debugging and correcting errors can be inconvenient.
- Simple batch systems may suffer from CPU idle time during I/O operations.
- Resource utilization may be less efficient than in multiprogramming systems.

---

# Multiprogramming Operating System

## Definition

**Multiprogramming** is an operating system technique in which multiple processes are kept in main memory (RAM) so that the CPU can execute another ready process when the currently running process cannot continue.

The main objective is to **reduce CPU idle time and improve CPU utilization**.

## How Multiprogramming Works

Suppose processes P1, P2, and P3 are loaded into memory.

1. The operating system selects P1 and assigns the CPU to it.
2. P1 executes until it finishes or requests an I/O operation.
3. If P1 requests I/O, it enters the waiting state.
4. The operating system selects another ready process, such as P2, rather than allowing the CPU to remain idle.
5. P2 executes while P1 waits for I/O.
6. When P2 finishes or waits for I/O, the operating system may select P3.
7. When P1's I/O operation completes, P1 becomes ready to execute again.

This approach helps keep the CPU busy.

## Example: Students and Questions

Imagine that 10 students each have five questions to solve, and one person is available to help them. The person represents the CPU, and the students represent processes.

The person helps Student 1 until Student 1 finishes or needs to wait. While Student 1 is waiting, the person helps Student 2 or another available student. When Student 1 is ready again, the person can return to them.

This illustrates how multiprogramming reduces CPU idle time.

## Scheduling Note

In a simplified model, multiprogramming is often described as switching when a process finishes or blocks for I/O. However, multiprogramming itself does not require non-preemptive scheduling; the scheduling policy depends on the operating system.

## Advantages

- **Improved CPU utilization:** Another ready process can run while one process waits for I/O.
- **Reduced CPU idle time:** The CPU spends less time waiting for I/O operations.
- **Improved resource utilization:** Multiple jobs can share the computer's resources.
- **Increased throughput:** More jobs may be completed over a given period.

---

# Multitasking / Time-Sharing Operating System

## Definition

**Multitasking** allows multiple tasks or processes to make progress by sharing CPU time.

In a time-sharing system, the CPU typically gives each ready process a limited amount of time called a **time slice** or **time quantum**. The operating system switches between processes so each gets an opportunity to execute.

The main emphasis is on responsiveness and supporting interactive use while using the CPU efficiently.

## How Multitasking Works

Suppose P1, P2, and P3 are ready to execute.

1. The CPU executes P1 for a fixed time slice.
2. If P1 finishes within that time, it leaves the ready queue.
3. If P1 does not finish before its time slice expires, the operating system can interrupt it.
4. The CPU switches to P2, then to P3.
5. The CPU returns to P1 if it is still ready and unfinished.

This cycle continues until the processes finish or no longer need CPU time.

## Preemptive Scheduling

Multitasking systems commonly use **preemptive scheduling**. This allows the operating system to interrupt a running process and assign the CPU to another process. In time-sharing systems, this often happens when the time quantum expires.

## Example: Students and Questions

Imagine that 10 students each have five questions, and one person is available to help them. Instead of helping Student 1 with all five questions before moving to Student 2, the person helps each student with a limited number of questions, then rotates to the next student. After everyone has had a turn, the person returns to Student 1.

The limited turns represent CPU time slices.

## Advantages

- **Responsiveness:** Processes receive frequent opportunities to execute.
- **Fairness:** CPU time can be shared among ready processes according to the scheduling policy.
- **Improved user experience:** Interactive applications do not have to wait for one long-running process to finish.
- **Better CPU utilization:** Another ready process can run when the current process blocks for I/O.
- **Concurrent progress:** Multiple applications can make progress over the same period.

## Round Robin Scheduling

**Round Robin** is a common scheduling algorithm used in time-sharing systems. Each ready process receives a fixed time quantum.

### How It Works

1. Processes are placed in a ready queue.
2. The CPU selects the first ready process.
3. The process executes for up to one time quantum.
4. If it finishes, it leaves the ready queue.
5. If it does not finish, it is preempted and placed at the end of the ready queue.
6. The CPU selects the next ready process and repeats the cycle.

### Example

If P1, P2, and P3 are ready and the time quantum is 4 milliseconds, each process can execute for up to 4 milliseconds in its turn. A process may use less time if it finishes or blocks before the quantum expires.

If the time quantum is too large, Round Robin may behave more like first-come, first-served scheduling. If it is too small, frequent context switches may introduce excessive overhead.

---

# Multiprogramming vs. Multitasking

| Basis | Multiprogramming | Multitasking / Time-Sharing |
|---|---|---|
| Basic concept | Keeps multiple processes in memory so the CPU has work available | Shares CPU time among processes |
| Main objective | Reduce CPU idle time and improve CPU utilization | Improve responsiveness while sharing CPU time |
| CPU switching | Commonly occurs when a process finishes or blocks for I/O in the simplified model | Can occur when a time quantum expires |
| Scheduling | May be preemptive or non-preemptive, depending on the system | Commonly uses preemptive scheduling |
| Time quantum | Not essential to the basic concept | Commonly used in time-sharing |
| Main emphasis | CPU utilization | Responsiveness and interactive use |
| Example | Running another job while the current job waits for disk input | Giving each ready process a limited turn on the CPU |

**Main difference:** Multiprogramming focuses on keeping the CPU busy, while multitasking/time-sharing focuses on sharing CPU time to support responsive execution. The two techniques can coexist in the same operating system.

---

# Real-Time Operating System (RTOS)

## Definition

A **Real-Time Operating System (RTOS)** is designed to process events and respond within specified time constraints. In a real-time system, correctness depends not only on the result but also on when the result is produced.

Real-time does not necessarily mean an instant response; it means meeting the timing requirements of the system.

## Real-Time vs. Non-Real-Time

- **Non-real-time:** There is generally no strict deadline for producing a result. For example, a pre-recorded video can be watched long after it was uploaded.
- **Real-time:** The system must respond within a specified time. Delays can affect the result or the system's operation.

A live stream is an example where low delay matters, although not every streaming application is itself a real-time operating system.

## Types of Real-Time Systems

### Hard Real-Time System

A **Hard Real-Time System** has strict deadlines that must be met. Missing a critical deadline is considered a system failure.

**Examples:**
- Missile guidance systems.
- Aircraft flight-control systems.

For example, a delayed response from a missile guidance system could cause it to miss its intended target.

### Soft Real-Time System

A **Soft Real-Time System** can tolerate occasional delays. A delay may reduce performance or quality but does not necessarily cause a critical failure.

**Examples:**
- Computer gaming.
- Live video streaming.

For example, a short delay in a live stream may reduce the viewing experience without causing a critical failure.

## Hard vs. Soft Real-Time Systems

| Hard Real-Time | Soft Real-Time |
|---|---|
| Critical deadlines must be met. | Occasional delays are acceptable. |
| Missing a critical deadline is a system failure. | Missing a deadline generally reduces performance or quality. |
| Example: Missile guidance system. | Example: Live video streaming. |

---

# Distributed Operating System

## Definition

A **Distributed System** consists of multiple independent computers connected through a network and working together. The computers may be located in different geographical regions.

## Characteristics

- **Multiple computers:** Several interconnected computers participate in the system.
- **Geographical distribution:** Computers may be located in different cities or countries.
- **Loosely coupled:** Each computer generally has its own kernel, memory, storage, and other resources.
- **Independent operation:** Each computer has its own environment but can communicate with others through a network.

## Example

A large online service may distribute its computing resources across several regions rather than storing and processing everything at one location. Services such as Google and Facebook use distributed computing environments.

## Advantages

- **Resource sharing:** Computers can share data and services.
- **Availability:** Other computers may continue providing services if one fails, provided the system is designed for this.
- **Scalability:** More computers can be added to increase capacity.
- **Fault tolerance:** The system may continue operating despite certain computer failures.

---

# Clustered System

## Definition

A **Clustered System** consists of multiple computers connected through a network and working together as a coordinated group. Cluster nodes are commonly located close to one another and connected through a local network (LAN) or another high-speed network.

## Example

Suppose 100 computers are connected and configured to work together. The workload can be distributed among them rather than relying on a single computer, increasing available processing capacity for suitable tasks.

## Advantages

- **High availability:** If one computer fails, another may take over, depending on the cluster's configuration.
- **Fault tolerance:** Redundancy can help the system continue operating despite certain failures.
- **Load balancing:** Work can be distributed across multiple computers.
- **Scalability:** More computers can be added to increase capacity.
- **Increased processing power:** Multiple computers can work together on computational tasks.

## Distributed System vs. Clustered System

| Distributed System | Clustered System |
|---|---|
| Computers may be geographically separated. | Computers are commonly located close to one another. |
| Computers communicate through a network. | Computers commonly communicate through a high-speed local network. |
| Each computer has its own resources and environment. | Computers cooperate as a coordinated group. |
| Focuses on distributing resources and tasks across computers. | Often focuses on availability, load balancing, or computational performance. |

A cluster is often considered a type of distributed system. The concepts overlap, but they emphasize different ways of organizing and using multiple computers.

---

# Embedded Operating System

## Definition

An **Embedded Operating System** is designed to manage a device that performs a specific function or a limited set of functions. Embedded systems are designed for dedicated purposes rather than general-purpose computing.

## Characteristics

- Designed for specific functions.
- Usually integrated with particular hardware.
- Often operates with limited memory and processing power.
- Software is developed according to the device's intended purpose.

## Examples

- Washing machines.
- Microwave ovens.
- Air conditioners.
- Routers.
- Printers.
- Smart appliances.
- Embedded controllers in vehicles.

## Example: Washing Machine

A washing machine performs specific operations such as washing, rinsing, and spinning clothes. Its embedded software controls these functions according to the selected program.

Unlike a general-purpose computer, a washing machine is not designed to run a wide range of unrelated applications.

Embedded devices can sometimes be reprogrammed or updated, but their functionality is generally designed around a specific purpose.

**Embedded system vs. embedded operating system:**
- An **embedded system** is the complete device, including its hardware and software.
- An **embedded operating system** is the operating system software that manages the device's hardware, resources, and tasks.

Not every embedded system requires a full operating system; some simple devices run directly on firmware.

---

# Quick Revision

| Type | Main Concept | Examples |
|---|---|---|
| Batch Operating System | Groups similar jobs and processes them with little or no direct user interaction | Payroll processing, report generation |
| Multiprogramming | Keeps multiple processes in memory to reduce CPU idle time | Running another job while one waits for I/O |
| Multitasking / Time-Sharing | Shares CPU time among processes, commonly using time slices | Interactive applications |
| Hard Real-Time System | Critical deadlines must be met | Missile guidance, aircraft control |
| Soft Real-Time System | Occasional delays are tolerable | Live streaming, computer gaming |
| Distributed System | Multiple independent computers cooperate over a network | Geographically distributed computing environments |
| Clustered System | Multiple computers work together as a coordinated group | High-availability server cluster |
| Embedded Operating System | Manages a device designed for specific functions | Washing machine, microwave oven, air conditioner |

---
