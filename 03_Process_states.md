**For Better Understanding of concepts first watch the video lecture :** [Watch The Explaination video](https://www.youtube.com/watch?v=2dJdHMpCLIg&list=PLxCzCOWd7aiGz9donHRrE9I3Mwn6XdP8p&index=6&pp=iAQB)

# Process States

The five primary process states are:

- **New**
- **Ready**
- **Running**
- **Waiting / Blocked**
- **Terminated**

Some systems also use additional states, such as **Suspended Ready** and **Suspended Waiting / Blocked**, to describe processes temporarily moved out of main memory.

---

# Primary Process States

## New State

The **New** state represents a process that is being created. For example, a user may launch an application or start a program.

The operating system prepares the process and its required information before it can be scheduled for execution.

## Ready State

A process in the **Ready** state is in main memory (RAM) and is prepared to run, but it is waiting for CPU time.

Ready processes wait in the **ready queue** until the short-term scheduler selects one for execution.

Because RAM is limited, not every process stored in secondary storage can necessarily be loaded into memory at the same time.

## Running State

A process is in the **Running** state when the CPU is executing its instructions.

In a single-processor system, only one process can execute on the CPU at a time. In a multiprocessor system, multiple processes may execute simultaneously on different CPU cores.

A running process may:
- Finish execution and enter the **Terminated** state.
- Request I/O and enter the **Waiting / Blocked** state.
- Be preempted and return to the **Ready** state.

## Waiting / Blocked State

A process enters the **Waiting / Blocked** state when it cannot continue until an event occurs, commonly the completion of an I/O operation.

For example, a process may request data from a disk. Since the process must wait for that operation, the operating system can run another ready process instead of leaving the CPU idle.

When the I/O operation or required event completes, the process returns to the **Ready** state. It does not normally move directly from Waiting to Running; it must wait for CPU scheduling.

## Terminated State

A process enters the **Terminated** state when it finishes execution or is ended by the operating system.

The operating system can then release the resources allocated to the process, such as its memory and other process-specific resources.

---

# Process State Transitions

A process typically moves between the primary states as follows:

- **New → Ready:** The process is admitted to the ready queue.
- **Ready → Running:** The short-term scheduler selects the process, and the dispatcher gives it the CPU.
- **Running → Terminated:** The process finishes execution.
- **Running → Waiting / Blocked:** The process requests I/O or waits for an event.
- **Waiting / Blocked → Ready:** The required I/O operation or event completes.
- **Running → Ready:** The operating system preempts the process, for example when its time quantum expires or a higher-priority process must run.

The last transition is associated with **preemptive scheduling**.

---

# Process Scheduling

The operating system uses schedulers to decide which processes enter memory, which process gets the CPU, and which processes may be temporarily moved out of memory.

## Long-Term Scheduler

The **long-term scheduler** (job scheduler) selects processes from the pool of jobs and admits them into memory for execution.

Its main role is to control which and how many processes are admitted, helping manage the degree of multiprogramming.

## Short-Term Scheduler

The **short-term scheduler** (CPU scheduler) selects a process from the ready queue and chooses it to run on the CPU.

The **dispatcher** then hands control of the CPU to the selected process.

## Medium-Term Scheduler

The **medium-term scheduler** may temporarily remove processes from main memory and later bring them back. This is associated with **swapping**.

It can help manage memory pressure by moving a process out of RAM and bringing it back when appropriate.

---

# Preemptive and Non-Preemptive Scheduling

## Non-Preemptive Scheduling

In **non-preemptive scheduling**, a process is not forcibly taken off the CPU simply because another process becomes ready or a time slice expires. It generally continues until it finishes or blocks, at which point the CPU can be assigned to another process.

## Preemptive Scheduling

In **preemptive scheduling**, the operating system can interrupt a running process and assign the CPU to another process.

This can happen when:
- A higher-priority process needs the CPU.
- The running process's time quantum expires.

The preempted process usually returns to the **Ready** state and can receive another turn later.

## Time Quantum and Round Robin

**Round Robin** is a scheduling algorithm that gives each ready process a fixed time quantum.

For example, if the time quantum is 2 seconds:
- A process can run for up to 2 seconds during its turn.
- If it finishes before the quantum expires, it terminates.
- If it does not finish, it is preempted and returned to the ready queue.
- It can run again when it receives another turn.

---

# Additional Process States

Some process-state models include suspended states. These describe processes that have been temporarily moved out of main memory, commonly through swapping.

## Suspended Waiting / Blocked

A process that is waiting for an I/O operation or event may be moved out of RAM if memory needs to be freed.

It remains unable to run until the required event occurs. After the event completes, it may need to be brought back into memory before it can return to the ready queue.

## Suspended Ready

A process in the **Suspended Ready** state is ready to run once it is back in main memory, but it has been temporarily moved out of RAM.

It can be resumed and returned to the Ready state when memory and scheduling conditions allow.

## Why Are Processes Suspended?

Suppose many processes are waiting for I/O, or the ready queue and memory are under pressure. The operating system may move some processes from RAM to secondary storage to free memory.

This is called **swapping out**. Bringing a process back into RAM is called **swapping in**.

The medium-term scheduler is associated with managing this movement. The exact policies depend on the operating system.

---

# Summary of Process States

| State | Meaning |
|---|---|
| **New** | The process is being created. |
| **Ready** | The process is in memory and waiting for CPU time. |
| **Running** | The CPU is executing the process. |
| **Waiting / Blocked** | The process is waiting for I/O or another event. |
| **Terminated** | The process has finished or has been ended. |
| **Suspended Ready** | The process is ready to run but has been temporarily moved out of main memory. |
| **Suspended Waiting / Blocked** | The process is waiting for an event and has been temporarily moved out of main memory. |

---

# Viewing Processes in an Operating System

- **Windows:** Task Manager displays processes and applications running on the system.
- **Linux / UNIX:** The `ps` command displays information about processes, such as process IDs and commands. Options such as `ps -e` can display a broader list of processes.

---

# Important Definitions

- **Process:** A program in execution.
- **Ready Queue:** The queue of processes in memory that are ready and waiting for CPU time.
- **I/O (Input/Output):** Operations that transfer data between a process and devices or storage.
- **Long-Term Scheduler:** Admits jobs into memory for execution.
- **Short-Term Scheduler:** Selects a ready process to run on the CPU.
- **Medium-Term Scheduler:** Manages the temporary removal and return of processes to memory.
- **Dispatcher:** Transfers CPU control to the process selected by the short-term scheduler.
- **Preemptive Scheduling:** Allows the operating system to interrupt a running process and assign the CPU to another.
- **Non-Preemptive Scheduling:** A running process is not forcibly interrupted simply because another process becomes ready or a time limit expires.
- **Time Quantum:** The maximum time a process is typically allowed to run in one turn under a time-sharing scheduling algorithm.
- **Swapping:** Temporarily moving a process out of main memory to secondary storage and bringing it back later.

---

# Key Points for Exams

- The five primary process states are **New, Ready, Running, Waiting / Blocked, and Terminated**.
- The ready state means a process is prepared to execute but is waiting for CPU time.
- A process usually enters the Waiting / Blocked state when it requests I/O or waits for an event.
- When the I/O operation completes, the process returns to Ready, not directly to Running.
- The long-term scheduler admits jobs into memory.
- The short-term scheduler selects a process from the ready queue for CPU execution.
- The medium-term scheduler is associated with swapping processes out of and back into memory.
- In preemptive scheduling, a running process may be moved back to Ready because its time quantum expires or a higher-priority process needs the CPU.
- Round Robin uses a fixed time quantum for each process's turn.
- Suspended states represent processes temporarily moved out of main memory.
