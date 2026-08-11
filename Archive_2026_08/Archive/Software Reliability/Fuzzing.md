---
id: Fuzzing
aliases: []
tags: []
---

AFL: American Fuzzy Lop is a coverage based fuzzer. This means that it must have a custom compiler to instrument the target program so that it can accurately obtain this coverage data to guide mutations. 

AFL ships with multiple C compilers: gcc & clang because real software projects have slight variations between the 2 compilers and how compatible they are with code. The idea is to have AFL as a drop-in solution, but some code which is gcc specific (particular extensions etc.) may fail when compiled with clang for example. AFL having many compilers will reduce the effect of being limited by a a specific compiler.
