---
id: Compiler fuzzing
aliases: []
tags: []
---

Starting from slide 38:

Subtle difference between Compiler bug type:
	-> Compiler Crashes: This is when the compiler itself crashes. This is like a segfault or whatever its actually something wrong with the compiler program itself.
	-> Miscompilation bugs: This is more sinister and is when the compiler does not crash but the compiled binary is different to what the source code specifies. This is often the result of compiler optimisations or incorrect compilation logic. This is entirely the compiler's fault.


EMI & Differential testing is a workaround to test miscompilation bugs. In an ideal world we would have an oracle compiler that we could compare all compiler outputs to, but this obviously does not exist. 

Differential testing is comparing the same input program across multiple compilers and seeing where there are "differences" between the outputs. Generally using a majority correct voting mechanism to detect bugs.

EMI is slightly different:
	1- Start with a a program P compiled with compiler C which is run with input I.
	2- Track the coverage of program executution P, i.e which parts of P executed and which didnt.
	3- Split the LoC of P into I-Live (did execute) and I-Dead (Didnt execute) for the given input I.
	4- Mutate the I-Dead section of the code of P to generate P1, P2 ... Pn
	5- Recompiler P1...Pn with C and run with the same original input I.
	6- Outputs should remain the same as those dead sections of the code should still NOT be executed. 
	   Where there are differences, this must have been due to compiler optimisations which are incorrect. It must have optimised/transformed some dead code branch portions into something that DOES affect the program behaviour.


Is fuzzing important?

A lot of debate in industry vs academia over whether compiler fuzzing is fruitful. General industry view is that bugs found are mostly very artificial corner cases which do not affect real world code. Academic view is that compiler bugs are very important, and there is particularly a security risk, particularly for open source projects which when compiled using possible buggy compilers could release sensitive information. 
