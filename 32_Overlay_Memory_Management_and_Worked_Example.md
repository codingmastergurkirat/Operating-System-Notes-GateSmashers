# Overlay Memory Management

## 1. What Is an Overlay?

**Overlay** is a memory-management technique that allows a program larger than the available main memory to run by loading only the parts needed at a particular time.

Instead of keeping the entire program in RAM simultaneously, the program is divided into modules or overlays. The required module is loaded into memory, executed, and then replaced by another module when needed.

### Example

Suppose a system has fixed memory partitions of 2 MB, 4 MB, 8 MB, and 16 MB. In ordinary fixed partitioning, the largest partition is 16 MB, so a process larger than 16 MB cannot fit into any one partition as a whole.

With overlays, a larger program may run if its modules can be arranged so that only the necessary parts need to be resident at the same time.

## 2. How Overlays Work

1. Divide the program or object code into modules based on functionality.
2. Load the module needed first into main memory.
3. Execute that module.
4. When it is no longer needed, replace it with another module.
5. Continue loading modules as required.

The whole program does not need to be in RAM at once. However, the modules that must coexist—such as shared routines or data—must fit in memory along with the active overlay and the overlay-management code.

## 3. Overlay Design and Limitations

The lecture emphasizes that overlay design requires the programmer to decide how to divide the program and which modules can replace one another.

- Modules should be arranged so that only compatible, non-concurrent parts occupy the same overlay space.
- If one module depends on code or data that has been replaced, the program may fail unless that dependency is handled.
- A poor overlay plan may load modules in an unsuitable order or leave a required routine unavailable.
- Overlay management adds complexity.

The lecture notes that overlays have been used in embedded systems, where memory is limited and program functionality may be relatively fixed. It contrasts this manual planning with modern general-purpose systems, which commonly use virtual memory and demand paging.

## 4. Worked Example: Two-Pass Assembler

### Given

A two-pass assembler has the following components:

| Component | Size |
|---|---:|
| Pass 1 | 80 KB |
| Pass 2 | 90 KB |
| Symbol table | 30 KB |
| Common routine | 20 KB |
| Overlay driver | 10 KB |

Only one pass is in use at a time. The symbol table, common routine, and overlay driver are assumed to be required alongside whichever pass is active.

Find the minimum partition / memory size required to use overlays.

### Step 1: Total size without overlays

If both passes were resident simultaneously:

\[
80 + 90 + 30 + 20 + 10 = 230\text{ KB}
\]

So the full assembler would occupy 230 KB if all listed components were loaded together.

### Step 2: Memory needed while Pass 1 is active

Pass 1 and the components it requires:

\[
80 + 30 + 20 + 10 = 140\text{ KB}
\]

### Step 3: Memory needed while Pass 2 is active

Pass 2 and the components it requires:

\[
90 + 30 + 20 + 10 = 150\text{ KB}
\]

### Step 4: Choose the larger simultaneous requirement

The memory must accommodate either pass, so choose the maximum of the two requirements:

\[
\max(140,150)=150\text{ KB}
\]

**Answer: 150 KB.**

A 150 KB partition can hold Pass 2 with the symbol table, common routine, and overlay driver. Pass 1 can also fit because its simultaneous requirement is only 140 KB.

## 5. Exam Method

For overlay-size questions:

1. Identify which components must be resident together.
2. Calculate the memory required for each possible overlay or execution phase.
3. Include all shared components required in each phase.
4. Take the **maximum** of the simultaneous memory requirements.
5. Do not add mutually exclusive overlays together when they are never needed at the same time.

For the assembler example:

- Pass 1 phase = 140 KB
- Pass 2 phase = 150 KB
- Minimum required partition = **150 KB**

## 6. Key Points

- Overlays allow a program larger than RAM to execute by loading only the required modules at a time.
- Program modules are planned so that modules not needed simultaneously can share the same memory area.
- Overlay design requires careful planning of module boundaries and dependencies.
- In the lecture's example, the full program size is 230 KB, but the required partition size is only 150 KB because Pass 1 and Pass 2 are never needed at the same time.
- The lecture contrasts overlays with virtual memory and demand paging, which are commonly used in general-purpose computer systems.
