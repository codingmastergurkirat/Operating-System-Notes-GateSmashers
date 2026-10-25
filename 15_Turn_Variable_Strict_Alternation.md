# Turn Variable (Strict Alternation Method)

## Overview

The turn-variable method, or strict alternation, is a software solution for two processes, `P0` and `P1`. A shared integer `turn` indicates which process may enter.

- `turn = 0`: P0's turn
- `turn = 1`: P1's turn

```c
// P0
while (turn != 0)
    ;
// Critical section
turn = 1;

// P1
while (turn != 1)
    ;
// Critical section
turn = 0;
```

## Mutual Exclusion

When `turn = 0`, P1 cannot pass its entry condition; when `turn = 1`, P0 cannot pass. Thus both processes cannot enter their conflicting critical sections at the same time.

## Progress Failure

Suppose `turn = 0`, the critical section is empty, P1 wants to enter, and P0 is not interested in entering. P1 waits because it is not its turn. If P0 never enters and exits, it never changes `turn` to `1`, so P1 can wait indefinitely. Therefore strict alternation does not satisfy progress.

## Bounded Waiting and Independence

When both processes continue to participate, the turn alternates and neither can repeatedly enter while ignoring the other, so the lecture treats bounded waiting as satisfied. The method uses ordinary shared-variable logic and does not depend on a particular processor speed or hardware instruction.

| Requirement | Result |
|---|---|
| Mutual exclusion | Satisfied |
| Progress | Not satisfied |
| Bounded waiting | Satisfied as presented when both participate |
| Hardware/speed independence | Satisfied as presented |

### Key Takeaways

- Strict alternation is designed for two processes.
- The process whose identifier equals `turn` may enter.
- It guarantees mutual exclusion.
- It can block a willing process when the other process is uninterested, violating progress.
