# Translation Lookaside Buffer (TLB): Effective Access Time Numerical

## 1. Question

A paging system uses a Translation Lookaside Buffer (TLB) with:

- TLB access time = 10 ns
- Main-memory access time = 50 ns
- TLB hit ratio = 90%
- Page faults = none

Find the **effective memory access time (EAT)**.

## 2. How Address Translation Works with a TLB

The CPU generates a logical address containing a page number and page offset. The system searches the TLB for the page's frame number.

### TLB hit

If the mapping is in the TLB:

1. Search the TLB.
2. Obtain the frame number.
3. Access the requested data in main memory.

\[
T_{\text{hit}} = T_{\text{TLB}} + T_{\text{memory}}
\]

Using the given values:

\[
T_{\text{hit}} = 10 + 50 = 60\text{ ns}
\]

### TLB miss (with no page fault)

If the mapping is not in the TLB:

1. Search the TLB.
2. Access the page table in main memory to find the frame number.
3. Access the requested data in main memory.

Assuming a single-level page table and that the page-table lookup takes one main-memory access:

\[
T_{\text{miss}} = T_{\text{TLB}} + T_{\text{memory}} + T_{\text{memory}}
\]

\[
T_{\text{miss}} = 10 + 50 + 50 = 110\text{ ns}
\]

The TLB lookup time is included for both hits and misses because the TLB is checked first.

## 3. Calculate the Hit and Miss Ratios

Hit ratio:

\[
h = 90\% = 0.9
\]

Miss ratio:

\[
1-h = 1-0.9=0.1
\]

Therefore:

- Probability of a TLB hit = 0.9
- Probability of a TLB miss = 0.1

## 4. Effective Memory Access Time

With no page faults, use the weighted average of hit and miss times:

\[
EAT = h(T_{\text{hit}})+(1-h)(T_{\text{miss}})
\]

Substitute the values:

\[
EAT = 0.9(60)+0.1(110)
\]

\[
EAT = 54+11
\]

\[
\boxed{EAT = 65\text{ ns}}
\]

## Final Answer

**Effective memory access time = 65 ns.**

## 5. Important Exam Notes

- **TLB hit time:** TLB access + one main-memory access.
- **TLB miss time:** TLB access + page-table access + data access.
- The formula above assumes a single-level page table and no page fault.
- If a multi-level page table is used, a TLB miss may require additional memory accesses for the page-table levels.
- If a page fault occurs, the page-fault service time must also be accounted for; it is not included in this question.
- A higher TLB hit ratio generally reduces effective memory access time.

### Quick formula

Let \(h\) be the TLB hit ratio, \(t\) the TLB access time, and \(m\) the main-memory access time. Under the assumptions in this question:

\[
\boxed{EAT = h(t+m)+(1-h)(t+2m)}
\]

For \(h=0.9\), \(t=10\text{ ns}\), and \(m=50\text{ ns}\):

\[
EAT=0.9(60)+0.1(110)=65\text{ ns}
\]
