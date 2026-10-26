# Methods for Handling Deadlock

## 1. Four Broad Approaches

Operating systems commonly discuss four approaches:

1. Deadlock ignorance (the Ostrich method)
2. Deadlock prevention
3. Deadlock avoidance
4. Deadlock detection and recovery

## 2. Deadlock Ignorance

The Ostrich approach does not continuously detect or prevent every possible deadlock. General-purpose systems may accept this trade-off when deadlocks are relatively uncommon and comprehensive handling would add complexity or runtime overhead. A user or administrator may restart an unresponsive system, but restarting is not a universal recovery guarantee.

**Advantage:** low implementation and runtime overhead.

**Disadvantage:** deadlocked processes may stop making progress and may require intervention.

## 3. Prevention

Prevention makes at least one Coffman condition impossible.

### Prevent mutual exclusion
Make a resource shareable where safe. This cannot be done for inherently non-shareable resources such as a printer executing one conflicting job at a time.

### Allow pre-emption
Reclaim a resource from a process waiting for another resource, if the resource and process state can safely be restored. CPU time-slice pre-emption is not the same as reclaiming every resource held by a process.

### Prevent hold and wait
Require a process to request all resources before it begins, or require it to release currently held resources before requesting more. This can waste resources and requires processes to know their needs in advance.

### Prevent circular wait
Impose a strict ordering on resource types and require processes to request resources only in increasing order. For example, a process that has requested resource 3 cannot later request resource 1.

## 4. Avoidance

Avoidance evaluates a request before granting it and grants it only if the resulting state remains safe. A safe state has at least one safe sequence in which every process can obtain its remaining needs, finish, and release its resources.

Banker's Algorithm is the standard example. An unsafe state is not necessarily already deadlocked; it means the algorithm cannot guarantee a completion sequence under its assumptions. Avoidance requires advance information about maximum resource demands, which may be unavailable in real workloads.

## 5. Detection and Recovery

The system permits allocations, periodically detects deadlock, and recovers when necessary.

### Terminate processes
Terminate one or more processes involved in the deadlock, possibly one at a time while rerunning detection. Victim selection may consider priority, work lost, restart cost, and resources held.

### Resource pre-emption
Take a resource from a selected process and assign it to another. The system must choose a victim, determine whether rollback is possible, restore the victim safely, and avoid repeatedly selecting the same victim. Some resources cannot be pre-empted safely.

## 6. Comparison

| Approach | Main idea | Trade-off |
|---|---|---|
| Ignorance | Do not explicitly handle every deadlock | Low overhead; intervention may be needed. |
| Prevention | Eliminate a necessary condition | Strong guarantee but may reduce flexibility or utilization. |
| Avoidance | Grant only requests preserving a safe state | Requires maximum-demand information and checking overhead. |
| Detection/recovery | Detect after occurrence and break the cycle | Flexible allocation but recovery may lose work. |

## 7. Key Distinction

Prevention changes the rules so deadlock cannot form. Avoidance makes allocation decisions using a safety test. Detection and recovery allow deadlock and repair it afterward. An unsafe state is different from a confirmed deadlock.