# User Mode vs. Kernel Mode

## User Mode

**User mode** is the mode in which ordinary applications and user programs generally execute. Examples include text editors, media players, and office applications.

When a user writes or runs a C program, the program normally executes in user mode. The program can perform ordinary computations, such as calculating `2 + 2`, without requiring the kernel to perform that calculation.

However, user-mode programs cannot directly perform privileged operations or freely control hardware. They request operating-system services when needed.

## Kernel Mode

**Kernel mode** is the privileged execution mode used by the operating-system kernel. Core operating-system functionality and much of the low-level hardware management operate with kernel privileges.

The kernel manages access to system resources, including files and hardware devices. This helps prevent applications from directly interfering with hardware or with one another.

## Why Does the Processor Switch Between Modes?

A program may need an operating-system service that it cannot perform directly in user mode. In that case, it uses a **system call** to request the service from the kernel.

For example, a program may need to read data from a file stored on a hard disk. The program requests the operation through a system call such as `read()`. Execution transfers to the kernel, which performs or coordinates the requested operation. After the system-call handling finishes, execution returns to user mode and the program continues.

### Example: Reading a File

A simplified sequence is:

1. A C program is executing in user mode.
2. The program requests data from a file using a system call such as `read()`.
3. A controlled transition transfers execution to the kernel.
4. The kernel handles the request and manages the required file or device access.
5. The system call returns, and the program resumes in user mode.

The exact implementation can involve additional operating-system and hardware steps, but the key idea is that privileged work is handled by the kernel.

## Mode Bit

The lecture describes a mode bit used to distinguish the two modes:

- **Mode bit `1`** — user mode
- **Mode bit `0`** — kernel mode

In this simplified model, the mode changes from `1` to `0` when execution enters the kernel and from `0` to `1` when execution returns to user mode.

A system call commonly causes a controlled trap or similar transition into the kernel. The kernel handles the request and returns control to the calling program.

> The mode-bit values above follow the convention used in the lecture. Actual processor architectures can implement privilege levels differently.

## Example: Printing Output

Suppose a C program calculates:

```c
int result = 2 + 2;
```

The arithmetic can normally be performed in user mode. If the program then displays the result using:

```c
printf("%d\n", result);
```

`printf()` is a C library function, not itself a system call. The library may eventually request an operating-system service to send output to a terminal or another destination. That request can require a transition into kernel mode.

The kernel and relevant device or driver components manage the underlying output operation.

## The Dual-Mode Concept

The separation between user mode and kernel mode is often called **dual-mode operation**.

- Applications generally run in user mode.
- The operating-system kernel performs privileged operations in kernel mode.
- System calls provide a controlled way for programs to request kernel services.
- The processor transitions between modes when necessary.

This separation is important for protection and resource management. Without it, ordinary applications could have too much direct control over hardware and shared system resources.

## Analogy: Using a Bank

The lecture compares the two modes to a bank:

- The **customer** represents a user-mode program.
- The **bank staff and secured operations** represent the kernel and its privileged services.
- The **request form and service window** represent the controlled request mechanism, such as a system call.

A customer cannot simply walk behind the counter and take money from a drawer. Instead, the customer makes a request and authorized staff carry it out. Similarly, a user-mode program requests privileged services from the kernel rather than directly controlling protected resources.

## Quick Revision

| Topic | Key point |
|---|---|
| User mode | Normal applications and user programs generally execute here |
| Kernel mode | Privileged operating-system code executes here |
| System call | A controlled request for a service from the kernel |
| Mode bit in the lecture's model | `1` for user mode; `0` for kernel mode |
| Trap / controlled transition | Transfers execution to kernel-mode handling |
| Return from system call | Control returns to the user program |
| Dual-mode operation | Separates ordinary application execution from privileged operations |
| Main purpose | Protection and controlled management of hardware and system resources |