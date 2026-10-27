# Operating System — Introduction and Functionalities

## 1. Operating System

An **Operating System (OS)** is **system software** that acts as an interface between the user and computer hardware.

The user generally accesses these devices through the operating system rather than controlling the hardware directly.

## 2. Why Do We Need an Operating System?

Without an operating system, users or applications would need to write separate programs to control different hardware devices. This would make using a computer more complicated.

The operating system:
- Provides a convenient way to interact with hardware.
- Coordinates the use of devices when multiple processes or users need them.

## 3. Major Functionalities of an Operating System

### 3.1 Resource Management

The operating system manages system resources, such as the CPU, memory, and input/output devices.

- It allocates resources to processes or users when needed.
- It can reclaim resources when they are no longer needed.
- Resource management is particularly important when multiple users or processes need the same resources.

### 3.2 Process Management

A **process** is a program that is being executed.

For example, running a word processor, media player, and game at the same time involves multiple processes.

The operating system manages the execution of these processes.

#### CPU Scheduling

- The CPU executes processes.
- When multiple processes need CPU time, the operating system uses **CPU scheduling** to decide which process gets the CPU and when.
- CPU scheduling algorithms help organize CPU use efficiently.

### 3.3 Storage Management

Storage management deals with secondary storage, such as a hard disk, where data is kept for long-term or permanent storage.

The operating system manages stored data through a **file system**.

Its responsibilities include:
- Organizing files and directories.
- Managing how data is stored and retrieved.

### 3.4 Memory Management

Memory management deals mainly with **RAM**, which is limited in size and is used by running programs and processes.

The operating system:
- Allocates memory to processes.
- Keeps track of which memory areas are in use.
- Reclaims memory when a process no longer needs it.
- Helps manage multiple processes that need memory at the same time.

#### Allocation and Deallocation

- **Allocation:** Assigning memory to a process.
- **Deallocation:** Releasing memory when it is no longer required.

#### Swapping

The lecture introduces **swapping** as moving processes or their memory contents between main memory and secondary storage to help manage limited RAM. The exact implementation depends on the operating system.

### 3.5 Security and Protection

The operating system provides mechanisms to protect the system and its data.

Examples discussed in the lecture:
- **Authentication:** A password can be used to verify a user's identity before granting access.
- **Process isolation:** Processes should not be allowed to access or modify memory belonging to other processes without authorization.
- **Access control:** The system restricts access to resources according to permissions.

## 4. How Users Access Operating System Services

Users generally interact with the operating system through applications or a command-line interface.

### 4.1 Through Applications

For example, when a user prints a document from a word processor:

1. The user chooses the print command in the application.
2. The application requests the relevant operating system service.
3. The operating system coordinates access to the printer.

The operating system acts as an interface between the application/user and the hardware.

### 4.2 Through a Shell or Command-Line Interface

A **shell** allows users to interact with operating system services by entering commands.

Examples:
- Command Prompt in Windows
- Terminal in Linux

### 4.3 System Calls

A **system call** is a mechanism through which a program requests a service from the operating system.

Examples :
- `open` — request to open a file.
- `read` — request to read data.
- `write` — request to write data.

For example, when an application opens a file, it requests the operating system to perform the required operation. System calls provide the interface through which applications request such services.

**Refrences:**

**For more understanding:** [Watch the explanation on YouTube](https://www.youtube.com/watch?v=WJ-UaAaumNA&list=PLxCzCOWd7aiGz9donHRrE9I3Mwn6XdP8p&index=2&t=753s).

