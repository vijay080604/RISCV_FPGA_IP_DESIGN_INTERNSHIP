# Task-1: Environment Setup & RISC-V Reference Bring-Up

## Objective

Set up the official development environment, validate the RISC-V reference design, and prepare the workflow for upcoming IP development and FPGA tasks.

---

## Learning Objectives

- Set up the GitHub Codespaces development environment.
- Explore the repository structure.
- Understand the development environment configuration.
- Identify the sample RISC-V applications and build files.
- Understand the build workflow before executing the reference program.

---

## Activities

- Fork the official `vsd-riscv2` repository
- Launch GitHub Codespace
- Explore the repository structure
- Analyze the sample build system
- Build and execute the reference RISC-V program *(In Progress)*
- Clone and run the `vsdfpga_labs`
- Understand the RISC-V execution flow
- Prepare the local development environment

---

## Repository

### Official Repository

https://github.com/vsdip/vsd-riscv2

### Personal Fork

https://github.com/vijay080604/vsd-riscv2

---

## Environment

| Item | Details |
|------|---------|
| Development Environment | GitHub Codespaces |
| Interface | VS Code Desktop |
| Operating System | Ubuntu (Codespace Container) |
| Working Directory | `/workspaces/vsd-riscv2` |

---

## Repository Structure

```text
vsd-riscv2
│
├── .devcontainer
├── images
├── samples
└── README.md
```

### Directory Overview

| Directory / File | Description |
|------------------|-------------|
| `.devcontainer` | Configuration files used to create the GitHub Codespaces development environment. |
| `images` | Stores images used throughout the project documentation. |
| `samples` | Contains sample RISC-V programs, startup assembly, and the Makefile used for compilation. |
| `README.md` | Provides instructions for setting up and using the reference repository. |

---

## Repository Exploration

### Development Environment Files

The `.devcontainer` directory contains the files responsible for creating and configuring the development environment.

| File | Purpose |
|------|---------|
| `Dockerfile` | Defines the software environment used by GitHub Codespaces. |
| `devcontainer.json` | Configures the Codespace environment and startup behavior. |
| `supervisord.conf` | Starts background services required by the Codespace environment. |

---

### Sample Programs

The `samples` directory contains the reference programs used throughout the internship.

| File | Purpose |
|------|---------|
| `Makefile` | Automates the build process for the sample program. |
| `load.S` | Startup assembly executed before the C application begins. |
| `sum1ton.c` | Sample C program demonstrating a simple computation. |
| `1ton_custom.c` | Sample C program for demonstrating custom functionality. |

---

## Build System Overview

The sample project uses a Makefile to automate compilation.

### Build Flow

```text
Makefile
     │
     ▼
Generate hello.c
     │
     ▼
RISC-V GCC Compiler
     │
     ▼
Generate hello.elf
```

At this stage, the Makefile has been analyzed to understand the compilation flow. The build process will be executed in the next section.

---

# Proof of Completion

## Step 1: Repository Fork

<p align="center">
  <img src="images/forked_repository.png" alt="Forked Repository" width="1000"/>
</p>

**Figure 1.** Successfully forked the official `vsd-riscv2` repository into my personal GitHub account.

---

## Step 2: GitHub Codespace

<p align="center">
  <img src="images/code_space_setup_01.png" width="1000"/>
</p>

<p align="center">
  <img src="images/code_space_setup_02.png" width="1000"/>
</p>

**Figure 2.** GitHub Codespace was successfully initialized and connected using VS Code Desktop. The repository was mounted successfully and verified using the `pwd` and `ls` commands.

---

## Step 3: Repository Exploration

<p align="center">
  <img src="images/repository_exploration.png" width="1000"/>
</p>

**Figure 3.** Explored the repository structure and inspected the `.devcontainer` and `samples` directories to understand the development environment and sample project organization.

---
---

# Build System Overview

The sample project uses a **Makefile** to automate the compilation of a RISC-V application. Instead of manually creating source files and invoking the compiler, the Makefile defines a sequence of build steps that generate the executable.

## Build Flow

```text
                Makefile
                    │
                    ▼
         Generate hello.c
                    │
                    ▼
   RISC-V Cross Compiler
(riscv64-unknown-elf-gcc)
                    │
                    ▼
         Generate hello.elf
```

The build process generates a simple C program (`hello.c`) and compiles it using the RISC-V cross compiler to produce a RISC-V executable (`hello.elf`).

---

# Reference Program Compilation

## Step 1: Navigate to the Sample Directory

```bash
cd samples
```

The `samples` directory contains the Makefile and the reference programs used throughout the internship.

---

## Step 2: Build the Reference Program

```bash
make hello-spike
```
<p align="center">
  <img src="images/Screenshot 2026-07-19 171236.png" width="1000"/>
</p>
<p align="center">
  <img src="images/Screenshot 2026-07-19 173204.png" width="1000"/>
</p>
### Build Output

```text
riscv64-unknown-elf-gcc -o hello.elf hello.c
Done.
```

