# System Calls and the `fork()` System Call

## System Calls

### What Is a System Call?

A **system call** is a programmatic mechanism that allows a user-mode program to request a service from the operating system kernel. It provides a controlled way to access functionality that requires kernel privileges.

Programs and applications generally run in **user mode**. Operations such as accessing hardware, working with files, or requesting certain operating-system services require kernel involvement. A system call transfers control to the kernel to carry out the requested operation.

### Why Are System Calls Needed?

Examples:

- **Printing output:** A C program can calculate `2 + 2` in user mode, but displaying the result on a monitor requires an operating-system service.
- **Accessing files:** Reading or modifying a file on secondary storage requires the operating system to manage access.
- **Accessing devices:** Operations involving a printer, disk, monitor, or other device are managed through the operating system.

The kernel manages access to system resources, so programs use system calls rather than directly performing privileged operations.

### System Calls and APIs or Library Functions

Some operating systems let programmers use system calls directly. In Unix-like systems, examples include `read()`, `write()`, `open()`, `close()`, and `fork()`.

Programs may also use APIs or library functions instead. For example, C's `printf()` is a library function, not itself a system call. It may use lower-level operating-system services to produce output. The exact path depends on the library and runtime environment.

System calls and their interfaces vary by operating system. The lecture mentions that operating systems can provide hundreds of system calls.

### Main Categories of System Calls

#### File Management

Used to create, open, read, write, close, or otherwise manage files.

Examples include:

- `open()`
- `read()`
- `write()`
- `close()`

#### Device Management

Used to access or control hardware devices and their attributes. Examples include reading from or writing to devices, changing device positions where supported, and controlling input/output operations.

The lecture mentions `ioctl()` as an interface used for device-specific control operations.

#### Information Maintenance

Used to obtain information about processes, files, devices, or the system.

Examples:

- `getpid()` — obtains the calling process's ID.
- `getppid()` — obtains the parent process's ID.

Metadata is information about an item rather than its main content. For a file, metadata can include its name, size, and permissions. System-information requests may also include date and time information.

#### Process Control

Used to create, execute, or terminate processes and to manage aspects of their execution.

Examples and related concepts include:

- `fork()` — creates a child process.
- `wait()` — allows a process to wait for a child process or a related event.
- Signals — used to notify a process that an event has occurred.
- Memory-management operations — request or manage memory through operating-system interfaces.

#### Communication

Used for **inter-process communication (IPC)**, which allows processes to exchange information.

Examples mentioned in the lecture include:

- `pipe()` — creates a pipe for communication between processes.
- `shmget()` — obtains a System V shared-memory segment identifier.

Operating systems also provide interfaces for establishing and managing communication connections.

#### Protection and Security

Used to control access permissions and related security settings.

Examples:

- `chmod` — changes file permissions.
- `umask` — sets the permission mask used when creating files and directories.

### Key Point

The purpose of learning these categories is to understand **why system calls are needed and what kinds of operating-system services they provide**, rather than memorizing every system call.

---

## The `fork()` System Call

### What Is `fork()`?

In Unix-like systems, `fork()` creates a new process called the **child process**. The process that calls `fork()` is commonly called the **parent process**.

The child is a separate process with its own process ID. Although the child initially has a process image based on the parent, details such as memory handling depend on the operating system; modern systems commonly use copy-on-write. A thread is different: threads within one process share the process's address space and resources.

### Return Values of `fork()`

A successful `fork()` returns different values in the parent and child:

- **`0` in the child process**
- **A positive value in the parent process** — specifically, the child's process ID
- **`-1` on failure** — no child process is created, and an error is indicated

For typical process-counting exercises, questions often assume that every `fork()` succeeds. Use the `-1` case only if failure is part of the question.

### One `fork()` Call

Example:

```c
fork();
printf("Hello\n");
```

Assuming `fork()` succeeds, both the parent and child continue from the point after the call. Each reaches the `printf()` statement, so `Hello` is printed twice in total, subject to normal output-buffering considerations.

### Multiple `fork()` Calls

For a simple program where each existing process reaches each `fork()` call, and every call succeeds:

- After one `fork()`: 2 total processes
- After two sequential `fork()` calls: 4 total processes
- After three sequential `fork()` calls: 8 total processes

If there are `n` unconditional `fork()` calls, the total number of processes is:

`2^n`

The number of newly created child processes, excluding the original process, is:

`2^n - 1`

These formulas do **not** automatically apply to every program. Conditional statements, short-circuit operators, loops, early exits, or failed calls can change how many times `fork()` executes.

---

## Solving a `fork()` Question with `if` and `&`

Consider this pattern from the lecture:

```c
if (fork() & fork()) {
    printf("Hello\n");
}
```

This is more complicated than two unconditional `fork()` calls because the second call is part of the `if` condition.

### How to Reason About It

1. The first `fork()` creates a child. In the child, the first call returns `0`; in the parent, it returns a positive child PID.
2. The bitwise `&` operator evaluates both operands. Unlike the logical `&&` operator, it does not short-circuit.
3. Therefore, both the original process and the child created by the first call proceed to evaluate the second `fork()`.
4. Each of those processes is split again by the second `fork()`, creating four processes in total.
5. The `if` body runs only in processes where the bitwise result of the two return values is nonzero.

### Count the `printf()` Calls Carefully

The lecture's explanation reaches four executions of `printf()`, but that conclusion is not guaranteed by the code as written. The parent-side return values are child PIDs, and the bitwise result depends on the actual PID values. A positive PID is not necessarily equivalent to the integer `1` for bitwise operations.

For a reliable exam solution, track the actual return values and the condition in each process. If a question simplifies parent return values to `1`, state that assumption explicitly before evaluating the bitwise expression.

### `&` Versus `||`

- `&` is **bitwise AND** and evaluates both operands.
- `||` is **logical OR** and short-circuits: if the first operand is true, the second operand is not evaluated.
- `&&` is **logical AND** and short-circuits: if the first operand is false, the second operand is not evaluated.

These operators are not interchangeable. Replacing one with another can change both the condition's result and the number of child processes created.

---

## Quick Revision

| Topic | Key point |
|---|---|
| System call | Controlled request from a user-mode program to the kernel |
| File-management calls | Manage file creation, access, reading, writing, and closing |
| Device-management calls | Access or control devices and their attributes |
| Information calls | Retrieve process, file, device, or system information |
| Process-control calls | Create, manage, or terminate processes |
| IPC calls | Allow processes to exchange data |
| `fork()` returns `0` | In the child process |
| `fork()` returns a positive value | In the parent; value is the child's PID |
| `fork()` returns `-1` | The call failed and no child was created |
| `n` unconditional successful forks | `2^n` total processes; `2^n - 1` newly created children |
| Bitwise `&` | Evaluates both operands; not the same as logical `&&` |
| `printf()` | C library function, not itself a system call |