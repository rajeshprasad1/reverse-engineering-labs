# OpenSecurityTraining2: Architecture 1001
## Executive Summary
This repository documents my progression through the OpenSecurityTraining2 (OST2) Architecture 1001: x86-64 Assembly course. The primary assignment involves the classic "Binary Bomb": a compiled executable comprising multiple discrete phases that "detonate" upon receiving incorrect input.

The objective is to meticulously analyze the raw x86-64 assembly, decipher the execution flow, and deduce the exact sequence of inputs required to safely defuse every phase, including a hidden secret stage. This analysis bridges the gap between low-level machine execution and high-level C code, detailing the reconstruction of compiler-optimized control flows, loops, jump tables, and complex data structures.

## Architecture & Calling Convention
The binaries are compiled for the **x86-64 architecture**. Throughout the reverse-engineering process, the execution logic heavily relies on the **Windows x64 calling convention (ABI)** to pass arguments into subroutines. 

By mapping registers to this specific ABI, arguments can be accurately tracked:
* `RCX` / `ECX` operates as the first argument (`arg_0`).
* `RDX` / `EDX` operates as the second argument (`arg_8`).
* `R8` / `R8D` operates as the third argument (`arg_10`).

## Tools & Methodology
The solutions and C reconstructions were derived using a combination of static analysis, dynamic debugging, and mathematical unwinding:
* **Static Analysis:** Disassemblers (such as IDA) were used to map out control flow, identify jump tables, and extract static memory structures.
* **Dynamic Analysis:** Debuggers were utilized to inspect runtime register states, verify condition flags, and confirm the lengths and values of strings in memory.
* **Memory Extraction & Structuring:** Statically allocated structures; such as integer arrays, linked list nodes, and Binary Search Tree (BST) elements were dumped directly from memory to statically derive the required permutations and paths without brute-forcing.

## Table of Contents

### Core Bomb Lab Analysis
* [**Phase 1 — String Comparison**](Binary_Bomb/Phase-1.md): Analyzes a standard `strcmp` implementation, satisfying the check via length validation and a byte-by-byte comparison loop.
* [**Phase 2 — Input Sequence**](Binary_Bomb/Phase-2.md): Deduces a mathematical constraint loop utilizing highly optimized logical left shifts (`shl`) in place of standard multiplication.
* [**Phase 3 — Reconstructing a Switch-Case Jump Table**](Binary_Bomb/Phase-3.md): Explores how high-level `switch` statements are translated into bounds-checked indirect jump tables at the assembly level.
* [**Phase 4 — Reconstructing a Recursive Binary Search**](Binary_Bomb/Phase-4.md): Unpacks recursive function calls, midpoint division optimizations (`cdq`, `sub`, `sar`), and dynamic return value accumulation.
* [**Phase 5 — Following an Indirect Array Traversal**](Binary_Bomb/Phase-5.md): Traces an indirect, pointer-chasing array traversal safely bounded by a bitwise AND mask (`& 0xF`).
* [**Phase 6 — Reconstructing and Reordering a Linked List**](Binary_Bomb/Phase-6.md): Details the memory alignment of a statically allocated linked list and the logic required to mathematically sort and rewire its nodes.
* [**Secret Phase — Reconstructing a Binary Search Tree**](Binary_Bomb/Secret-Phase.md): Covers uncovering a hidden trigger string and mathematically unwinding a recursive Binary Search Tree traversal to derive a target value.

### Additional Assignments
* [**RTFM & WTFI Manual Encoding**](RTFM/RTFM.md): Details the manual construction and encoding of x86 machine-code bytes (such as `MOV`, `SAHF`, `JZ`, and `AND`) using the Intel Software Developer's Manual and assembler data directives.