The Makefile automatically:

- Generates the `hello.c` source file.
- Invokes the RISC-V cross compiler.
- Produces the executable file `hello.elf`.

### Compilation Flow

```text
hello.c
     │
     ▼
RISC-V Cross Compiler
(riscv64-unknown-elf-gcc)
     │
     ▼
hello.elf
```

---

## Step 3: Verify the Generated Files

```bash
ls -l
```

### Output

```text
hello.c
hello.elf
```

The successful generation of both files confirms that the compilation completed without errors.

---

## Step 4: Inspect the Executable

```bash
file hello.elf
```
<p align="center">
  <img src="images/Screenshot 2026-07-19 173652.png" width="1000"/>
</p>
### Output

```text
ELF 64-bit LSB executable,
UCB RISC-V,
RVC,
double-float ABI,
statically linked,
not stripped
```

This verifies that the generated executable targets the **RISC-V architecture** rather than the host machine.

---

# Reference Program Execution

## Step 1: Explore the Spike ISA Simulator

```bash
spike --help
```

This command displays the available options and confirms that the Spike ISA Simulator is installed in the development environment.

---

## Step 2: Verify the Proxy Kernel

```bash
which pk
```
<p align="center">
  <img src="images/Screenshot 2026-07-19 175700.png" width="1000"/>
</p>
### Output

```text
/opt/riscv/riscv64-unknown-elf/bin/pk
```

The output confirms that the Proxy Kernel (`pk`) is available and ready to execute RISC-V applications.

---

## Step 3: Execute the Reference Program

```bash
spike pk hello.elf
```

### Observation

```text
bbl loader
```

The program executed successfully and returned to the terminal without errors.

The message **"bbl loader"** is displayed by the **Proxy Kernel**, indicating that the runtime environment was initialized before transferring control to the application.

Since the sample program only returns a value and does not print anything to the console, no additional output is expected.

---

# Engineering Concepts

## Why is a Cross Compiler Required?

The reference application is written in the C programming language, which cannot be executed directly by a RISC-V processor. It must first be translated into RISC-V machine instructions.

Because the development environment uses an **x86-64 processor** while the target architecture is **RISC-V**, a **cross compiler** is required.

### Cross Compilation Flow

```text
C Source Code
      │
      ▼
RISC-V Cross Compiler
      │
      ▼
RISC-V Executable (ELF)
```

---

## What is an ELF File?

ELF (Executable and Linkable Format) is the executable format generated by the RISC-V compiler.

In addition to machine instructions, an ELF file contains:

- Program entry point
- Executable code
- Data sections
- Memory layout information
- Symbol information for debugging

The executable generated during this task is:

```text
hello.elf
```

---

## Why Can't the Host Machine Execute `hello.elf` Directly?

The development environment runs on an **x86-64 processor**, whereas `hello.elf` contains **RISC-V machine instructions**.

Since both processors use different Instruction Set Architectures (ISAs), the executable cannot run directly on the host machine.

Instead, the Spike ISA Simulator emulates a RISC-V processor and executes the program.

### Host vs Target Architecture

```text
        x86-64 Host Machine
                │
                ▼
     RISC-V Cross Compiler
                │
                ▼
          hello.elf
     (RISC-V Executable)
                │
                ▼
      Spike ISA Simulator
                │
                ▼
       Proxy Kernel (pk)
                │
                ▼
        Execute main()
```

---

## Understanding the Spike ISA Simulator

Spike is the official RISC-V ISA Simulator.

Instead of translating instructions, Spike behaves like a software implementation of a RISC-V processor. It emulates the processor architecture, allowing RISC-V applications to execute on an x86-64 host machine.

Its primary responsibilities include:

- Executing RISC-V instructions
- Emulating processor registers
- Simulating memory operations
- Supporting debugging and instruction tracing

---

## Understanding the Proxy Kernel (pk)

The Proxy Kernel (`pk`) is a lightweight runtime environment used with the Spike simulator.

Before the application begins execution, the Proxy Kernel:

- Loads the executable into memory
- Initializes the runtime environment
- Sets up the program execution context
- Transfers control to the application's `main()` function

Unlike a full operating system, the Proxy Kernel provides only the essential services required to execute bare-metal RISC-V applications.

---

## Complete Software Execution Flow

The following diagram summarizes the complete workflow followed during this task.

```text
                C Program
              (hello.c)
                    │
                    ▼
     RISC-V Cross Compiler
(riscv64-unknown-elf-gcc)
                    │
                    ▼
              hello.elf
        (Executable File)
                    │
                    ▼
        Spike ISA Simulator
                    │
                    ▼
         Proxy Kernel (pk)
                    │
                    ▼
      Execute RISC-V Instructions
                    │
                    ▼
              main()
                    │
                    ▼
         Program Terminates
```
---

# Running the First RISC-V Reference Program

After verifying the toolchain using the simple `hello-spike` example, the next step was to compile and execute one of the reference applications provided in the repository.

