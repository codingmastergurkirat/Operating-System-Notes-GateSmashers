# Inverted Page Tables and Paging Size Comparison

## 1. Conventional vs. Inverted Page Tables

In conventional paging, each process has a page table organized by that process's virtual page number. Each entry supplies a physical frame number.

An inverted page table is one global table organized by physical frames:

- One entry exists for every physical frame.
- The table index is the frame number.
- Each entry records at least the process ID and virtual page number occupying that frame.

The process ID is necessary because different processes can both have a page numbered 1.

| Feature | Conventional page table | Inverted page table |
|---|---|---|
| Organization | Virtual pages | Physical frames |
| Tables | Usually one per process | One system-wide table |
| Entry count | Virtual pages | Physical frames |
| Entry content | Frame number plus controls | PID and page number plus controls |
| Main benefit | Direct page-number indexing | Lower page-table memory overhead |
| Main drawback | Potentially large tables | Lookup requires matching PID and page number |

## 2. Lookup

For a request for page `p` of process `PID`:

1. Obtain `(PID, p)` and the page offset.
2. Search or hash the inverted table for a matching PID and page number.
3. The matching entry's index is the physical frame.
4. Combine that frame with the unchanged offset.

A basic lecture model describes a linear search; real systems can use hashing, associative hardware, or a TLB to accelerate lookup.

## 3. Example

With 1,024 physical frames, the inverted table has 1,024 entries. If the entry at index 5 contains `(P3, page 2)`, a request for page 2 of `P3` maps to frame 5.

## 4. Size-Ratio Example

Given:

- Virtual address size: 32 bits
- Page size: 4 KiB
- RAM size: 128 KiB
- Both entry types: 4 bytes

### Conventional table

```text
Virtual pages = 2^32 / 2^12 = 2^20
Normal size = 2^20 * 4 bytes = 2^22 bytes = 4 MiB
```

### Inverted table

```text
Physical frames = 2^17 / 2^12 = 2^5 = 32
Inverted size = 32 * 4 bytes = 128 bytes
```

### Ratio

```text
Normal : Inverted
= (2^20 * 4) : (2^5 * 4)
= 2^15 : 1
= 32,768 : 1
```

The equal entry sizes cancel. The ratio is driven by virtual-page count versus physical-frame count.

## 5. Exam Method

1. Number of virtual pages = virtual address space / page size.
2. Normal table size = virtual pages × normal-entry size.
3. Number of physical frames = RAM / frame size.
4. Inverted table size = physical frames × inverted-entry size.
5. Divide the sizes for the requested ratio.

## 6. Key Takeaways

- Normal paging maps a process page to a frame.
- Inverted paging maps a physical frame to the process/page currently occupying it.
- Inverted tables reduce memory overhead when virtual address spaces are much larger than physical memory.
- Lookup must identify both PID and page number.
- The simplified size comparison may omit control bits; real designs can have different entry sizes.