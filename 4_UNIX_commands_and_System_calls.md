# UNIX Commands and System Calls

These notes cover file permissions using `chmod` and file-position changes using the `lseek()` system call.

---

# Changing File Permissions with `chmod`

## File Permission Categories

The `chmod` command is used to change the permissions of a file or directory.

Permissions are assigned to three categories:

- **User (`u`):** The file's owner.
- **Group (`g`):** Users who belong to the file's group.
- **Others (`o`):** All other users.
- **All (`a`):** User, group, and others together.

There are three basic permissions:

- **Read (`r`):** Permission to read a file.
- **Write (`w`):** Permission to modify a file.
- **Execute (`x`):** Permission to execute a file, where applicable.

The permissions are commonly written in this order: **read, write, execute (`rwx`)**.

## Symbolic Notation

Symbolic notation uses category letters and permission symbols to add or remove permissions.

- `+` adds a permission.
- `-` removes a permission.
- `=` sets the permissions to exactly those specified.

### Examples

```bash
chmod ugo+r note
```

Adds read permission for the user, group, and others.

```bash
chmod a+r note
```

Also adds read permission for all three categories.

```bash
chmod go+r note
```

Adds read permission for the group and others, but not explicitly for the owner.

```bash
chmod ugo+rw note
```

Adds read and write permissions for the user, group, and others.

```bash
chmod o-x note
```

Removes execute permission from others.

To set the permissions for all three categories to read-only, use:

```bash
chmod a=r note
```

or:

```bash
chmod 444 note
```

These set the file's permissions to read-only for the user, group, and others. The `=` form and the octal form set the specified permissions rather than merely adding them.

## Octal Permission Notation

Permissions can also be represented by numbers:

| Permission | Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |

Add the values to represent a combination of permissions.

| Number | Calculation | Permissions |
|---:|---|---|
| `0` | `0` | No permissions |
| `1` | `1` | Execute |
| `2` | `2` | Write |
| `3` | `2 + 1` | Write and execute |
| `4` | `4` | Read |
| `5` | `4 + 1` | Read and execute |
| `6` | `4 + 2` | Read and write |
| `7` | `4 + 2 + 1` | Read, write, and execute |

Three octal digits represent permissions for **user, group, and others**, in that order.

### Examples

```bash
chmod 666 note
```

Gives read and write permissions to the user, group, and others.

```bash
chmod 555 note
```

Gives read and execute permissions to all three categories.

```bash
chmod 333 note
```

Gives write and execute permissions to all three categories.

```bash
chmod 444 note
```

Gives read-only permission to all three categories.

### Quick Exam Notes

- To add read permission for user, group, and others: `chmod ugo+r note`.
- To set read-only permission for all three categories: `chmod 444 note` or `chmod a=r note`.
- To give read and write permissions to all three categories: `chmod 666 note`.
- The three octal digits always represent **user, group, others**, in that order.

---

# The `lseek()` System Call

## Purpose

`lseek()` is a **system call**, not generally a shell command. It changes the current file offset associated with an open file descriptor, allowing a program to access data at different positions in a file.

The file offset is commonly measured in bytes. The exact result depends on the current offset, the requested offset, and the reference point.

## Syntax

```c
lseek(file_descriptor, offset, reference_point);
```

- **`file_descriptor`:** Identifies the open file.
- **`offset`:** The number of bytes to move, or the position to set, depending on the reference point.
- **`reference_point`:** Specifies where the offset calculation starts.

## Reference Points

| Constant | Meaning |
|---|---|
| `SEEK_SET` | Measures the offset from the beginning of the file. |
| `SEEK_CUR` | Measures the offset relative to the current file offset. |
| `SEEK_END` | Measures the offset relative to the end of the file. |

A positive offset moves forward relative to the chosen reference point. A negative offset moves backward where the operation and file type permit it.

## Example of Multiple `lseek()` Calls

Suppose a file is opened and its file descriptor is stored in `n`.

```c
lseek(n, 10, SEEK_CUR);
lseek(n, 5, SEEK_SET);
```

The first call moves the file offset 10 bytes forward from its current position.

The second call sets the file offset to 5 bytes from the beginning of the file, regardless of the position established by the first call.

Therefore, after both calls, the file offset is **5 bytes from the beginning of the file**.

The calls are order-dependent. If the order were reversed:

```c
lseek(n, 5, SEEK_SET);
lseek(n, 10, SEEK_CUR);
```

the first call would set the offset to 5 bytes from the beginning, and the second would move it 10 bytes forward from there, resulting in an offset of 15 bytes from the beginning.

## Using `SEEK_END`

`SEEK_END` measures the offset relative to the end of the file.

For example:

```c
lseek(n, -5, SEEK_END);
```

This attempts to position the file offset 5 bytes before the end of the file.

## Important Exam Notes

- `lseek()` is a system call used to change the file offset of an open file.
- `SEEK_SET` uses the beginning of the file as the reference point.
- `SEEK_CUR` uses the current file offset as the reference point.
- `SEEK_END` uses the end of the file as the reference point.
- Calls are evaluated in order; each call affects the position used by a later `SEEK_CUR` call.
- The offset is generally measured in bytes, not character positions or numbered locations.

---

# Quick Revision

| Topic | Key Point |
|---|---|
| `chmod` | Changes file or directory permissions. |
| `u`, `g`, `o` | User, group, and others. |
| `r`, `w`, `x` | Read, write, and execute. |
| `+` | Adds permissions. |
| `-` | Removes permissions. |
| `=` | Sets permissions to the specified permissions. |
| Octal permissions | Read = 4, write = 2, execute = 1. |
| `chmod 444 note` | Sets read-only permissions for user, group, and others. |
| `chmod 666 note` | Sets read and write permissions for all three categories. |
| `lseek()` | Changes the file offset for an open file descriptor. |
| `SEEK_SET` | Offset from the beginning. |
| `SEEK_CUR` | Offset from the current position. |
| `SEEK_END` | Offset from the end. |
