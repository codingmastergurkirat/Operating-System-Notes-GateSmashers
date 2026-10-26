# Dining Philosophers Problem

## 1. Problem Setup

Five philosophers `P0`–`P4` sit around a circular table with forks `F0`–`F4`. Each philosopher alternates between thinking and eating. To eat, philosopher `Pi` needs two adjacent forks:

- Left fork: `Fi`
- Right fork: `F((i + 1) mod N)`

For `N = 5`:

| Philosopher | Left | Right |
|---|---|---|
| `P0` | `F0` | `F1` |
| `P1` | `F1` | `F2` |
| `P2` | `F2` | `F3` |
| `P3` | `F3` | `F4` |
| `P4` | `F4` | `F0` |

The problem illustrates synchronization, mutual exclusion, deadlock, and concurrent resource use.

## 2. Semaphore Representation

Use one binary semaphore per fork:

```text
S[0], S[1], ..., S[N-1]
```

Every semaphore starts at `1`, meaning the corresponding fork is available. `Wait`/`Down` acquires a fork; `Signal`/`Up` releases it.

For most philosophers:

```text
while (true) {
    think()

    Wait(S[i])
    Wait(S[(i + 1) mod N])

    eat()

    Signal(S[i])
    Signal(S[(i + 1) mod N])
}
```

A fork cannot be held by two philosophers at once. Non-neighbouring philosophers can eat concurrently when their fork sets do not overlap; for example, `P0` uses `F0,F1`, while `P2` uses `F2,F3`.

## 3. Deadlock with the Basic Protocol

If every philosopher picks up the left fork first and is pre-empted before acquiring the right fork, the state becomes:

| Philosopher | Holds | Waits for |
|---|---|---|
| `P0` | `F0` | `F1` held by `P1` |
| `P1` | `F1` | `F2` held by `P2` |
| `P2` | `F2` | `F3` held by `P3` |
| `P3` | `F3` | `F4` held by `P4` |
| `P4` | `F4` | `F0` held by `P0` |

This is a circular wait. Every philosopher holds one resource and waits for another; nobody can acquire the second fork, eat, and release the first.

## 4. Reversing One Philosopher's Order

A common deadlock-avoidance technique is to make one philosopher acquire forks in the opposite order. For example, `P4` takes the right fork first:

```text
// P0 through P3
Wait(S[i])
Wait(S[(i + 1) mod N])

// P4, or any one selected philosopher
Wait(S[(i + 1) mod N])
Wait(S[i])
```

Both forks are released afterward:

```text
Signal(S[i])
Signal(S[(i + 1) mod N])
```

Reversing one order breaks the symmetric pattern in which every philosopher holds a left fork and waits for a right fork. If `P0` already holds `F0`, `P4` blocks while trying to acquire `F0` and does not hold `F4`; consequently, `P3` can acquire `F4`, eat, and release its forks. Under this protocol, circular wait is eliminated.

This technique prevents the described deadlock but does not by itself guarantee fairness. Scheduling can still cause starvation or long waiting.

## 5. Key Lessons

- Per-fork binary semaphores provide mutual exclusion for individual forks.
- Mutual exclusion alone does not prevent deadlock.
- Deadlock occurs when hold-and-wait and circular wait combine with the other Coffman conditions.
- Independent philosophers may eat at the same time.
- Alternative solutions include limiting the number of philosophers attempting to pick up forks, imposing a global resource order, or using a monitor/state-based solution.
- For the lecture's protocol, reversing one philosopher's acquisition order is sufficient to break the circular-wait pattern.