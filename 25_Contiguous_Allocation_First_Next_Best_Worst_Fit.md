# Contiguous Allocation: First Fit, Next Fit, Best Fit, and Worst Fit

## 1. Allocation Problem

In variable partitioning, released regions become holes. Each new process must fit in one contiguous hole, so the OS must select a suitable hole.

## 2. Algorithms

### First Fit

Scan from the beginning and use the first hole large enough. A 15 KB request placed in a 25 KB hole leaves 10 KB. First Fit is simple and often fast because it may stop early.

### Next Fit

Like First Fit, but begin searching from the location of the most recent allocation rather than the beginning. It may reduce repeated scanning from the start, but performance depends on the hole layout.

### Best Fit

Search all holes and choose the smallest hole that can satisfy the request. A 15 KB request chooses a 20 KB hole over 25 KB, 40 KB, and 100 KB, leaving 5 KB. It can create many small unusable holes and often requires a full search.

### Worst Fit

Choose the largest suitable hole. A 15 KB request in a 100 KB hole leaves 85 KB. The remaining large hole may serve later requests, but repeated selection of the largest hole does not guarantee good overall utilization.

In variable partitioning, leftovers are free holes, not internal fragmentation inside a fixed partition.

## 3. Worked Example

Initial holes:

```text
150 KB, 350 KB
```

Requests in order:

```text
P1=300 KB, P2=25 KB, P3=125 KB, P4=50 KB
```

### First Fit

| Request | Selected hole | Remainder |
|---|---:|---:|
| `P1=300` | 350 | 50 |
| `P2=25` | 150 | 125 |
| `P3=125` | 125 | 0 |
| `P4=50` | 50 | 0 |

All requests succeed.

### Best Fit

| Request | Selected hole | Remainder |
|---|---:|---:|
| `P1=300` | 350 | 50 |
| `P2=25` | 50 | 25 |
| `P3=125` | 150 | 25 |
| `P4=50` | None | — |

The final request fails because the two 25 KB holes cannot be combined for a contiguous 50 KB allocation.

### Worst Fit

| Request | Selected hole | Remainder |
|---|---:|---:|
| `P1=300` | 350 | 50 |
| `P2=25` | 150 | 125 |
| `P3=125` | 125 | 0 |
| `P4=50` | 50 | 0 |

All requests succeed.

## 4. Comparison

| Algorithm | Search starts at | Chooses |
|---|---|---|
| First Fit | Beginning | First suitable hole |
| Next Fit | Last allocation position | First suitable hole from there |
| Best Fit | Usually beginning/full scan | Smallest suitable hole |
| Worst Fit | Usually full scan | Largest suitable hole |

## 5. Exam Procedure

1. Preserve request order.
2. Update the selected hole after every allocation.
3. Do not combine separate holes for one contiguous request.
4. First Fit restarts at the beginning.
5. Next Fit continues from its remembered pointer.
6. Best Fit minimizes the selected hole's remainder.
7. Worst Fit maximizes the selected hole's remainder.
8. Judge the entire request sequence, not one allocation in isolation.