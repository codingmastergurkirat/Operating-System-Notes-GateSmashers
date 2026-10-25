# Binary Semaphore

## Overview

A semaphore is a synchronization tool for coordinating concurrent cooperative processes. A **binary semaphore** has values `0` and `1` and is commonly used for mutual exclusion. A counting semaphore represents multiple available resource instances.

Operation names include:

| Acquire | Release |
|---|---|
| `Down(S)` | `Up(S)` |
| `P(S)` | `V(S)` |
| `Wait(S)` | `Signal(S)` |
|  | `Post` / `Release` |

For a simple critical-section lock, initialize `S = 1`:

- `S = 1`: available
- `S = 0`: unavailable

## Down / Wait

A simplified operation is:

```text
Down(S):
    if S == 1:
        S = 0
        allow the process to proceed
    else:
        block the process and place its PCB in the blocked list
```

A successful Down changes `1` to `0`. If the semaphore is already `0`, the process blocks rather than entering.

## Up / Signal

```text
Up(S):
    if the blocked list is not empty:
        select and wake a waiting process
        move it to the ready queue
    else:
        S = 1
```

The awakened process becomes ready; it does not necessarily execute immediately. Scheduling determines when it runs. FIFO may be used to select a blocked process, depending on implementation.

## Example

With `S = 1`, P2 may perform Down first, change `S` to `0`, and enter. P1's Down then blocks. When P2 exits and performs Up, P1 is awakened and can later run and acquire access. Only one process proceeds through the critical section at a time.

If a semaphore is initialized to `0` and no process signals it, processes performing Down block immediately.

## Binary vs. Counting

| Feature | Binary | Counting |
|---|---|---|
| Values | `0` or `1` | Resource count/range |
| Typical use | One critical section | Multiple resource instances |
| Down | Acquire or block | Decrement or block according to rules |
| Up | Wake a waiter or make available | Increment or wake a waiter |

### Key Takeaways

- `Down`, `P`, and `Wait` acquire a semaphore.
- `Up`, `V`, and `Signal` release it or wake a waiter.
- Initialize a simple mutual-exclusion semaphore to `1`.
- A blocked process moves to the ready state only after being awakened.
