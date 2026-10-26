# Unix Inode Structure

## 1. What Is an Inode?

An **inode** is a file-system data structure used by Unix-like file systems to store information about a file and references to the file's data blocks.

The lecture describes the inode as a hybrid approach to block addressing. It combines:
- File metadata (attributes)
- Direct pointers to data blocks
- Single-indirect pointers
- Double-indirect pointers
- Triple-indirect pointers

> **Terminology note:** The lecture informally interprets “i-node” as “index node.” In Unix terminology, an inode is a data structure containing file metadata and block references; it is not simply an index block.

## 2. Metadata Stored in an Inode

Metadata is information about the file rather than the file's actual contents. The lecture lists examples such as:
- File size
- Permissions
- Last-modified information
- Other file-related attributes

The file name and path are usually maintained by directory entries and the directory hierarchy rather than being stored as ordinary inode fields. The inode is used to identify the file's metadata and locate its contents.

## 3. The Inode's Block-Pointer Structure

The lecture's model contains four kinds of block references.

### 3.1 Direct pointers

A **direct pointer** points straight to a data block containing part of the file.

```text
Inode ──direct pointer──> Data block
```

If an inode has 8 direct pointers, those pointers can refer to 8 data blocks.

### 3.2 Single-indirect pointer

A **single-indirect pointer** points to an indirect block that contains addresses of data blocks.

```text
Inode ──> Single-indirect block ──> Data blocks
```

The indirect block stores addresses, not the file's actual data. The lecture's example uses one disk block of 128 bytes and an address size of 8 bytes, so one indirect block can hold:

\[
\frac{128\text{ bytes}}{8\text{ bytes/address}} = 16\text{ addresses}
\]

Thus, a single-indirect pointer can address 16 data blocks in this example.

### 3.3 Double-indirect pointer

A **double-indirect pointer** points to a block of addresses of indirect blocks. Each of those indirect blocks then contains addresses of data blocks.

```text
Inode
  └──> Double-indirect block
         └──> Single-indirect blocks
                └──> Data blocks
```

If each index block can hold 16 addresses, the double-indirect pointer can reach:

\[
16 \times 16 = 16^2 = 256\text{ data blocks}
\]

### 3.4 Triple-indirect pointer

A **triple-indirect pointer** adds another level of indirection:

```text
Inode
  └──> Triple-indirect block
         └──> Double-indirect blocks
                └──> Single-indirect blocks
                       └──> Data blocks
```

With 16 addresses per index block, it can reach:

\[
16 \times 16 \times 16 = 16^3 = 4096\text{ data blocks}
\]

The key distinction is the number of index levels traversed before reaching the data block.

---

## 4. Worked Numerical Example: Maximum File Size

### Given

A file system uses the following inode structure:

- 8 direct pointers
- 1 single-indirect pointer
- 1 double-indirect pointer
- 1 triple-indirect pointer
- Disk block size = 128 bytes
- Size of each block address = 8 bytes

Find the maximum file size, using the lecture's assumptions.

### Step 1: Calculate addresses per indirect block

\[
N = \frac{\text{Block size}}{\text{Address size}}
= \frac{128}{8} = 16
\]

Each indirect block can store 16 block addresses.

### Step 2: Count data blocks reachable through each pointer type

| Pointer type | Number of data blocks reachable |
|---|---:|
| 8 direct pointers | \(8\) |
| Single indirect | \(16\) |
| Double indirect | \(16^2 = 256\) |
| Triple indirect | \(16^3 = 4096\) |

Total number of data blocks:

\[
8 + 16 + 16^2 + 16^3
= 8 + 16 + 256 + 4096
= 4376\text{ blocks}
\]

### Step 3: Convert the block count to bytes

\[
\text{Maximum file size} = 4376 \times 128
= 560{,}128\text{ bytes}
\]

Using the binary convention \(1\text{ KiB} = 1024\text{ bytes}\):

\[
\frac{560{,}128}{1024} = 547\text{ KiB}
\]

**Answer: 547 KiB** under the lecture's assumptions.

### A compact exam formula

Let:
- \(D\) = number of direct pointers
- \(B\) = disk block size in bytes
- \(P\) = size of one block address in bytes
- \(N = B/P\) = addresses per indirect block

For one single-, one double-, and one triple-indirect pointer:

\[
\text{Maximum data blocks} = D + N + N^2 + N^3
\]

\[
\text{Maximum file size} = (D + N + N^2 + N^3)\times B
\]

For this example, \(D=8\), \(B=128\), and \(P=8\), so \(N=16\), giving \(547\text{ KiB}\).

---

## 5. Important Exam Points

- **Direct pointer:** points directly to a data block.
- **Single indirect:** one index-block level before the data block.
- **Double indirect:** two index-block levels before the data block.
- **Triple indirect:** three index-block levels before the data block.
- Addresses per index block = block size ÷ address size.
- For \(N\) addresses per index block, a double-indirect pointer can address \(N^2\) data blocks and a triple-indirect pointer can address \(N^3\) data blocks.
- Multiply the total number of data blocks by the data-block size to obtain the maximum file size.

## 6. Scope and Assumptions

The numerical answer follows the exact structure and sizes stated in the lecture. It counts the blocks available for file data and does not add the storage occupied by the indirect index blocks themselves to the file's data size. Real Unix-like file systems differ in inode layout, pointer sizes, block sizes, and use of extents or other allocation schemes.
