# Page-Table Entry Fields and Functions

A page table maps process pages to physical frames. A page-table entry (PTE) contains the frame mapping and may contain status, protection, and replacement information.

## 1. Frame Number

The frame number is the essential mapping field. It identifies the physical frame containing the page. During translation, the page number indexes the table and the frame number replaces it in the physical address.

## 2. Valid/Invalid or Present Bit

This bit indicates whether the mapping is valid or the page is present according to the architecture and OS:

- `1`: valid/present mapping.
- `0`: invalid or absent mapping.

Referencing a valid virtual page that is not present may generate a page fault. An invalid entry can also mean that the virtual address is not part of the process's valid address space; it does not always simply mean “swapped out.”

## 3. Protection Bits

Protection bits control permitted operations, commonly:

- `R`: read
- `W`: write
- `X`: execute

They prevent operations such as writing to a read-only code page.

## 4. Reference/Accessed Bit

The reference bit records whether a page has been accessed under the architecture's rules. It helps page-replacement algorithms estimate usage. It can support approximations to LRU, but one bit alone does not implement exact LRU and the precise reset/access behavior varies by system.

## 5. Caching-Related Controls

A PTE may contain caching or memory-type controls that influence whether accesses to the page are cached. This is different from an application or web-content cache. Frequently reused data may benefit from caching, whereas some memory-mapped device or synchronization regions require special caching behavior.

## 6. Dirty/Modified Bit

The dirty bit indicates that a page has been changed in memory since it was loaded or last written back.

Example:

1. Storage contains `x=100`.
2. The page is loaded into RAM.
3. A process changes it to `x=200`.
4. The dirty bit becomes 1.
5. If the page is evicted, the OS writes it back; a clean page may not need write-back.

The dirty bit records the condition; it does not itself perform I/O.

## 7. Summary

| Field | Main purpose |
|---|---|
| Frame number | Identifies the physical frame. |
| Valid/present | Indicates whether the mapping is valid/present. |
| Protection | Controls read, write, and execute access. |
| Reference/accessed | Records access information for replacement decisions. |
| Caching control | Influences caching/memory type. |
| Dirty/modified | Indicates whether write-back may be required. |

If a numerical question asks for PTE size, include the frame-number bits and only the additional fields explicitly specified. Real PTEs usually contain more bits than the simplified frame-number-only calculation.