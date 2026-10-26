# TLB Effective Memory Access Time: Worked Numerical Example

## 1. Problem Setup

The lecture considers a **single-level paging system** with a Translation Lookaside Buffer (TLB).

Given:

- Main-memory access time: **100 ns**
- TLB lookup time: **20 ns**
- Disk page-transfer time: **5,000 ns per transfer**
- TLB hit ratio: **95%**
- Page-fault rate: **10%** (treated here as conditional on a TLB miss, following the lecture's intended calculation)
- Of the page faults, **20% require a dirty page to be written back to disk** before the requested page can be loaded.

Find the effective memory access time (EMAT).

## 2. Access Paths

### Case A: TLB hit

The translation is found in the TLB, so the system:
1. Looks up the TLB.
2. Accesses the requested data in main memory.

\[
T_{\text{hit}} = 20 + 100 = 120\text{ ns}
\]

Probability:

\[
P(\text{TLB hit}) = 0.95
\]

### Case B: TLB miss, page resident in main memory

The translation is not in the TLB, so the system checks the page table in main memory. If the page is resident, it then accesses the requested data in main memory.

\[
T_{\text{miss, resident}} = 20 + 100 + 100 = 220\text{ ns}
\]

The two main-memory accesses are:
- One to read the page-table entry
- One to access the requested data

The TLB miss probability is:

\[
P(\text{TLB miss}) = 1 - 0.95 = 0.05
\]

Under the lecture's intended interpretation, 90% of TLB misses do not cause a page fault:

\[
P(\text{miss, resident}) = 0.05 \times 0.90 = 0.045
\]

### Case C: Page fault, no dirty-page write-back

If the requested page is not in main memory, the system must transfer it from disk. In this case, no dirty page needs to be written back.

\[
T_{\text{fault, clean}} = 20 + 100 + 5000 = 5120\text{ ns}
\]

Probability:

\[
P(\text{fault, clean}) = 0.05 \times 0.10 \times 0.80 = 0.004
\]

### Case D: Page fault, dirty-page write-back required

In 20% of page faults, a dirty page must first be written from main memory to disk to free a frame. The requested page is then read from disk.

This requires two disk transfers:

\[
T_{\text{fault, dirty}} = 20 + 100 + 5000 + 5000 = 10120\text{ ns}
\]

Probability:

\[
P(\text{fault, dirty}) = 0.05 \times 0.10 \times 0.20 = 0.001
\]

## 3. Effective Memory Access Time

Using the four mutually exclusive paths:

\[
\begin{aligned}
EMAT ={}& 0.95(120) \\
&+ (0.05 \times 0.90)(220) \\
&+ (0.05 \times 0.10 \times 0.80)(5120) \\
&+ (0.05 \times 0.10 \times 0.20)(10120)
\end{aligned}
\]

Calculate each contribution:

\[
0.95(120) = 114
\]

\[
0.05 \times 0.90 \times 220 = 9.9
\]

\[
0.05 \times 0.10 \times 0.80 \times 5120 = 20.48
\]

\[
0.05 \times 0.10 \times 0.20 \times 10120 = 10.12
\]

Therefore:

\[
EMAT = 114 + 9.9 + 20.48 + 10.12
\]

\[
\boxed{EMAT = 154.5\text{ ns} \approx 155\text{ ns}}
\]

## 4. Probability Check

Under this interpretation, the four path probabilities sum to 1:

- TLB hit: \(0.95\)
- TLB miss, page resident: \(0.05 \times 0.90 = 0.045\)
- TLB miss, page fault without dirty write-back: \(0.05 \times 0.10 \times 0.80 = 0.004\)
- TLB miss, page fault with dirty write-back: \(0.05 \times 0.10 \times 0.20 = 0.001\)

\[
0.95 + 0.045 + 0.004 + 0.001 = 1
\]

**Interpretation note:** The transcript's result of approximately 154–155 ns is consistent when the 10% page-fault rate is treated as applying to TLB misses. If a question defines page-fault rate differently, use that definition when assigning probabilities.

## 5. Exam Method

For similar questions:

1. Draw the access paths: TLB hit, TLB miss with the page resident, page fault without dirty-page write-back, and page fault with dirty-page write-back.
2. Convert percentages to probabilities.
3. Calculate the time for each path, including every memory and disk operation specified.
4. Multiply each path's time by its probability.
5. Add the weighted times to obtain EMAT.
6. Check that the mutually exclusive path probabilities sum to 1.
