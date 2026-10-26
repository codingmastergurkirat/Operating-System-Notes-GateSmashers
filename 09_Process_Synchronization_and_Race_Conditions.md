# Process Synchronization and Race Conditions

## Overview

Process synchronization coordinates concurrent processes that share data or resources. On a single processor, concurrency can occur through scheduling and context switching even though only one process executes at one instant.

- **Cooperative processes** can affect one another through shared variables, memory, buffers, code, or resources.
- **Independent processes** do not affect one another in the relevant sense.

## Shared-Variable Example

Suppose `shared` initially equals `5`:

```c
// P1
X = shared;
X++;
sleep(1);
shared = X;

// P2
Y = shared;
Y--;
sleep(1);
shared = Y;
```

A possible execution is:

1. P1 reads 5 and sets `X = 6`.
2. P1 sleeps.
3. P2 reads 5 and sets `Y = 4`.
4. P2 sleeps.
5. P1 writes 6.
6. P2 writes 4.

The final value is `4`, although the expected coordinated result is `5 + 1 - 1 = 5`. Another interleaving can produce `6`. The later write overwrites the other process's update.

## Race Condition

A **race condition** occurs when the result depends on the timing or interleaving of concurrent operations on shared state. The problem is not simply simultaneous execution; it is the uncoordinated interleaving of operations that should be treated as a protected sequence.

## Context Switching

When a process calls `sleep(1)`, the process becomes blocked; the CPU does not sleep. The operating system saves the process context, including information represented in the PCB and program counter, and schedules another ready process. When the process resumes, it continues from its saved execution point.

## Synchronization Approaches

Synchronization techniques introduced in the lecture include semaphores, Peterson's solution, locks, and other critical-section mechanisms. They coordinate access to shared state and prevent inconsistent results.

### Key Takeaways

- Synchronization is needed for cooperative processes sharing data or resources.
- Concurrent unsynchronized operations can produce nondeterministic results.
- `sleep(1)` blocks a process, not the CPU.
- A context switch saves one process's state and restores another's.
- A race condition results from timing-dependent access to shared state.
