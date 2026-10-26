# Paging Fundamentals, Address Translation, and Paging Calculations

## 1. Non-Contiguous Allocation and Paging

Non-contiguous allocation allows different parts of a process to occupy separate physical-memory locations. For example, a 6 KB process can use three separate 2 KB holes, whereas contiguous allocation would require one 6 KB hole.

Paging makes this manageable by dividing:

- A process into fixed-size **pages**.
- Physical memory into equal-size **frames**.

Page size equals frame size, so any page fits exactly into any free frame. Pages of one process need not be adjacent. Paging avoids external fragmentation caused by requiring one large contiguous region, although page-level internal waste can occur when a process does not fill its last page.

## 2. Logical-to-Physical Translation

A logical address is divided into:

```text
(page number, page offset)
```

The page table maps the page number to a frame number. The MMU replaces the page number with that frame number and preserves the offset:

```text
physical address = (frame number, same offset)
```

For byte-addressed memory:

```text
page number = floor(logical address / page size)
offset      = logical address mod page size
physical    = frame number * frame size + offset
```

### Small example

A 4-byte process has 2-byte pages. Page 0 is in frame 2 and page 1 is in frame 4. For logical address 3:

```text
page = floor(3/2) = 1
offset = 3 mod 2 = 1
physical = 4*2 + 1 = 9
```

Thus logical address 3 maps to physical address 9. Logical address 1 maps to page 0, offset 1, and therefore physical address `2*2+1=5`.

## 3. Address-Space Calculations

### Example A: 4 GB logical, 64 MB physical, 4 KB pages

Assume byte-addressable memory and binary units.

```text
Logical address bits = log2(4 GB) = 32
Physical address bits = log2(64 MB) = 26
Page/frame offset bits = log2(4 KB) = 12
```

Therefore:

```text
Pages = 2^(32-12) = 2^20 = 1,048,576
Frames = 2^(26-12) = 2^14 = 16,384
```

There is one basic page-table entry per page. A frame number needs 14 bits, so if entries contain only frame numbers:

```text
Page table size = 2^20 * 14 bits
                = 14,680,064 bits
                = 1,835,008 bytes
                = 1,792 KiB (about 1.75 MiB)
```

Real entries can be larger because of valid, protection, reference, caching, and dirty bits.

### Example B: 7-bit logical address, 6-bit physical address, page size 8 words

Assuming a word is the addressable unit:

```text
Offset bits = log2(8) = 3
Page-number bits = 7 - 3 = 4
Pages = 2^4 = 16

Frame-number bits = 6 - 3 = 3
Frames = 2^3 = 8
```

The page table has 16 entries. If an entry stores only a frame number, it needs 3 bits.

## 4. Page Table and MMU Workflow

1. CPU generates a logical address.
2. MMU extracts the page number and offset.
3. MMU reads the page-table entry.
4. The entry supplies the physical frame number.
5. The MMU preserves the offset.
6. Hardware accesses the resulting physical address.

Each process normally has its own page table in the basic paging model.

## 5. Benefits and Costs

**Benefits:** pages can use any available frames, contiguous physical memory is unnecessary, and external fragmentation from large contiguous requests is avoided.

**Costs:** page tables consume memory, address translation adds work, and a process can suffer a page fault if a referenced page is not currently present.

## 6. Calculation Method

1. Convert logical address-space size to a power of two.
2. Convert physical address-space size to a power of two.
3. Convert page size to a power of two.
4. Offset bits are the page-size exponent.
5. Page-number bits = logical-address bits minus offset bits.
6. Frame-number bits = physical-address bits minus offset bits.
7. Pages and frames are the corresponding powers of two.
8. Basic page-table entries equal the number of pages.
9. Entry size depends on the frame-number bits plus any specified control fields.