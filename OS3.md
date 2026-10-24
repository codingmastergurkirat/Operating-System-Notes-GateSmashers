# Multiprogramming vs. Multitasking (Time-Sharing) Operating Systems

## 1. Overview

The lecture focuses on two important operating system concepts:

- **Multiprogramming**
- **Multitasking**, also called **time-sharing** in the lecture

The main difference discussed is how the CPU schedules processes and how quickly processes receive attention.

## 2. Multiprogramming Operating System

The concept of **multiprogramming** is to keep multiple processes in main memory (RAM), so the CPU has other work available when a running process cannot continue.

### How It Works

1. Multiple processes are loaded into RAM, subject to the available memory.
2. The operating system gives a process, such as `P1`, to the CPU.
3. In the lecture's simplified description, the CPU continues executing that process until it finishes or requests an I/O operation.
4. If the process requests I/O, the CPU can switch to another available process.
5. Keeping multiple jobs available helps reduce CPU idle time.

The lecture describes multiprogramming scheduling as **generally non-preemptive**: a process is not interrupted merely because its allotted time has expired. The CPU switches when the process finishes or blocks, for example while waiting for I/O.

### Example: Students and Questions

Imagine 10 students, each with 5 questions to solve. The CPU represents the person helping them.

- The person helps Student 1 with all 5 questions before moving to Student 2.
- If Student 1 needs to leave for an I/O-like activity, the person can help another student while Student 1 is away.
- Otherwise, Student 1 continues until the work is complete.

This represents the lecture's explanation of multiprogramming.

### Main Advantage

- **Reduced CPU idle time:** If one process waits for I/O, another process can use the CPU.

## 3. Multitasking / Time-Sharing Operating System

In **multitasking (time-sharing)**, the CPU gives each process a limited time interval, often called a **time slice** or **time quantum**.

### How It Works

1. The CPU executes a process, such as `P1`, for a fixed time slice.
2. If the process finishes within that time, execution is complete.
3. If it does not finish, the operating system can pre-empt it and schedule another ready process, such as `P2`.
4. The CPU later returns to `P1` so it can continue.
5. This switching allows multiple processes to make progress and helps applications respond promptly.

This is **pre-emptive scheduling** because the operating system can interrupt a process when its time slice expires.

### Example: Students and Questions

Imagine the same 10 students, each with 5 questions.

- The person helps Student 1 with 2 questions, then moves to Student 2.
- Next, the person helps Student 3 with 2 questions, and continues through the students.
- After giving each student a turn, the person returns to Student 1 and continues with the remaining questions.

The limited turns represent CPU time slices.

### Main Advantages

- **Responsiveness:** Each process gets a chance to run without waiting for one process to finish all its work.
- **Reduced CPU idle time:** As in multiprogramming, the CPU can run another ready process when one process cannot use it.

## 4. Key Differences

| Point | Multiprogramming | Multitasking / Time-Sharing |
|---|---|---|
| Basic idea | Keep multiple processes in memory so the CPU has work available | Share CPU time among processes using time slices |
| Scheduling described in the lecture | Generally non-preemptive | Pre-emptive |
| When a running process is switched | Commonly when it finishes or blocks for I/O | It may be interrupted when its time slice expires |
| Main emphasis in the lecture | Keeping CPU idle time low | Responsiveness and quicker turns for processes |
| User experience | A process may wait longer before getting another turn | Processes get frequent turns, supporting interactive use |
| Example | Finish one student's set of questions before moving to the next, unless the student must wait for I/O | Give each student a limited number of questions per turn, then rotate |

## 5. Round-Robin Scheduling

The lecture mentions **Round Robin** as an example of a scheduling algorithm used in time-sharing systems.

In Round Robin:
- Each ready process receives a time quantum.
- When its quantum expires, the process is pre-empted if it has not finished.
- It is placed back in the ready queue, and the next process gets a turn.

The lecture notes that the details of scheduling algorithms are a separate topic.

## 6. Key Points to Remember

- **Multiprogramming:** Multiple processes are kept in RAM to help keep the CPU busy.
- In the lecture's description, multiprogramming is generally **non-preemptive** and switches when a process finishes or waits for I/O.
- **Multitasking/time-sharing:** The CPU gives processes limited time slices.
- Multitasking uses **pre-emptive scheduling** in the example discussed.
- Multiprogramming emphasizes reducing CPU idle time; time-sharing emphasizes responsiveness as well as keeping the CPU busy.
- **Round Robin** is a common time-sharing scheduling algorithm.