The sample program `sum1ton.c` demonstrates the complete RISC-V software execution flow by calculating the sum of numbers from **1 to 9** and displaying the result through the Proxy Kernel.

---

## Step 1: Inspect the Source Program

```bash
cat sum1ton.c
```
<p align="center">
  <img src="images/sum1ton.c_code.png" width="1000"/>
</p>
The source code was inspected before compilation to understand its functionality and expected output.

---

## Step 2: Compile the Program

```bash
riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c
```
<p align="center">
  <img src="images/sum1ton.o_output.png" width="1000"/>
</p>
### Compilation Flow

```text
sum1ton.c
      │
      ▼
RISC-V Cross Compiler
(riscv64-unknown-elf-gcc)
      │
      ▼
sum1ton.o
```

The compiler successfully generated the executable `sum1ton.o`.

---

## Step 3: Execute the Program

```bash
spike pk sum1ton.o
```
<p align="center">
  <img src="images/spike_output_of_sum1ton.o.png" width="1000"/>
</p>
<p align="center">
  <img src="images/verification_of_sum1ton.o.png" width="1000"/>
</p>
### Program Output

```text
Sum from 1 to 9 is 45
```

The output confirms that:

- The RISC-V cross compiler generated a valid executable.
- The Spike ISA Simulator successfully executed the program.
- The Proxy Kernel correctly initialized the runtime environment before transferring control to the application.

---

## Complete Execution Flow

```text
          sum1ton.c
               │
               ▼
    RISC-V Cross Compiler
               │
               ▼
      sum1ton.o (ELF)
               │
               ▼
      Spike ISA Simulator
               │
               ▼
       Proxy Kernel (pk)
               │
               ▼
      Execute Application
               │
               ▼
   Sum from 1 to 9 is 45
```


---

# Commands Executed

| No. | Command | Purpose | Observation |
|----:|---------|----------|-------------|
| 1 | `pwd` | Display the current working directory. | Verified the repository was mounted under `/workspaces/vsd-riscv2`. |
| 2 | `ls` | List the repository contents. | Verified the top-level directories and files. |
| 3 | `ls -la .devcontainer` | Inspect the development environment configuration files. | Identified the Dockerfile, Codespace configuration, and supervisor configuration. |
| 4 | `ls -la samples` | Explore the sample program directory. | Identified the Makefile, startup assembly, and sample C programs. |
| 5 | `cat samples/Makefile` | Display the Makefile contents. | Understood the automated build process. |
| 6 | `cd samples` | Change to the sample directory. | Accessed the build environment. |
| 7 | `make hello-spike` | Build the reference application. | Successfully generated `hello.c` and `hello.elf`. |
| 8 | `ls -l` | Verify generated build artifacts. | Confirmed the creation of `hello.c` and `hello.elf`. |
| 9 | `file hello.elf` | Inspect the executable format. | Verified the executable as a 64-bit RISC-V ELF file. |
| 10 | `spike --help` | Display Spike simulator usage information. | Confirmed the installation and available simulator options. |
| 11 | `which pk` | Locate the Proxy Kernel executable. | Verified that the Proxy Kernel (`pk`) is installed and accessible. |
| 12 | `spike pk hello.elf` | Execute the reference application. | Successfully executed the sample program using Spike and the Proxy Kernel. |
| 13 | `cat sum1ton.c` | Inspect the reference source program. | Understood the application logic before compilation. |
| 14 | `riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c` | Compile the reference RISC-V application. | Successfully generated the executable `sum1ton.o`. |
| 15 | `file sum1ton.o` | Verify the generated executable format. | Confirmed that the generated file is a RISC-V ELF executable. |
| 16 | `spike pk sum1ton.o` | Execute the compiled RISC-V application. | Successfully produced the expected output: **"Sum from 1 to 9 is 45"**. |

---

# Key Learnings

| Topic | Learning |
|--------|----------|
| GitHub Codespaces | Provides a preconfigured cloud-based development environment for RISC-V development. |
| Repository Structure | The repository is organized into environment configuration, documentation, and sample applications. |
| `.devcontainer` | Defines the software environment automatically configured within GitHub Codespaces. |
| Makefile | Automates the generation and compilation of RISC-V applications. |
| Cross Compilation | Applications are compiled on an x86-64 host to generate executables for the RISC-V architecture. |
| ELF Executable | The compiler generates an ELF executable containing machine instructions, memory layout, and debugging information. |
| Spike ISA Simulator | Emulates a RISC-V processor, enabling RISC-V applications to execute on an x86-64 host system. |
| Proxy Kernel (`pk`) | Provides a lightweight runtime environment that initializes the application before transferring control to `main()`. |
| Reference Program Execution | Successfully compiled and executed multiple RISC-V reference applications using the Spike simulator. |
| Program Verification | Verified the correctness of the reference application by comparing the execution result with the expected output. |
| Software Execution Flow | Understood the complete workflow: **C Source → Cross Compiler → ELF Executable → Spike ISA Simulator → Proxy Kernel → Program Execution**. |

---
