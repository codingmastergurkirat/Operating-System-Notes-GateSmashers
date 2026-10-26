# Segmentation in Operating Systems

## 1. What Is Segmentation?

**Segmentation** is a memory-management technique that divides a process into logical, variable-sized parts called **segments**. Examples include a main program, functions such as addition or subtraction, and the stack.

The segments are then placed in main memory. Unlike paging, segmentation reflects the logical structure of a program.

## 2. Paging vs. Segmentation

### Paging

- A process is divided into **fixed-size pages**.
- The division is performed without regard to the program's logical modules.
- A function may span multiple pages, and those pages may be stored in different physical frames.
- Pages are all the same size.

### Segmentation

- A process is divided into **logical segments**, such as `main`, `addition`, `subtraction`, or `stack`.
- Segment boundaries follow the logical organization of the program.
- Segments can have different sizes.
- Segments may be stored at different locations in physical memory.

| Feature | Paging | Segmentation |
|---|---|---|
| Unit of division | Page | Segment |
| Size | Fixed | Variable |
| Basis of division | Fixed-size blocks, not program modules | Logical parts of a program |
| Address information | Page number and page offset | Segment number and offset |
| Mapping structure | Page table | Segment table |

The lecture's example contrasts a function split across two pages with segmentation, where the function can be represented as a logical segment. A page fault can occur if a process references a page that is not currently in RAM; this is separate from the fact that a function may span pages.

## 3. Logical Addresses in Segmentation

The CPU generates a **logical address**. In segmentation, the logical address is represented by two components:

- **Segment number (`s`)**: identifies the segment being referenced.
- **Offset (`d`)**: identifies the position within that segment.

The lecture describes the segment number as being represented by the more significant bits and the offset by the remaining, less significant bits.

The Memory Management Unit (MMU) uses the segment table to translate the logical address into a physical address.

## 4. Segment Table

A segment table contains an entry for each segment. The two key fields described in the lecture are:

- **Base address**: the starting physical address of the segment.
- **Limit / size**: the length of the segment.

The segment number selects the relevant entry in the segment table.

### Example segment-table entries from the lecture

| Segment | Base address | Size / limit |
|---|---:|---:|
| S0 | 3300 | 200 bytes |
| S1 | 1800 | 400 bytes |
| S3 | 2300 | 400 bytes |

These values are examples used in the lecture.

## 5. Translating a Logical Address

To translate a logical address `(s, d)`:

1. Use `s` to select the segment-table entry.
2. Read that segment's base address and limit.
3. Check whether the offset is within the segment's bounds.
4. If it is valid, add the offset to the base address to obtain the physical address.
5. If the offset is outside the segment's bounds, raise a protection fault / trap rather than accessing memory beyond the segment.

In the conventional interpretation, the offset must satisfy:

\[
0 \le d < \text{limit}
\]

If the limit is defined as a segment length in bytes, a segment of length 400 bytes permits offsets 0 through 399. The lecture informally says the offset should be “less than or equal to” the size and illustrates adding the size to the base to reach the segment's end. For address translation, however, an offset equal to the length is one byte beyond the valid range.

### Example 1: Segment S1

Given:

- Base address = 1800
- Segment size = 400 bytes

If the offset is 200:

\[
\text{Physical address} = 1800 + 200 = 2000
\]

This is within the segment's valid range.

If the offset is 500, it exceeds the segment's 400-byte limit, so the reference is invalid and the system raises a trap.

The lecture also illustrates adding 400 to the base to calculate the segment's end address:

\[
1800 + 400 = 2200
\]

This is the address immediately after the 400-byte segment, not a valid byte within the segment.

### Example 2: Segment S3

Given:

- Base address = 2300
- Segment size = 400 bytes
- Offset = 200

Then:

\[
\text{Physical address} = 2300 + 200 = 2500
\]

The reference is valid because the offset is within the segment's limit.

## 6. Why Use Segmentation?

According to the lecture, segmentation has these benefits:

- It organizes a program into meaningful logical units.
- Related code or data can be grouped in the same segment.
- Different logical units can have different sizes.
- The segment table supports translation from a segment number and offset to a physical address.

## 7. Key Points for Exams

- **Paging:** fixed-size pages; the page size is the same for all pages.
- **Segmentation:** logical, variable-sized segments.
- A segmented logical address contains a **segment number** and an **offset**.
- A segment-table entry contains a **base address** and a **limit/size**.
- Physical address = base address + offset, provided the offset is within bounds.
- An out-of-range offset causes a trap / protection fault.
- Segments can be placed at different locations in physical memory.

## Quick Revision

| Term | Meaning |
|---|---|
| Segment | Variable-sized logical unit of a process |
| Segment number | Selects a segment-table entry |
| Offset | Position within the selected segment |
| Base address | Physical start address of the segment |
| Limit | Maximum valid extent of the segment |
| MMU | Hardware that translates logical addresses to physical addresses |
