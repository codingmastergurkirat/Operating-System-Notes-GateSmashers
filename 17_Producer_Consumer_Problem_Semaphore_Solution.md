# Producer–Consumer Problem: Semaphore Solution

## 1. Overview

The Producer–Consumer Problem concerns producer and consumer processes sharing a finite circular buffer. Producers add items; consumers remove items. Without synchronization, concurrent access can cause race conditions, lost data, inconsistent indices, or buffer overflow/underflow.

This solution uses:

- `empty`: counting semaphore containing the number of empty slots.
- `full`: counting semaphore containing the number of filled slots.
- `S`: binary semaphore protecting the shared-buffer critical section.
- `IN` and `OUT`: circular-buffer indices.

For a buffer of capacity `n`, the normal invariant is:

```text
empty + full = n
```

## 2. Semaphore Roles

| Semaphore | Type | Initial value | Purpose |
|---|---|---:|---|
| `empty` | Counting | `n` or the current number of empty slots | Prevents production when the buffer is full. |
| `full` | Counting | `0` or the current number of filled slots | Prevents consumption when the buffer is empty. |
| `S` | Binary | `1` | Provides mutual exclusion for shared-buffer operations. |

`Down`/`Wait`/`P` acquires a semaphore and may block. `Up`/`Signal`/`V` releases it.

## 3. Producer Protocol

```text
while (true) {
    item = produce()

    Down(empty)
    Down(S)

    Buffer[IN] = item
    IN = (IN + 1) mod n

    Up(S)
    Up(full)
}
```

`Down(empty)` reserves a slot before the producer enters the buffer. `Down(S)` protects the write and index update. After insertion, `Up(S)` releases the critical section and `Up(full)` announces that one more item is available.

## 4. Consumer Protocol

```text
while (true) {
    Down(full)
    Down(S)

    item = Buffer[OUT]
    OUT = (OUT + 1) mod n

    Up(S)
    Up(empty)

    consume(item)
}
```

`Down(full)` reserves an available item. The consumer then acquires `S`, reads the item, advances `OUT`, releases the critical section, and signals that one more slot is empty.

## 5. Sequential Example

For an eight-slot buffer initially containing `a`, `b`, and `c`:

```text
empty = 5, full = 3, S = 1, IN = 3, OUT = 0
```

### Producer adds `d`

| Operation | `empty` | `full` | `S` | Effect |
|---|---:|---:|---:|---|
| `Down(empty)` | 4 | 3 | 1 | Reserves slot 3. |
| `Down(S)` | 4 | 3 | 0 | Enters critical section. |
| Write `Buffer[3] = d` | 4 | 3 | 0 | Stores the item. |
| `IN = (3+1) mod 8` | 4 | 3 | 0 | `IN` becomes 4. |
| `Up(S)` | 4 | 3 | 1 | Leaves critical section. |
| `Up(full)` | 4 | 4 | 1 | Records the new item. |

### Consumer removes `a`

| Operation | `empty` | `full` | `S` | Effect |
|---|---:|---:|---:|---|
| `Down(full)` | 4 | 3 | 1 | Reserves an item. |
| `Down(S)` | 4 | 3 | 0 | Enters critical section. |
| Read `Buffer[0]` | 4 | 3 | 0 | Reads `a`. |
| `OUT = (0+1) mod 8` | 4 | 3 | 0 | `OUT` becomes 1. |
| `Up(S)` | 4 | 3 | 1 | Leaves critical section. |
| `Up(empty)` | 5 | 3 | 1 | Records the free slot. |

## 6. Pre-emption and Concurrency

Suppose a producer performs `Down(empty)`, changing `empty` from 5 to 4, and is pre-empted before acquiring `S`. A consumer can then perform `Down(full)` and `Down(S)`, enter the critical section, and remove an item. If the producer resumes while `S = 0`, its `Down(S)` blocks. It cannot modify the buffer until the consumer executes `Up(S)`.

This demonstrates the distinction between concurrency and mutual exclusion:

- Both processes may be ready and may be interleaved by the scheduler.
- Only one process can hold `S` and execute the protected buffer operations.
- `empty` and `full` control availability; `S` controls exclusive access.

## 7. Correctness Rules

- Producer: `Down(empty)` before insertion and `Up(full)` after insertion.
- Consumer: `Down(full)` before removal and `Up(empty)` after removal.
- Both processes acquire `S` before accessing shared buffer state.
- `IN = (IN + 1) mod n` and `OUT = (OUT + 1) mod n` implement circular indexing.
- The semaphore operations must be atomic.
- The order shown avoids entering the critical section when no slot or item is available.

Related synchronization solutions may use mutexes or locks, but the availability and mutual-exclusion responsibilities must still be preserved.