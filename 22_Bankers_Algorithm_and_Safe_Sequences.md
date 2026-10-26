# Banker's Algorithm and Safe Sequences

## 1. Purpose

Banker's Algorithm is a deadlock-avoidance algorithm. Before granting a resource request, it checks whether the resulting state has a safe completion sequence.

It assumes that each process declares a maximum possible demand.

## 2. Data Structures

- `Allocation[i][j]`: resources currently held by process `i`.
- `Max[i][j]`: maximum total demand of process `i`.
- `Need[i][j] = Max[i][j] - Allocation[i][j]`.
- `Available[j]`: currently unallocated resources.
- `Work`: a working copy of `Available` used by the safety check.

A process is eligible only when its complete Need vector is component-wise less than or equal to Work.

## 3. Safety Algorithm

1. Compute `Need = Max - Allocation`.
2. Set `Work = Available`.
3. Find an unfinished process with `Need <= Work` in every resource column.
4. Assume it completes.
5. Add its **current Allocation**, not its Max or Need, to Work.
6. Mark it finished and repeat.
7. If all processes finish, the state is safe and the selected order is a safe sequence.
8. If no remaining process qualifies, the algorithm cannot establish safety.

A safe sequence is sufficient to prove safety; it need not be unique.

## 4. Worked Example: Five Processes

Resources are `A`, `B`, and `C` with total `(10,5,7)`.

### Allocation

| Process | A | B | C |
|---|---:|---:|---:|
| `P1` | 0 | 1 | 0 |
| `P2` | 2 | 0 | 0 |
| `P3` | 3 | 0 | 2 |
| `P4` | 2 | 1 | 1 |
| `P5` | 0 | 0 | 2 |

Allocated total is `(7,2,5)`, so:

```text
Available = (10,5,7) - (7,2,5) = (3,3,2)
```

### Maximum and Need

| Process | Max A | Max B | Max C | Need A | Need B | Need C |
|---|---:|---:|---:|---:|---:|---:|
| `P1` | 7 | 5 | 3 | 7 | 4 | 3 |
| `P2` | 3 | 2 | 2 | 1 | 2 | 2 |
| `P3` | 9 | 0 | 2 | 6 | 0 | 0 |
| `P4` | 4 | 2 | 2 | 2 | 1 | 1 |
| `P5` | 5 | 3 | 3 | 5 | 3 | 1 |

### Safety sequence

- `P2` needs `(1,2,2)`, so it finishes and releases `(2,0,0)`: Work `(5,3,2)`.
- `P4` needs `(2,1,1)`, releases `(2,1,1)`: Work `(7,4,3)`.
- `P5` needs `(5,3,1)`, releases `(0,0,2)`: Work `(7,4,5)`.
- `P1` needs `(7,4,3)`, releases `(0,1,0)`: Work `(7,5,5)`.
- `P3` needs `(6,0,0)`, releases `(3,0,2)`: Work `(10,5,7)`.

One safe sequence is:

```text
P2 → P4 → P5 → P1 → P3
```

## 5. GATE 2018 Worked Example

Resources are `E`, `F`, and `G`; `Available=(3,3,0)`.

| Process | Allocation E | F | G | Maximum E | F | G | Need E | F | G |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| `P0` | 1 | 0 | 1 | 4 | 3 | 1 | 3 | 3 | 0 |
| `P1` | 1 | 1 | 2 | 2 | 1 | 4 | 1 | 0 | 2 |
| `P2` | 1 | 0 | 3 | 1 | 3 | 3 | 0 | 3 | 0 |
| `P3` | 2 | 0 | 0 | 5 | 4 | 1 | 3 | 4 | 1 |

Safety check:

1. `P0` needs `(3,3,0)`, finishes, releases `(1,0,1)`, giving `(4,3,1)`.
2. `P1` cannot yet finish because it needs two `G` while only one is available.
3. `P2` needs `(0,3,0)`, finishes, releases `(1,0,3)`, giving `(5,3,4)`.
4. `P1` finishes and releases `(1,1,2)`, giving `(6,4,6)`.
5. `P3` finishes and releases `(2,0,0)`, giving `(8,4,6)`.

A safe sequence is:

```text
P0 → P2 → P1 → P3
```

## 6. Exam Rules

- Never confuse the number of bits or columns with the number of resources.
- Calculate `Need` column by column.
- Compare every resource dimension.
- Add the process's existing allocation after it finishes.
- Safe means at least one valid sequence exists.
- Unsafe does not automatically mean that deadlock has already occurred.
- The algorithm's limitation is its requirement for reliable maximum-demand declarations.