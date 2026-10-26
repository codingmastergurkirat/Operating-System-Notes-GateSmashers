# Linker and Loader in Operating Systems

## 1. Program Execution: Overall Flow

The lecture explains how a high-level program becomes an executable program and is then placed in memory for execution.

A simplified flow is:

```text
Source program
    ↓
Compiler
    ↓
Assembly code
    ↓
Assembler
    ↓
Object file(s)
    ↓
Linker
    ↓
Executable file
    ↓
Loader
    ↓
Program in main memory
    ↓
CPU executes the program
```

A compiler toolchain may combine or automate several of these steps, but the conceptual roles remain distinct.

### Compiler
The compiler translates a high-level language program into a lower-level representation, commonly assembly code or object code depending on the toolchain. It also diagnoses many syntax and semantic errors.

### Assembler
The assembler translates assembly-language instructions into machine-code object files.

### Linker
The linker combines object files and required libraries, resolves references between separately compiled components, and produces an executable or another linked output.

### Loader
The loader prepares an executable for execution by placing its required contents into memory, arranging the program's memory image, and performing any loading-time work required by the system.

## 2. Why Is Linking Needed?

A program may consist of multiple source files, modules, functions, and libraries. These components can be compiled separately into object files.

For example:
- Object file 1 defines or uses functions from module A.
- Object file 2 defines or uses functions from module B.
- Another component uses a library function such as a printing routine.

The linker combines the required components and resolves references between them. This allows a project to be built from separately compiled modules rather than requiring all code to be written in one file.

**Key distinction:**
- **Linker:** combines and resolves program components.
- **Loader:** places the executable program into memory so it can run.

## 3. Object File Structure

The lecture lists the following common object-file components. Exact formats vary by executable format and toolchain.

### 3.1 Header
The header contains information about the object file and its layout. It may describe sections and provide offsets or other metadata used to locate them.

### 3.2 Text section
The **text section** contains the program's machine instructions, or code.

### 3.3 Data section
The **data section** contains initialized data, such as initialized global or static variables.

### 3.4 BSS section
The **BSS section** represents uninitialized or zero-initialized static-storage variables. In many object formats, it records the required size without storing all zero bytes in the file.

### 3.5 Symbol table
The **symbol table** records symbols such as function and variable names, along with information used to identify their definitions or references. The linker uses symbol information to resolve references across object files and libraries.

### 3.6 Relocation information
**Relocation information** identifies locations in the object file that need address-related adjustments when sections or symbols are assigned their final addresses.

For example, an instruction may refer to a function or data object whose final address is not known when the object file is produced. Relocation records help the linker or loader adjust that reference appropriately.

### 3.7 Debugging information
The lecture also mentions debugging information. Debug sections may map machine instructions back to source-level constructs and contain information useful to debuggers. Such information is optional and depends on the build configuration.

## 4. Relocation: Absolute vs. Relative Addressing

The lecture uses a jump instruction example to illustrate why relocation is needed.

Suppose an instruction must jump to another instruction. If the object code assumes that the target is always at an absolute address such as `105`, that assumption may become invalid if the program is loaded somewhere else in memory.

If the program's load base changes, the target instruction may move as well. Relocation information allows relevant addresses or references to be adjusted.

A simplified illustration:

```text
Original assumed target address: 105
Program's load base changes
        ↓
Linker/loader adjusts the relevant reference
        ↓
The jump reaches the intended target in the loaded program
```

The transcript describes storing an offset such as `5` and adding it to a base address. This illustrates **base-relative relocation**: the final address can be calculated from a base plus an offset. Actual relocation records and formulas depend on the object format and instruction type; not every reference is represented as a simple offset.

## 5. Example of Linking Multiple Object Files

Imagine a project that is compiled into several object files:

```text
Object file A ──┐
Object file B ──┼──> Linker ──> Executable file
Required library┘
```

One object file may refer to a function defined in another object file. The symbol table and relocation information help the linker locate the function and update references.

After linking, the executable is ready for the loading stage, subject to the platform's executable format and any runtime requirements.

## 6. What Does the Loader Do?

The loader brings an executable into the execution environment. Depending on the operating system and executable format, its work may include:
- Establishing the program's memory image
- Mapping code and data into memory
- Arranging the stack and other required regions
- Loading or mapping shared libraries
- Applying any required load-time relocations
- Transferring control to the program's entry point

The CPU then executes the program's instructions.

## 7. Linker vs. Loader

| Linker | Loader |
|---|---|
| Works with object files and libraries. | Works with an executable and its runtime dependencies. |
| Resolves references between symbols and components. | Prepares the program's memory image for execution. |
| Performs linking and may perform relocation. | Loads or maps program contents and may perform load-time relocation. |
| Produces a linked executable or linked output. | Starts the program's execution environment. |

## 8. Exam Revision Points

- A typical conceptual sequence is **compiler → assembler → linker → loader**, with object files produced before linking.
- A **linker** combines object files and libraries and resolves external references.
- A **loader** prepares the executable in memory for execution.
- The **text section** contains instructions.
- The **data section** contains initialized static/global data.
- The **BSS section** represents uninitialized or zero-initialized static-storage data.
- The **symbol table** records symbols and helps resolve references.
- **Relocation information** helps adjust address-dependent references when placement is determined.
- A program's actual load address may differ from an address assumed during compilation.
- Object-file layouts and relocation details vary by platform and executable format.
