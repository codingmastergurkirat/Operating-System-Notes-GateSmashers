# Two-Level Paging

## 1. Motivation

A single-level page table has one entry for every virtual page. For a large address space, it may be much larger than one physical frame. Two-level paging divides the page table into page-sized inner tables and uses an outer table to locate them.

## 2. Worked Values

- Physical address space: 256 MB
- Logical address space: 4 GB
- Page/frame size: 4 KB
- Byte-addressable memory
- Simplified entries contain only a frame number

### Physical memory

```text
Physical bits = log2(256 MB) = 28
Offset bits = log2(4 KB) = 12
Frame-number bits = 28 - 12 = 16
Frames = 2^16 = 65,536
```

### Logical address space

```text
Logical bits = log2(4 GB) = 32
Page-number bits = 32 - 12 = 20
Pages = 2^20 = 1,048,576
```

## 3. Single-Level Table Size

A frame number needs 16 bits = 2 bytes. The single-level table would contain `2^20` entries:

```text
2^20 entries * 2 bytes = 2^21 bytes = 2 MiB
```

It cannot fit in one 4 KiB frame, motivating multiple levels.

## 4. Inner Page Tables

Each inner table occupies one 4 KiB page. With 2-byte entries:

```text
Entries per inner table = 4 KiB / 2 bytes = 2^11 = 2,048
Number of inner tables = 2^20 / 2^11 = 2^9 = 512
```

The outer table has 512 entries, one for each inner table. At 2 bytes per entry, its simplified size is 1 KiB.

## 5. Address Format

The 32-bit logical address is divided as:

```text
outer index: 9 bits | inner index: 11 bits | offset: 12 bits
```

The fields sum to 32 bits. The outer index chooses an inner page table; the inner index chooses the page entry; the offset selects the byte within the frame.

## 6. Translation Steps

1. Extract the 9-bit outer index.
2. Use it to find the relevant inner page table.
3. Extract the 11-bit inner index.
4. Read the physical frame number from the inner entry.
5. Preserve the 12-bit offset.
6. Combine the frame number and offset to form the physical address.

## 7. Results

| Quantity | Result |
|---|---:|
| Physical address bits | 28 |
| Frame offset | 12 bits |
| Frame-number bits | 16 |
| Physical frames | 65,536 |
| Logical address bits | 32 |
| Page offset | 12 bits |
| Page-number bits | 20 |
| Logical pages | 1,048,576 |
| Entries per inner table | 2,048 |
| Inner tables | 512 |
| Outer entries | 512 |
| Simplified outer size | 1 KiB |

Actual entries can include valid, protection, reference, caching, and dirty bits, changing table sizes.