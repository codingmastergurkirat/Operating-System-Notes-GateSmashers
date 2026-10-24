# Types of Operating Systems — Batch Operating System

## 1. Types of Operating Systems

The lecture lists these main types of operating systems:

1. Batch Operating System
2. Multiprogramming Operating System
3. Multitasking Operating System
4. Real-Time Operating System
5. Distributed Operating System
6. Clustered Operating System
7. Embedded Operating System

The lecture identifies **multiprogramming** and **multitasking** as especially important topics. This set is presented as a main classification; other classifications may also exist.

## 2. Batch Operating System

A **Batch Operating System** groups similar jobs into batches and processes them together, without requiring the user to interact directly with the computer while each job runs.

### 2.1 Historical Background

The lecture describes the use of batch systems around the 1960s, when computers were mainly available at large organizations and research institutions rather than in homes.

Examples mentioned include organizations such as NASA and ISRO, which needed computers for calculations and research.

### 2.2 How Jobs Were Submitted

Users prepared their jobs offline using media such as:

- Punch cards
- Paper cards or paper tape
- Magnetic tape

The user then submitted the job to an operator at the computer facility.

The operator:
1. Collected jobs from users.
2. Grouped similar jobs into batches.
3. Submitted each batch for execution.
4. Informed users when results were available.

Users did not normally interact directly with the computer while their jobs were being executed. They might have had to wait a long time before collecting the results.

### 2.3 How a Batch Was Executed

Suppose there are several batches, such as `B1`, `B2`, and `B3`, with similar jobs grouped together.

1. The operating system or operator selects a job from a batch.
2. The job is given to the CPU for execution.
3. If the job requests an input/output (I/O) operation, such as reading a file or accessing a peripheral device, the CPU may have to wait for that operation to complete.
4. In the simple batch system described in the lecture, the CPU remains idle during this wait rather than switching to another job.
5. When the I/O operation finishes, the job can continue using the CPU. The system then proceeds through the queued jobs.

## 3. Main Disadvantage of a Simple Batch System

The lecture emphasizes **CPU idle time** as a major disadvantage.

- A job may need I/O, such as reading from a disk or communicating with a peripheral device.
- While that I/O operation is in progress, the CPU may remain idle.
- The CPU does not make effective use of the waiting time to execute another job.
- This reduces CPU utilization and can increase the time needed to complete the workload.

The lecture contrasts this with multiprogramming, where multiple jobs can be kept in memory and the CPU can switch to another ready job while one job waits for I/O.

## 4. User Experience in Batch Processing

The batch system separated job submission from job execution.

- Users submitted jobs to the operator using cards or tape.
- Users did not know exactly when their jobs would finish.
- Results might be available much later, and users would return to collect them.
- This process was different from modern systems, where users commonly interact with applications directly.

## 5. Later Developments Mentioned

The lecture says that batch systems were refined over time, including the introduction of **monitors**, which reduced the need for operators to manually manage every step and helped users submit jobs more directly.

The lecture mentions these historical names:

- **FORTRAN system**, described in the lecture as designed by IBM.
- **IBSYS 709X**.

The lecture places these examples in the 1960s and presents them as sufficient historical context for its discussion.

## 6. Key Points to Remember

- A **Batch Operating System** groups similar jobs into batches and processes them with little or no direct user interaction during execution.
- Early batch jobs were submitted using punch cards, paper tape, or magnetic tape.
- An operator collected and grouped jobs before execution.
- A major disadvantage of the simple batch system described is **CPU idle time during I/O operations**.
- Multiprogramming improves CPU utilization by allowing another job to run while one job waits for I/O.
- The lecture mentions monitors, FORTRAN system, and IBSYS 709X as historical developments or examples.
- Other operating system types listed in the lecture are multiprogramming, multitasking, real-time, distributed, clustered, and embedded systems.
