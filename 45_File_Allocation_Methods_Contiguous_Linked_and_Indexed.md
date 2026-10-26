# File Allocation Methods in Operating Systems

## 1. Purpose of File Allocation

A file system stores files on secondary storage, such as a hard disk or SSD. Before storage, a file is treated as a sequence of **logical blocks**. The storage device exposes locations that the file system can allocate to those blocks.

A file allocation method defines **how a file's blocks are placed on storage and how the file system keeps track of them**.

The lecture introduces three methods:

1. **Contiguous allocation**
2. **Linked-list allocation**
3. **Indexed allocation**

### Goals of an allocation method

- **Efficient disk-space utilization:** use available storage while reducing waste and fragmentation.
- **Fast access:** make it efficient to retrieve file data, whether reading the file sequentially or accessing a particular part.

---

## 2. Contiguous Allocation

### Definition

In contiguous allocation, all blocks belonging to a file are stored in **consecutive storage locations**.

For example, if a file occupies four blocks and starts at block 10, its blocks are:

```text
10 → 11 → 12 → 13
```

The file can start at any suitable location; the requirement is that its blocks remain consecutive.

### Directory information

A directory entry can record:
- **File name**
- **Starting block**
- **Length** (number of blocks allocated)

For example:

| File | Starting block | Length |
|---|---:|---:|
| A | 0 | 3 |
| B | 6 | 5 |
| C | 14 | 4 |

File A occupies blocks 0–2, file B occupies blocks 6–10, and file C occupies blocks 14–17.

### Advantages

1. **Simple to implement:** the file system can locate a file using its starting block and length. It does not need a pointer in every data block or a separate index block.
2. **Good sequential access:** after reaching the beginning of the file's storage area, consecutive blocks can be read in order.
3. **Good direct access:** the address of a block can be calculated from the starting block and the block's offset within the file.

### Disadvantages

#### External fragmentation

External fragmentation occurs when free space is split into separate gaps. There may be enough free blocks in total, but no single contiguous run large enough for a new file.

Example:

```text
[Free][Free][Free][Used][Free][Used][Free][Free]
```

There are six free blocks in this example only if all free regions are counted; however, the largest contiguous region is three blocks. A file needing four consecutive blocks cannot be placed even though the total free space may be sufficient.

#### Difficult file growth

Suppose a file occupies blocks 0, 1, and 2, but block 3 is already occupied by another file. If the first file grows, the next consecutive blocks are not available. The file system may need to move the file to a larger contiguous region or use another strategy.

#### Internal fragmentation

If the system allocates storage in fixed-size blocks and the final block is not completely filled, some space within that allocated block may be unused. The lecture uses a 5 KB block holding 3 KB of data as an example, leaving 2 KB unused. This is internal fragmentation at the allocated-block level.

### Summary

| Property | Contiguous allocation |
|---|---|
| Placement | Consecutive blocks |
| Directory information | Starting block and length |
| Sequential access | Very good |
| Direct access | Very good |
| External fragmentation | Possible |
| File growth | Can be difficult |
| Pointer/index overhead | Low |

---

## 3. Linked-List Allocation

### Definition

Linked-list allocation stores a file's blocks in **non-consecutive locations**. Each block contains file data and a pointer to the next block. The last block contains an end-of-file marker or a null pointer.

Example placement:

```text
Directory entry: File A → start block 2

Block 2:  [Data | pointer to 7]
Block 7:  [Data | pointer to 9]
Block 9:  [Data | pointer to 11]
Block 11: [Data | pointer to 14]
Block 14: [Data | end-of-file]
```

The physical blocks need not be adjacent. The directory entry typically records the first block, and the pointers connect the rest of the file.

### How it works

1. The file is divided into logical blocks.
2. Each block is placed in an available storage location.
3. A pointer in each block records where the next block is located.
4. To read the whole file, the file system follows the pointers from the starting block until it reaches the end.

If the file grows, a new block can be allocated elsewhere and the previous last block's end marker can be changed to point to the new block.

### Advantages

1. **Avoids external fragmentation:** blocks can be placed wherever suitable free blocks are available.
2. **Flexible file growth:** a file can grow by linking another available block, without requiring adjacent free space.
3. **No need for one contiguous region:** the file can use separated free areas of the disk.

### Disadvantages

1. **More seek/head movement may be required:** blocks may be scattered across the disk, potentially increasing access time on magnetic disks.
2. **Poor direct/random access:** to reach a later block, the file system may need to follow the chain from the first block. It cannot generally calculate the location of an arbitrary block from only the starting address.
3. **Pointer overhead:** each block needs space for a pointer, leaving less room for file data in that block.
4. **Pointer reliability:** if a pointer is corrupted, the remaining chain may become difficult to access.

