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

# GUI-Based Development Using noVNC Desktop

In addition to the terminal-based workflow, GitHub Codespaces provides a graphical Linux desktop through **noVNC**. This enables the use of GUI-based development tools directly from a web browser without requiring a local Linux installation.

The graphical desktop is particularly useful for Electronic Design Automation (EDA) tools such as **Magic**, **GTKWave**, **Xschem**, and graphical editors like **gedit**, which are extensively used throughout the internship.

---

## Launching the noVNC Desktop

The graphical desktop was launched by opening the forwarded **noVNC Desktop (Port 6080)** from the GitHub Codespaces **PORTS** tab.

After opening `vnc_lite.html`, a complete Linux desktop environment became available inside the browser.
<p align="center">
  <img src="images/no_vnc_interface.png" width="1000"/>
</p>
<p align="center">
  <img src="images/Screenshot 2026-07-19 162934.png" width="1000"/>
</p>
### GUI Environment Flow

```text
GitHub Codespace
        │
        ▼
Forwarded Port (6080)
        │
        ▼
noVNC
        │
        ▼
Linux Desktop
        │
        ▼
GUI Applications
```

---

## Accessing the Project Workspace

A terminal was opened inside the graphical desktop and navigated to the project workspace.

```bash
cd /workspaces/vsd-riscv2
cd samples
ls -ltr
```
<p align="center">
  <img src="images/Screenshot 2026-07-19 163451.png" width="1000"/>
</p>
<p align="center">
  <img src="images/linux.png" width="1000"/>
</p>
<p align="center">
  <img src="images/files_in_vsd_riscv2.png" width="1000"/>
</p>
The sample programs and build files were successfully verified.

---

## Native Compilation and Execution (Host x86)

The reference application was first compiled using the native GCC compiler.

```bash
gcc sum1ton.c
```

The generated executable was then executed.

```bash
./a.out
```
<p align="center">
  <img src="images/Screenshot 2026-07-19 193212.png" width="1000"/>
</p>
The generated executable was then executed.
### Output

```text
Sum from 1 to 9 is 45
```
<p align="center">
  <img src="images/verification_of_sum1ton.o.png" width="1000"/>
</p>
This confirms that the application executes correctly on the **host x86-64 processor** using the native compiler.

---

## RISC-V Compilation and Execution

The same source program was then compiled for the RISC-V architecture.

```bash
riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c
```
<p align="center">
  <img src="images/sum1ton.o_output.png" width="1000"/>
</p>
The generated executable was executed using the Spike ISA Simulator and Proxy Kernel.

```bash
spike pk sum1ton.o
```
<p align="center">
  <img src="images/spike_output_of_sum1ton.o.png" width="1000"/>
</p>
### Output

```text
Sum from 1 to 9 is 45
```

This confirms that the application executes successfully on the simulated RISC-V processor.

---

## Editing Source Code Using gedit

The graphical editor **gedit** was used to edit the source code directly within the noVNC desktop.

```bash
gedit sum1ton.c &
```
<p align="center">
  <img src="images/code_changed_to_10.png" width="1000"/>
</p>
<p align="center">
  <img src="images/sum_of_10.png" width="1000"/>
</p>

After modifying the source code, the application was rebuilt using the RISC-V compiler and executed again with Spike.

This validated the complete edit → compile → execute workflow inside the graphical development environment.

---
# Engineering Concepts

## CLI vs GUI Development

Throughout this task, two different development workflows were explored.

### Command-Line Workflow

```text
VS Code Terminal
        │
        ▼
Compile
        │
        ▼
Execute
        │
        ▼
Debug
```

### Graphical Workflow

```text
noVNC Desktop
        │
        ▼
GUI Applications
        │
        ▼
Edit Source Code
        │
        ▼
Compile & Execute
```

Both workflows operate on the same project workspace but provide different development experiences depending on the tools being used.

---
## Native Execution vs Cross Execution

The same C source program can be executed in two different ways.

### Native Execution

```text
sum1ton.c
      │
      ▼
Native GCC
      │
      ▼
a.out
      │
      ▼
Host x86-64 Processor
```

### Cross Execution

```text
sum1ton.c
      │
      ▼
RISC-V Cross Compiler
      │
      ▼
sum1ton.o
      │
      ▼
Spike ISA Simulator
      │
      ▼
Simulated RISC-V Processor
```

Although both executions produce the same output, they target completely different processor architectures.

| Native Execution | Cross Execution |
|------------------|-----------------|
| Executed on the host x86-64 processor | Executed on a simulated RISC-V processor |
| Compiled using `gcc` | Compiled using `riscv64-unknown-elf-gcc` |
| Generates `a.out` | Generates a RISC-V executable |
| Runs directly on the host | Requires Spike ISA Simulator |
---

# Commands Executed

