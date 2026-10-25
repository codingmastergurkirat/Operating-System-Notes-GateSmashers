# Lock Variable Method for the Critical-Section Problem

## Overview

The lock-variable method uses a shared variable to indicate whether a critical section is occupied:

- `lock = 0`: free
- `lock = 1`: occupied

Teaching pseudocode:

```c
while (lock == 1)
    ;

lock = 1;

// Critical section

lock = 0;
```

## When It Appears to Work

If P1 checks while `lock == 0`, immediately sets `lock = 1`, and enters, P2 sees `lock == 1` and waits. When P1 exits and sets the lock to `0`, P2 can proceed. This execution appears correct.

## Failure Under Pre-emption

The check and assignment are separate operations:

1. Initially `lock = 0`.
2. P1 checks the lock and passes.
3. P1 is pre-empted before setting `lock = 1`.
4. P2 checks and also sees `0`.
5. P2 sets the lock and enters.
6. P1 resumes, sets the lock, and enters too.

Both processes are now in the critical section, violating mutual exclusion.

## Main Limitation

The method does not atomically combine checking whether the lock is free with claiming it. Therefore, it does not reliably solve the critical-section problem. The lecture introduces Test-and-Set as a way to address this weakness.

### Key Takeaways

- The lock variable is shared.
- The simple method can work in one execution order.
- Pre-emption between the check and assignment allows two processes to enter.
- A correct solution needs an atomic claim operation.
