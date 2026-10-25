# Test-and-Set Instruction

## Overview

Test-and-Set is a hardware-supported synchronization method. It combines testing a lock and setting it into one **atomic operation**, so another process cannot interrupt the operation halfway through.

The basic use is:

```c
while (TestAndSet(&lock))
    ;

// Critical section

lock = false;
```

The lock starts as `false`. Test-and-Set returns the old value and sets the target to `true`.

```c
boolean TestAndSet(boolean *target) {
    boolean r = *target;
    *target = true;
    return r;
}
```

This pseudocode only illustrates the behavior. The real operation must be atomic through hardware support; an ordinary non-atomic function would not provide the guarantee.

## How It Works

If `lock = false`, P1's atomic operation returns `false` while setting the lock to `true`. The loop ends and P1 enters the critical section.

If P2 tries while P1 is inside, the operation returns `true`, so P2 remains in the busy-wait loop. After P1 leaves and sets `lock = false`, a later Test-and-Set can succeed.

## Improvement and Limitation

Test-and-Set prevents two processes from both observing an unlocked state and claiming it. It provides mutual exclusion when used correctly. However, the loop is busy waiting, consuming CPU while waiting, and basic Test-and-Set does not by itself guarantee bounded waiting; starvation is possible if other processes repeatedly acquire the lock.

### Key Takeaways

- Test-and-Set makes checking and claiming atomic.
- It returns the previous lock value and sets the lock to `true`.
- A returned `false` permits entry; a returned `true` causes waiting.
- The lock is released by setting it to `false`.
- Hardware atomicity is essential.
