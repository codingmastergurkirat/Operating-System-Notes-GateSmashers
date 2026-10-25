# Critical Section and Process Synchronization

## Critical Section

A **critical section** is the part of a program that accesses shared data or resources and may conflict with another process. Code using only private local data can be outside the critical section.

A process is commonly structured as:

1. **Entry section:** requests permission to enter.
2. **Critical section:** accesses shared state.
3. **Exit section:** releases access.
4. **Remainder section:** performs other work.

```text
while (true) {
    // Entry section
    // Critical section
    // Exit section
    // Remainder section
}
```

## Why Protection Is Needed

Operations such as `count++` and `count--` can be implemented as load, modify, and store steps. If these steps interleave, one process can overwrite another's update, producing a race condition. Synchronization prevents conflicting processes from entering the protected region simultaneously.

## Requirements for a Correct Solution

### Mutual Exclusion

At most one process can be inside a critical section protecting the same shared resource at a time.

### Progress

If the critical section is empty and processes want to enter, the decision about who enters cannot be postponed indefinitely by processes that are not trying to enter.

### Bounded Waiting

After a process requests entry, there must be a limit on how many times other processes may enter before it is allowed to enter. This prevents indefinite postponement or starvation.

### No Hardware or Speed Assumptions

Correctness must not depend on a particular processor speed, clock rate, or timing relationship between processes. The method should be portable and based on synchronization logic rather than lucky execution speed.

## Methods Mentioned

The lecture introduces semaphores, monitors, lock variables, and Test-and-Set Lock (TSL) as synchronization approaches.

### Quick Revision

- Critical section: accesses shared data or resources.
- Entry section: requests access.
- Exit section: releases access.
- Mutual exclusion: no simultaneous conflicting access.
- Progress: interested processes are not blocked by uninterested ones.
- Bounded waiting: no process waits forever.
- Correctness should not depend on hardware speed.
