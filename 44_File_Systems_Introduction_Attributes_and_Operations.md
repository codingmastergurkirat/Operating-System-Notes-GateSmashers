# File Systems: Introduction, File Attributes, and File Operations

## 1. Introduction to File Systems

A **file system** is software within an operating system that manages files and the way data is stored and retrieved. It provides the organization that lets users work with files and folders without needing to know exactly where the underlying data resides on a disk.

The lecture gives examples of file systems associated with different operating-system families:
- Windows: NTFS
- Older DOS systems: FAT
- Unix: Unix file system
- Linux: extended file systems
- Large-storage examples mentioned in the lecture: ZFS and a “data-wide” file system (the latter name is unclear in the transcript)

These are examples from the lecture, not an exhaustive list of file systems used by those systems.

## 2. Is a File System Hardware or Software?

A file system is **software**. It is part of the operating system's storage-management functionality.

Its main responsibilities, as described in the lecture, are to:
- Organize user data into files and directories.
- Store file data on permanent storage.
- Keep track of where file data is located.
- Retrieve the correct data when a file is opened or read.
- Maintain file metadata (attributes) and support file operations.

## 3. Why File Systems Are Needed

Users work with data in file form, such as:
- Documents: DOC/DOCX, PDF
- Presentations and spreadsheets: PPT/PPTX, XLS/XLSX
- Audio and video: MP3, MP4
- Images: PNG, JPG/JPEG

Users also organize files into folders. In Unix-like systems, folders are commonly called **directories**.

A folder or directory groups files and may contain other directories. The file system maintains this organization and maps the user's view of files and folders to the underlying storage.

## 4. Files, Blocks, Sectors, and Mapping

### Physical storage overview

The lecture describes a magnetic disk using this hierarchy:

**Platters → Surfaces → Tracks → Sectors**

Data is stored in sectors. The file system must keep track of where each file's data is located on storage.

### Logical blocks

When a file is stored, the file system can treat its contents as a sequence of logical **blocks**. These blocks are mapped to storage locations.

- A file is divided logically into blocks.
- Each block is associated with a storage location.
- The locations need not be adjacent; existing data may occupy some areas of the disk.
- The file system maintains the information needed to locate the blocks again when the file is read.

The lecture uses an analogy of students being assigned to available rooms: students represent file blocks, rooms represent storage locations, and the mapping records which student goes to which room. The rooms may not be consecutive because some are already occupied.

**Important distinction:** Logical blocks are file-system units; disk sectors are physical or device-level storage units. The mapping between them is managed by the storage stack and depends on the file system and device.

## 5. File Attributes (Metadata)

A **file attribute** is metadata: information *about* a file, rather than the file's main content.

The lecture lists these attributes and related metadata:

| Attribute / metadata | Meaning |
|---|---|
| File name | The name used to refer to the file |
| File type / extension | Indicates a file type or format, such as PDF, JPG, or MP3 |
| Identifier | An internal identifier used by the operating system or file system to distinguish a file |
| Location / path | Where the file is found in the directory structure |
| Size | The amount of data associated with the file |
| Creation time/date | When the file was created, where supported |
| Modification time/date | When the file was last modified |
| Permissions / protection | Which users or groups can access the file and what actions they may perform |
| Other metadata | May include ownership, security information, compression, or encryption-related attributes, depending on the system |

### Notes on attributes

- File extensions are commonly used to suggest a file's format, but the extension alone does not always determine the actual content or the application that can open it.
- Paths are particularly important in directory-based file systems because they identify a file's place in the hierarchy.
- Permissions may distinguish operations such as reading, writing, and executing.
- Metadata occupies storage space, although the exact amount depends on the file system and its implementation.

## 6. Common File Operations

The lecture discusses the following operations.

### Create
Creates a new file entry and establishes the information needed to manage the file. The user may specify a name and location.

### Read
Retrieves file contents without changing them through that read operation.

### Write
Stores new data or modifies existing file contents.

### Delete
Removes the file from the file system's namespace and releases or marks its storage for reuse according to the file system's behavior. The lecture describes deletion as removing both the content and its attributes from the user's view.

### Truncate
Removes the file's contents or reduces its length, often to zero, while keeping the file itself and its identity/metadata. For example, a text file may remain present with the same name but contain no data after truncation.

**Delete vs. truncate**
- **Delete:** the file entry is removed from the directory structure.
- **Truncate:** the file remains, but its contents are cleared or shortened.

The exact treatment of data and metadata depends on the file system; deleting a file does not necessarily mean every underlying byte is immediately erased.

### Reposition / seek
Changes the current position used for the next read or write. For example, a program may skip the beginning of a file and move to a later byte position before reading. This is often called a **seek** operation.

## 7. Command and Interface Examples Mentioned in the Lecture

The lecture notes that operations may be performed through different interfaces:
- Windows commonly provides graphical file-management interfaces.
- Linux and Unix-like systems commonly support command-line tools as well as graphical interfaces.
- `mkdir` is a common command for creating a directory.
- `touch` can create an empty file or update timestamps, depending on usage.
- `cat` can display or concatenate file contents; it is not a general replacement for all file-creation workflows.
- File reading and writing can also be performed through operating-system system calls or APIs.
- The transcript mentions `lseek`-style repositioning; in Unix-like systems, `lseek()` changes a file descriptor's offset.

The transcript contains several unclear command spellings, so only recognizable commands are included here.

## 8. File-System Role: Summary

The user interacts with files and directories. Beneath that view, the file system:
1. Organizes files and directories.
2. Stores and maintains file metadata.
3. Maps logical file blocks to storage locations.
4. Tracks the locations needed to retrieve file contents.
5. Provides operations such as create, read, write, delete, truncate, and seek.

## 9. Quick Exam Revision

- **File system:** operating-system software for organizing, storing, and retrieving files.
- **Folder / directory:** a container for files and possibly other directories.
- **Metadata / attributes:** information about a file, such as its name, type, identifier, path, size, timestamps, and permissions.
- **Logical blocks:** units used to organize file content; the file system maps them to storage.
- **Create, read, write:** basic operations for making, retrieving, and modifying file content.
- **Delete:** removes the file entry.
- **Truncate:** removes or shortens contents while retaining the file.
- **Seek / reposition:** changes the current file position for subsequent I/O.
