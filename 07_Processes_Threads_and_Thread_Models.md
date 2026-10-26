# Processes and Threads

## Process vs. Thread

A **process** is a running program with its own process identity, address space, code, data, stack, registers, and operating-system-managed information such as a process ID and process control block (PCB). In Unix-like systems, `fork()` creates a child process with its own process identity and execution context. Modern systems commonly use copy-on-write, so a complete immediate physical-memory copy is an oversimplification.

A **thread** is an execution path within a process. Threads in the same process generally share code, data, and address space, but each thread has its own stack and register state. Threads are often called lightweight because creating and switching between them can require less overhead than separate processes.

| Feature | Processes | Threads in one process |
|---|---|---|
| Address space | Separate | Shared |
| Code and data | Separate process address spaces; pages may be shared by the OS | Shared |
| Stack and registers | Each process has its own execution state | Each thread has its own |
| Identity | Process ID | Thread identity within a process |
| Creation | `fork()` in Unix-like systems | Threading API or library |
| Resource sharing | Requires explicit IPC mechanisms | Shared memory directly |
| Context switching | Often more state and overhead | Often cheaper, depending on implementation |

## Blocking and Context Switching

Context switching saves one execution context and restores another. A process may block while waiting for I/O, allowing another process to run. With user-level threads, a blocking system call can block the underlying process or kernel execution context if the runtime cannot handle it asynchronously. With kernel-level threads, the kernel schedules threads individually, so one blocked thread generally does not prevent other runnable threads in the same process from running.

## User-Level Threads

User-level threads are managed by a user-space library or runtime. Management and switching can be fast, and the kernel may not know each thread individually. However, blocking operations may block all user threads sharing one underlying execution context.

## Kernel-Level Threads

Kernel-level threads are managed and scheduled by the operating-system kernel. The kernel knows and schedules individual threads. Creation and switching may require more kernel support, but one blocked thread normally does not stop other runnable threads.

POSIX threads (`pthreads`) are an API; the API name alone does not imply a purely user-level implementation. Modern systems commonly map POSIX threads to kernel threads.

## Thread Mapping Models

- **Many-to-one:** Many user threads map to one kernel thread. Blocking can stop all of them, and parallel execution is unavailable through that one kernel thread.
- **One-to-one:** Each user thread maps to a kernel thread. This supports individual kernel scheduling with greater resource use.
- **Many-to-many:** Many user threads are multiplexed over multiple kernel threads.

### Quick Revision

- A process has an independent process identity and address space.
- Threads in one process share code, data, and address space.
- Each thread has its own stack and registers.
- User-level threads are managed by a runtime; kernel-level threads are managed by the kernel.
- `fork()` creates a child process.
- Many-to-one, one-to-one, and many-to-many describe user-to-kernel thread mappings.