### Summary

| Property | Linked-list allocation |
|---|---|
| Placement | Non-consecutive blocks |
| Directory information | Usually starting block (and possibly other metadata) |
| Sequential access | Supported by following pointers |
| Direct access | Inefficient; chain may need to be traversed |
| External fragmentation | Avoided |
| File growth | Flexible |
| Pointer overhead | Present in each linked block |

---

## 4. Indexed Allocation

### Definition

Indexed allocation stores the addresses of a file's data blocks in a separate **index block**. The directory entry identifies the file's index block; the index block contains the locations of the file's data blocks.

Example:

```text
Directory entry: File A → index block 6

Index block 6:
  Block 1 of file A → disk block 9
  Block 2 of file A → disk block 1
  Block 3 of file A → disk block 14
  Block 4 of file A → disk block 4
```

The file's data blocks can be scattered across storage. The index block records their locations.

### How it works

1. Divide the file into logical blocks.
2. Allocate its data blocks wherever free space is available.
3. Allocate an index block for the file.
4. Store the addresses of the file's data blocks in the index block.
5. Record the index-block location in the directory entry.
6. To access a particular file block, consult the index and go to the corresponding data block.

**Each file has its own index information** in the model described by the lecture; the index is not one shared list for all files.

### Advantages

1. **Supports direct/random access:** the file system can look up the address of a requested block in the index instead of traversing all preceding data blocks.
2. **Avoids external fragmentation:** data blocks can be placed in any suitable free locations.
3. **Flexible file growth:** additional data blocks can be allocated without requiring them to be adjacent to existing blocks, provided the indexing structure can represent them.

### Disadvantages

1. **Index-block overhead:** storage must be reserved for block addresses rather than file content. Even a small file may require an index block.
2. **Large files may need more indexing:** if all block addresses do not fit in one index block, the file system may use multiple levels or another indexing arrangement. This adds complexity and storage overhead.
3. **Index access adds work:** the system may need to read the index information before accessing the requested data block.

### Unix inode note

The lecture connects indexed allocation with the Unix inode structure. An inode stores file metadata and references to file data; traditional inode designs can use direct block pointers and indirect blocks at one or more levels. Therefore, an inode is more than simply an index block, although it provides an indexing mechanism for locating file contents.

### Summary

| Property | Indexed allocation |
|---|---|
| Placement | Non-consecutive blocks |
| Directory information | Index-block location |
| Sequential access | Supported |
| Direct access | Supported through the index |
| External fragmentation | Avoided |
| File growth | Flexible, subject to index capacity/design |
| Index overhead | Present |

---

## 5. Comparison of the Three Methods

| Feature | Contiguous | Linked-list | Indexed |
|---|---|---|---|
| Block placement | Consecutive | May be scattered | May be scattered |
| How blocks are located | Start block + offset | Pointer chain | Index of block addresses |
| Directory entry typically stores | Start block and length | Start block | Index-block location |
| Sequential access | Excellent | Good, by following links | Good |
| Direct/random access | Excellent | Poor in the basic linked-list method | Supported |
| External fragmentation | Yes | No | No |
| File growth | Difficult if next space is occupied | Easy to extend by linking a block | Extend by adding an address to the index structure |
| Per-data-block pointer overhead | No | Yes | Not in the basic data blocks |
| Separate index-block overhead | No | No | Yes |

## 6. Worked Conceptual Example

Suppose a file has four logical blocks, and available disk locations are 2, 7, 9, and 14.

- **Contiguous allocation:** these blocks could not be used together for one file because they are not consecutive. The file system would need a suitable consecutive run elsewhere.
- **Linked-list allocation:** place the blocks at 2, 7, 9, and 14, and store pointers `2 → 7 → 9 → 14 → EOF`.
- **Indexed allocation:** place the blocks at 2, 7, 9, and 14, and store those addresses in the file's index block.

The key difference between linked-list and indexed allocation is how the file system discovers the next or requested data block: **follow a pointer from block to block** versus **look up the address in an index**.

## 7. Exam Revision Points

- **Contiguous allocation:** simple; strong sequential and direct access; suffers from external fragmentation and difficult file growth.
- **Linked-list allocation:** avoids external fragmentation and allows easy growth; has pointer overhead and weak direct access.
- **Indexed allocation:** avoids external fragmentation and supports direct access; requires index storage and may need multi-level indexing for large files.
- **External fragmentation:** free space exists, but it is divided into gaps that cannot satisfy a contiguous allocation request.
- **Internal fragmentation:** unused space remains inside an allocated unit.
- **Seek time:** on a magnetic disk, time needed for the read/write head to reach the required track. Scattered blocks may cause additional head movement.
