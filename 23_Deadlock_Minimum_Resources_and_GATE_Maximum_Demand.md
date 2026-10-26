# Deadlock-Free Resource Guarantees for a Single Resource Type

## 1. General Result

Suppose `n` processes share one interchangeable resource type. Process `Pi` may require at most `Di` units in total. The minimum number of units that guarantees no deadlock is:

```text
Rmin = (D1 - 1) + (D2 - 1) + ... + (Dn - 1) + 1
     = sum(Di) - n + 1
```

Equivalently:

```text
R + n > sum(Di)
```

The strict inequality matters.

Why? A deadlock can still be possible if every process holds one fewer than its maximum demand:

```text
sum(Di - 1) = sum(Di) - n
```

One additional unit guarantees that at least one process can obtain its final required unit, finish, release all its units, and allow the remaining processes to finish in sequence.

Assumptions:

- Resource units are interchangeable.
- Each process eventually releases all resources when it finishes.
- `Di` is the maximum total requirement of process `Pi`.
- The question asks for a guarantee under every possible allocation.

## 2. Three Processes, Two Units Each

Here `n=3` and `D1=D2=D3=2`.

Maximum allocation that can still permit deadlock:

```text
(2-1) + (2-1) + (2-1) = 3
```

Therefore:

```text
Rmin = 3 + 1 = 4
```

- With 2 units, processes can hold one each and wait for another; no process can finish.
- With 3 units, each process can hold one and all can wait for a second.
- With 4 units, one process can be supplied its second unit, finish, and release resources for the others.

**Answer: 4 units.**

## 3. Different Maximum Requirements

If `P1`, `P2`, and `P3` require 3, 4, and 5 units:

```text
Rmin = (3-1) + (4-1) + (5-1) + 1
     = 2 + 3 + 4 + 1
     = 10
```

Nine units can still be allocated as `(2,3,4)`, leaving every process one unit short. Ten units guarantee progress under the model.

## 4. GATE-Style Question: Largest K

There are three processes, four resource instances, and each process may require at most `K` units. We need the largest `K` that guarantees no deadlock.

Use:

```text
R + n > nK
```

Substitute `R=4` and `n=3`:

```text
4 + 3 > 3K
7 > 3K
K < 7/3
```

Since `K` is an integer:

```text
Kmax = 2
```

Why `K=3` fails: allocate 2 units to `P1` and 1 unit each to `P2` and `P3`. All four units are allocated. `P1` needs one more; `P2` and `P3` each need two more. No process can complete, so deadlock is possible.

For equal demand:

```text
Kmax = floor((R + n - 1) / n)
```

For `R=4,n=3`, this is `floor(6/3)=2`.

## 5. Exam Checklist

1. Identify `n`, total units `R`, and each maximum demand.
2. Compute `sum(Di) - n + 1` for the minimum guaranteed total.
3. For equal demand, use `R+n>nK`.
4. Test the next larger integer to show why the answer is maximal.
5. Distinguish a favourable allocation from a guarantee over every possible allocation.