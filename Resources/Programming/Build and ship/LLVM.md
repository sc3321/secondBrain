---
id: LLVM
aliases: []
tags: []
---

# LLVM #

- LLVM: Low-Level-Virtual-Machine code.
- It can be confused with a compiler, but it is rather a set of compiler tools and technologies. 
  Compilation has 3 distinct parts:
    - Front end: This is the lexer and parser and perhaps optimisations happen here too. For the LLVM framework, the common front-end is clang.
    - Middle end: This is where LLVM comes into play. The libraries, toolchain technologies etc. all work together to create an intermediate bit level interpretation of the code. It is not yet assembly, but an intermediate representation that has both a bitcode and human readable format. The key here is that this representation is architecture agnostic, i.e x86, ARM etc. 
    - Back end: The LLVM IR is converted into the final machine binaries for the target architecture.



- Many languages will compile to the LLVM IR for the optimisations that it offers. These languages include C, C++, Rust, Julia among others.