| No. | Command | Purpose | Observation |
|----:|---------|----------|-------------|
| 1 | `pwd` | Display the current working directory. | Verified the repository was mounted under `/workspaces/vsd-riscv2`. |
| 2 | `ls` | List the repository contents. | Verified the top-level directories and files. |
| 3 | `ls -la .devcontainer` | Inspect the Codespaces configuration files. | Identified the Dockerfile, `devcontainer.json`, and `supervisord.conf`. |
| 4 | `ls -la samples` | Explore the sample application directory. | Verified the presence of sample programs, startup assembly, and the Makefile. |
| 5 | `cat samples/Makefile` | Examine the build automation script. | Understood the compilation workflow and generated outputs. |
| 6 | `cd samples` | Navigate to the sample application directory. | Accessed the build environment successfully. |
| 7 | `make hello-spike` | Build the reference Hello World application. | Generated `hello.c` and `hello.elf` successfully. |
| 8 | `ls -l` | Verify generated build artifacts. | Confirmed the successful creation of `hello.c` and `hello.elf`. |
| 9 | `file hello.elf` | Inspect the executable format. | Verified that `hello.elf` is a 64-bit RISC-V ELF executable. |
| 10 | `spike --help` | Display Spike ISA Simulator usage information. | Verified that Spike was installed and available. |
| 11 | `which pk` | Locate the Proxy Kernel executable. | Confirmed the availability of the Proxy Kernel (`pk`). |
| 12 | `spike pk hello.elf` | Execute the reference RISC-V application. | Successfully executed the program using Spike and the Proxy Kernel. |
| 13 | `cat sum1ton.c` | Inspect the source code of the reference application. | Understood the application logic before compilation. |
| 14 | `riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c` | Cross-compile the application for the RISC-V architecture. | Successfully generated the executable `sum1ton.o`. |
| 15 | `file sum1ton.o` | Verify the executable format. | Confirmed that `sum1ton.o` is a RISC-V ELF executable. |
| 16 | `spike pk sum1ton.o` | Execute the RISC-V application. | Successfully displayed the expected output: **"Sum from 1 to 9 is 45"**. |
| 17 | `cd /workspaces/vsd-riscv2` | Navigate to the project workspace from the noVNC desktop. | Successfully accessed the mounted GitHub Codespaces workspace. |
| 18 | `cd samples` | Open the sample application directory inside the GUI environment. | Verified the project structure from the graphical desktop. |
| 19 | `ls -ltr` | List the contents of the sample directory. | Confirmed the availability of source files, executables, and build files. |
| 20 | `gcc sum1ton.c` | Compile the application using the native GCC compiler. | Successfully generated the host executable `a.out`. |
| 21 | `./a.out` | Execute the native x86 application. | Successfully displayed the expected output on the host processor. |
| 22 | `riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c` | Cross-compile the application for the RISC-V architecture from the GUI environment. | Successfully regenerated the RISC-V executable. |
| 23 | `spike pk sum1ton.o` | Execute the RISC-V executable using Spike. | Successfully validated execution on the simulated RISC-V processor. |
| 24 | `gedit sum1ton.c &` | Open the source code using the graphical editor. | Successfully edited the application using the noVNC desktop environment. |
| 25 | `riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c` | Rebuild the modified application. | Successfully compiled the updated source code. |
| 26 | `spike pk sum1ton.o` | Execute the modified RISC-V application. | Verified that the modified source code executed successfully. |

---

# Key Learnings

| Topic | Learning |
|--------|----------|
| GitHub Codespaces | Provides a fully configured cloud-based development environment without requiring local tool installation. |
| Repository Structure | Understanding the project organization simplifies navigation, development, and debugging. |
| `.devcontainer` | Defines the complete software environment and automatically configures the development container. |
| Makefile | Automates the build process, reducing manual compilation steps and ensuring reproducible builds. |
| Cross Compilation | Applications developed on an x86-64 host can be compiled for a different target architecture such as RISC-V using a cross compiler. |
| Native Compilation | The native GCC compiler generates executables that run directly on the host x86-64 processor. |
| ELF Executable | ELF files contain executable machine code, memory layout information, program entry points, and debugging symbols. |
| Spike ISA Simulator | Spike emulates a complete RISC-V processor, enabling RISC-V applications to execute on an x86-64 host system. |
| Proxy Kernel (`pk`) | Provides the runtime environment required to initialize memory and transfer control to the application before execution. |
| Native vs Cross Execution | The same C source code can be executed either as a native x86 application or as a RISC-V application through cross compilation and simulation. |
| noVNC Desktop | Provides a complete Linux graphical desktop within GitHub Codespaces for running GUI-based development tools. |
| GUI-Based Development | Enables graphical applications such as **gedit**, **GTKWave**, **Magic**, and **Xschem** to be used directly from a web browser. |
| Source Code Editing | Applications can be modified, rebuilt, and re-executed entirely within the graphical desktop environment. |
| Software Execution Flow | Understood the complete workflow: **C Source Code → Cross Compiler → ELF Executable → Spike ISA Simulator → Proxy Kernel → Program Execution**. |
| Development Workflow | Successfully completed the complete engineering workflow of exploring, compiling, executing, modifying, rebuilding, verifying, and documenting a RISC-V application. |

---

# Task Outcome

By completing **Task-1: Environment Setup & RISC-V Reference Bring-Up**, I successfully:

- Configured the GitHub Codespaces development environment.
- Explored and understood the reference repository structure.
- Analyzed the Makefile-based build system.
- Compiled and executed reference RISC-V applications.
- Verified the generated ELF executables.
- Understood the complete software execution flow using the Spike ISA Simulator and Proxy Kernel.
- Compared native x86 execution with cross-compiled RISC-V execution.
- Explored both command-line and graphical development workflows using the noVNC desktop.
- Edited, rebuilt, and re-executed applications using the graphical editor.
- Established a complete development environment for the upcoming VSDFPGA Labs, custom IP development, and FPGA-based RISC-V system implementation.

This task provides the software development foundation required for the remaining stages of the internship, including hardware IP design, SoC integration, and FPGA implementation.

---
