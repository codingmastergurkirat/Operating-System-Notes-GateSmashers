# Operating Systems (OS) — Interview Preparation Checklist

> [!NOTE]
> **Study Guide & Disclaimer**
>
> These Operating Systems interview preparation notes are organized around topics covered in the **Gate Smashers Operating System video playlist**.
>
> **Recommended Study Approach:**
> 1. **Watch the lecture first:** Start with the relevant video.
> 2. **Study the notes:** Read the corresponding section.
> 3. **Practice for interviews:** Answer questions and solve numerical problems.
>
> **Credits & Acknowledgment:** Credit goes to **Gate Smashers** for creating the educational video lectures used as references for these study notes.
>
> **Official Reference Playlist:** [Gate Smashers — Operating System Lectures](https://www.youtube.com/playlist?list=PLxCzCOWd7aiGz9donHRrE9I3Mwn6XdP8p)

---

> [!TIP]
> **How to Use This Repository for Interview Preparation**
>
> 1. **Fork this repository:** Create your own copy of this repository on GitHub so you can maintain your personal progress.
> 2. **Reset the checkboxes:** Untick the topics already marked as completed (`[x]`) to start with a fresh checklist.
> 3. **Study topic by topic:** Watch the relevant lectures, review the notes, and practice interview questions.
> 4. **Track your progress:** Mark a topic as completed (`[x]`) only after you understand it and can explain it confidently.
> 5. **Stay consistent:** Update your checklist regularly to monitor your progress and identify topics that need more revision.
>
> **Goal:** Use this checklist as your personal roadmap to prepare for Operating Systems interviews and track your progress until you are interview-ready.

# Operating Systems (OS) — Interview Preparation Checklist

A consolidated checklist for OS interviews. Topics from both source lists have been merged, duplicates removed, and related concepts grouped together.

**How to use this checklist**
- Mark a topic complete only when you can explain it without notes.
- Complete the practice problems for numerical topics.
- Use the overall progress tracker to track each major section.

---

## Priority 1: Must Know

### 1. OS Fundamentals and Architecture
- [x] What is an Operating System?
- [x] Main purpose and functions of an Operating System
- [x] Types of Operating Systems
- [ ] Real-Time Operating Systems (RTOS): hard vs. soft real-time; types and use cases
- [ ] Kernel and Shell
- [ ] Kernel types, including monolithic kernels
- [x] User Mode vs. Kernel Mode
- [ ] System Calls: purpose and types
- [ ] Interrupts: hardware vs. software interrupts; interrupt handling
- [ ] Context Switching: meaning, purpose, and overhead
- [ ] Spooling: meaning, working, and examples
- [ ] Sockets: purpose and use in inter-process/network communication

### 2. Processes and Threads
- [ ] What is a Process?
- [x] Process States
- [x] Process State Transitions
- [ ] Process Control Block (PCB)
- [ ] Process vs. Program
- [ ] Process vs. Thread
- [ ] Types of Processes
- [ ] User-Level Threads vs. Kernel-Level Threads
- [ ] Multitasking vs. Multiprocessing
- [ ] Multithreading
- [ ] Context switching between processes and threads
- [ ] Inter-Process Communication (IPC): shared memory, message passing, pipes, signals, and sockets

### 3. CPU Scheduling
- [ ] Preemptive vs. Non-Preemptive Scheduling
- [ ] First Come First Served (FCFS)
- [ ] Shortest Job First (SJF)
- [ ] Shortest Remaining Time First (SRTF)
- [ ] Longest Remaining Time First (LRTF)
- [ ] Priority Scheduling
- [ ] Round Robin Scheduling
- [ ] Starvation and Aging
- [ ] Convoy Effect
- [ ] Scheduling metrics:
  - [ ] Arrival Time, Burst Time, and Completion Time
  - [ ] Waiting Time
  - [ ] Turnaround Time
  - [ ] Response Time

**CPU Scheduling Practice**
- [ ] Solve FCFS numerical problems
- [ ] Solve SJF numerical problems
- [ ] Solve SRTF numerical problems
- [ ] Solve LRTF numerical problems
- [ ] Solve Priority Scheduling numerical problems
- [ ] Solve Round Robin numerical problems
- [ ] Draw Gantt charts
- [ ] Calculate Completion, Waiting, Turnaround, and Response Times
- [ ] Compare scheduling algorithms by fairness, overhead, and suitability

### 4. Synchronization and Critical Sections
- [ ] Race Conditions
- [ ] Critical Section Problem
- [ ] Requirements of a correct critical-section solution: Mutual Exclusion, Progress, and Bounded Waiting
- [ ] Mutex vs. Semaphore
- [ ] Binary Semaphore
- [ ] Counting Semaphore
- [ ] Spinlocks
- [ ] Monitors

**Classical Synchronization Problems**
- [ ] Producer-Consumer Problem
- [ ] Readers-Writers Problem
- [ ] Dining Philosophers Problem

### 5. Deadlocks
- [ ] What is a Deadlock?
- [ ] Four Necessary Conditions for Deadlock: Mutual Exclusion, Hold and Wait, No Preemption, and Circular Wait
- [ ] Deadlock Prevention
- [ ] Deadlock Avoidance
- [ ] Deadlock Detection
- [ ] Deadlock Recovery
- [ ] Safe vs. Unsafe States
- [ ] Banker's Algorithm
- [ ] Solve Banker's Algorithm numerical problems

---

## Priority 2: Very Important

### 6. Memory Management
- [ ] Main Memory vs. Secondary Memory
- [ ] Cache: purpose, locality, and cache levels
- [ ] Direct-Mapped vs. Fully Associative Cache Mapping
- [ ] Logical vs. Physical Addresses
- [ ] Dynamic Binding: meaning and role in address binding
- [ ] Contiguous vs. Non-Contiguous Memory Allocation
- [ ] Paging: how it works and why it is used
- [ ] Segmentation
- [ ] Internal vs. External Fragmentation
- [ ] Page Tables
- [ ] Translation Lookaside Buffer (TLB)
- [ ] Virtual Memory
- [ ] Demand Paging
- [ ] Page Faults: causes and handling

### 7. Page Replacement and Virtual-Memory Performance
- [ ] First-In, First-Out (FIFO) Page Replacement
- [ ] Least Recently Used (LRU) Page Replacement
- [ ] Optimal Page Replacement
- [ ] Belady's Anomaly
- [ ] Thrashing: what it is, why it occurs, effects, and prevention

**Page Replacement Practice**
- [ ] Solve FIFO page-replacement problems
- [ ] Solve LRU page-replacement problems
- [ ] Solve Optimal page-replacement problems
- [ ] Calculate page faults for reference strings
- [ ] Demonstrate Belady's Anomaly with an example
- [ ] Explain the relationship between frame allocation, page faults, and thrashing

### 8. File Systems and Storage
- [ ] Files and Directories
- [ ] Basic File-System Structure
- [ ] File Allocation Methods: contiguous, linked, and indexed allocation
- [ ] File Access Methods
- [ ] Inodes
- [ ] File Permissions
- [ ] RAID: purpose, benefits, and common levels (RAID 0, 1, 5, 6, and 10)

---

## Overall Progress Tracker

- [ ] 1. OS Fundamentals and Architecture
- [ ] 2. Processes and Threads
- [ ] 3. CPU Scheduling
- [ ] 4. Synchronization and Critical Sections
- [ ] 5. Deadlocks
- [ ] 6. Memory Management
- [ ] 7. Page Replacement and Virtual-Memory Performance
- [ ] 8. File Systems and Storage

---

## Completion Criteria

Mark a topic complete when you can:

- [ ] Explain the concept in your own words without notes.
- [ ] Answer common OS interview questions about it.
- [ ] Explain differences between related concepts.
- [ ] Solve numerical problems wherever applicable.
- [ ] Give a practical example or use case.
- [ ] Explain the topic clearly enough to teach it to someone else.

## Final Interview Readiness Check

- [ ] Review all Priority 1 topics.
- [ ] Complete CPU scheduling numerical practice, including Gantt charts and all timing metrics.
- [ ] Complete synchronization and deadlock questions, including Banker's Algorithm.
- [ ] Complete page-replacement numerical practice and explain Belady's Anomaly.
- [ ] Practice comparison questions (for example, process vs. thread, mutex vs. semaphore, paging vs. segmentation, and multitasking vs. multiprocessing).
- [ ] Do a mock interview covering all eight sections.
