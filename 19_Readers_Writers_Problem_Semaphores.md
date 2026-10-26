# Readers–Writers Problem: Semaphore Solution

## 1. Overview

The Readers–Writers Problem concerns concurrent access to shared data such as a railway-reservation database.

- Readers inspect data and do not modify it.
- Writers modify data.
- Multiple readers may read together.
- A writer requires exclusive access.

Allowed combinations:

| Operations | Allowed? | Reason |
|---|---|---|
| Read–Read | Yes | Reads do not conflict. |
| Read–Write | No | The reader could observe a changing state. |
| Write–Read | No | The reader could observe an incomplete update. |
| Write–Write | No | Concurrent updates can conflict or lose data. |

## 2. Variables and Semaphores

```text
rc = 0       // number of active readers
mutex = 1    // protects rc
 db = 1      // protects the database from writers and the reader group
```

`mutex` is not held throughout the read operation. It only protects changes to `rc`. `db` is held by a writer or by the group of readers.

## 3. Reader Protocol

```text
Reader:
while (true) {
    Down(mutex)
    rc = rc + 1
    if (rc == 1)
        Down(db)
    Up(mutex)

    // Read the database

    Down(mutex)
    rc = rc - 1
    if (rc == 0)
        Up(db)
    Up(mutex)
}
```

The first reader acquires `db` on behalf of all readers. Additional readers only increment `rc`. The last reader releases `db`.

## 4. Writer Protocol

```text
Writer:
while (true) {
    Down(db)

    // Write the database

    Up(db)
}
```

A writer cannot enter while another writer or the reader group holds `db`.

## 5. Execution Examples

### First reader

Initially `rc = 0`, `mutex = 1`, and `db = 1`:

1. `R1` executes `Down(mutex)`.
2. It changes `rc` from 0 to 1.
3. Because it is the first reader, it executes `Down(db)`.
4. It releases `mutex` and reads.

A writer arriving now blocks at `Down(db)`.

### Additional readers

`R2` acquires `mutex`, changes `rc` from 1 to 2, does not acquire `db`, releases `mutex`, and reads concurrently with `R1`. `R3` behaves similarly.

### Reader exits

If `rc = 3`, one reader decrements it to 2 and does not release `db`. The last reader decrements `rc` from 1 to 0 and executes `Up(db)`, allowing a waiting writer to proceed.

### Writer arrives first

A writer changes `db` from 1 to 0 and writes. A reader can increment `rc` to 1, but its `Down(db)` blocks until the writer releases `db`; it cannot read while the writer is active.

## 6. Properties and Limitation

| Case | Result |
|---|---|
| Reader–Reader | Concurrent reading is allowed. |
| Reader–Writer | Writer waits while `db` is held by readers. |
| Writer–Reader | First reader waits for the writer to release `db`. |
| Writer–Writer | Second writer waits for the first. |

This is the classic **reader-preference** solution. If readers continuously arrive, a writer may wait indefinitely, causing writer starvation depending on the semaphore's scheduling policy. Writer-preference or fair reader–writer locks can address that limitation.

## 7. Important Invariants

- `mutex` protects every modification of `rc`.
- `db` is held by either one writer or the reader group, never both.
- The first reader acquires `db`; the last reader releases it.
- Writers acquire and release `db` around their complete write operation.
- `rc` is used while readers join or leave the group under `mutex`, so the protocol remains consistent even when a reader blocks while acquiring `db`.