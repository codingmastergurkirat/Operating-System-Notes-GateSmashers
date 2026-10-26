# Deadlock and Resource-Allocation Graphs

## 1. Deadlock Definition

Deadlock is a state in which a set of processes cannot progress because each is waiting for a resource or event that depends on another blocked process.

Examples include two cars blocking each other on a narrow road, or two processes acquiring semaphores in opposite orders:

```text
P1: Down(S1); Down(S2)
P2: Down(S2); Down(S1)
```

If `P1` acquires `S1` and `P2` acquires `S2`, both block on their second operation.

## 2. Coffman Conditions

All four conditions must hold simultaneously for deadlock to be possible in the standard resource model:

1. **Mutual exclusion:** at least one resource is non-shareable.
2. **No pre-emption:** a resource cannot be forcibly taken from its holder.
3. **Hold and wait:** a process holds resources while requesting additional resources.
4. **Circular wait:** processes form a cycle in which each waits for a resource held by the next process.

Preventing any one condition can prevent the corresponding deadlock scenario.

### Resource-allocation example

```text
P1 holds R1 and requests R2
P2 holds R2 and requests R1
```

This has mutual exclusion, no pre-emption, hold and wait, and circular wait.

## 3. Resource-Allocation Graph (RAG)

A RAG contains:

- Circle vertices for processes.
- Rectangle vertices for resource types.
- `Resource → Process`: allocation; the process holds an instance.
- `Process → Resource`: request; the process waits for an instance.

For example:

```text
R1 → P1       P1 → R2
R2 → P2       P2 → R1
```

This produces the cycle:

```text
P1 → R2 → P2 → R1 → P1
```

## 4. Single-Instance Resources

If every resource type has exactly one instance, a cycle exists **if and only if** deadlock exists.

Example:

| Process | Allocation R1 | Allocation R2 | Request R1 | Request R2 |
|---|---:|---:|---:|---:|
| `P1` | 1 | 0 | 0 | 1 |
| `P2` | 0 | 1 | 1 | 0 |

Available is `(0,0)`. Neither outstanding request can be satisfied, so both processes remain blocked and deadlock exists.

## 5. Multiple-Instance Resources

When at least one resource type has multiple instances, a cycle alone does not prove deadlock. A process outside the apparent cycle may finish and release an instance, allowing the cycle to dissolve.

Use three structures:

- `Allocation`: instances currently held by each process.
- `Request`: additional instances currently requested.
- `Available`: unallocated instances.

Repeatedly find a process whose `Request` is component-wise less than or equal to `Available`. Assume it finishes, add its allocation to `Available`, and continue. If all processes can finish, there is no deadlock. Processes left unfinishable are deadlocked under the model.

### Multiple-instance example with a cycle but no deadlock

`R1` has one instance and `R2` has two:

| Process | Allocation R1 | Allocation R2 | Request R1 | Request R2 |
|---|---:|---:|---:|---:|
| `P1` | 1 | 0 | 0 | 1 |
| `P2` | 0 | 1 | 1 | 0 |
| `P3` | 0 | 1 | 0 | 0 |

Initially `Available=(0,0)`. `P3` requests nothing, so it finishes and releases `(0,1)`, giving `Available=(0,1)`. Then `P1` can finish and release `(1,0)`, giving `(1,1)`, and `P2` can finish. A completion sequence is `P3 → P1 → P2`; therefore there is no deadlock despite the `P1`/`P2` cycle.

### Four-process example

Totals are `R1=2`, `R2=3`, and `R3=2`, with initial `Available=(0,0,1)`:

| Process | Alloc R1 | Alloc R2 | Alloc R3 | Req R1 | Req R2 | Req R3 |
|---|---:|---:|---:|---:|---:|---:|
| `P0` | 1 | 0 | 1 | 0 | 1 | 1 |
| `P1` | 1 | 1 | 0 | 1 | 0 | 0 |
| `P2` | 0 | 1 | 0 | 0 | 0 | 1 |
| `P3` | 0 | 1 | 0 | 1 | 2 | 0 |

A valid detection sequence is:

1. `P2` uses the available `R3` and releases `(0,1,0)`: `Available=(0,1,1)`.
2. `P0` finishes and releases `(1,0,1)`: `Available=(1,1,2)`.
3. `P1` finishes and releases `(1,1,0)`: `Available=(2,2,2)`.
4. `P3` finishes and releases `(0,1,0)`: `Available=(2,3,2)`.

All processes finish, so the state is not deadlocked.

## 6. Deadlock vs. Starvation

Deadlock is permanent mutual blocking among a dependency set. Starvation is repeated or indefinite denial of service to one process while others continue making progress. A finite wait is not automatically starvation.

## 7. Practical Detection Procedure

1. List processes, resource types, and instance counts.
2. Mark allocation and request edges.
3. For single-instance resources, check for a cycle.
4. For multiple instances, construct Allocation, Request, and Available.
5. Repeatedly finish any process whose request can be satisfied.
6. Add its allocation back to Available.
7. If all finish, there is no deadlock; otherwise the remaining processes may be deadlocked.

A cycle is conclusive only in the single-instance case.