# Printer-Spooler Problem

## Overview

A printer spooler stores documents in a shared directory so a slow printer can process them sequentially. Multiple user processes may add documents concurrently. The shared variable `in` points to the next available slot.

Each process performs:

```text
Ri = in
spooler_directory[Ri] = filename
Ri = Ri + 1
in = Ri
```

`Ri` is a process-local register; `in` and the spooler directory are shared.

## Race Condition

Assume slots `0`, `1`, and `2` contain `f1.DOC`, `f2.DOC`, and `f3.DOC`, and `in = 3`. P1 wants to add `f4.DOC`; P2 wants to add `f5.DOC`.

1. P1 loads `in = 3` into `R1`.
2. P1 writes `f4.DOC` to slot `3`.
3. P1 increments `R1` to `4` but is pre-empted before updating `in`.
4. P2 loads the still-visible `in = 3` into `R2`.
5. P2 writes `f5.DOC` to slot `3`, overwriting `f4.DOC`.
6. P2 increments `R2` and stores `4` in `in`.
7. P1 resumes and also stores `4` in `in`.

Only one document remains in slot `3`, and `f4.DOC` is lost. The pointer advanced once even though two processes attempted to add documents.

## Cause and Solution

Selecting a slot, writing the filename, and advancing `in` must be coordinated as one protected operation. Without synchronization, two processes can read the same old value and reserve the same slot.

The lecture names semaphores and monitors as possible solutions. The problem is another example of a race condition among cooperative processes.

### Key Takeaways

- A spooler queues print jobs for a slow printer.
- `in` identifies the next available slot.
- Separate load, write, increment, and store instructions can interleave.
- Two processes can overwrite one document and cause data loss.
- Synchronization must protect slot reservation and insertion.
