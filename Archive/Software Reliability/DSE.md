---
id: Lecture4_DSE
aliases: []
tags: []
---

# DSE #

- Exploration of program space by considering variables as "symbols" instead of "concrete" values.
- The DSE engine, dynamically "explores" program paths building constrains around the inputs of interest for every path. 
  When a path terminates due to a crash... assertion... or any other defined condition, the constraints are solved for using a constraint solver to get concrete inputs which give us real coverage information. 
- The paths explored are related to branches in the code, and we are only considering the ones which involve our variable of interest.


## Key features of KLEE as a DSE ##

- Instruments the intermediate level byte code (LLVM bitcode) [[LLVM|LLVM #]]
- General KLEE flow:

  while there are unexplored states:
    pick one state (using search heuristic)
    symbolically execute instructions along that path
    when you reach a branch:
        fork into new states (add to pool)
    if the state reaches program end (exit/bug/assert):
        send its path condition to the solver
        get a concrete input
        save that input as a test case


## Path Explosion

- DSE and Path explosion. The "search heuristic" is important as it helps prune which states to explore. With every branch for a symbol, we fork the program into 2 branches. For real code with thousands or millions of branches like this, there is an explosion of paths.
- The "search heuristic" changes for different DSE's. KLEE uses MD2U- Minimum distance (from state s) to uncovered code. This heuristic systematically improves the overall coverage as for every state in the pool of unexplored states, we weight the probability of exploring a path based on close it is to a region of uncovered (unexplored) code. These states are given higher priority.
- We can also merge paths to avoid path explosion.
- Regression suites: These are manual tests written by developers to test a program. There are some techniques and advantages to bootstrapping exploration by using these existing regression suites.
- Redundant path exploration:
  Similar idea as pruning, but it builds on the mathematical idea that after a given point, if two paths are equivalent to remove one of them from the pool of unexplored paths.
  Complicated looking algorithm:

    When program point P reached with PC:

      Forall PCa ∈ CacheSet(P):
         If TransCl(PC, ReadSet(P, PCa)) = PCa:
            Then stop exploration   ← prune path

      Explore execution tree rooted at P in DFS mode:
         Compute ReadSet(P, PC)

      CacheSet(P) = CacheSet(P) ∪ TransCl{PC, ReadSet(P,PC)}
 
 Basically- ReadSet identifies what will still matter in the future.
            TransCl filters out the constraints irrelevant to those variables.


## Constraint-Solving ##

Constraint solving is inherently difficult for 2 reasons:
  - This is a difficult problem to solve. Verifying memory and numeric consistency in SAT format is difficult.
  - Very performance intensive.

  - BV format is used to basically encode all arithmetic operations in BV format. These can be easily reasoned by a SAT solver which is basically like a boolean circuit.
  - Memory becomes harder is enforced using things called array axioms. These are "proofs" on how arrays work and making sure that these assertions are hit. These axioms can be reduced to improve performance, and only introduce necessary ones incrementally if the SAT fails.
  - The performance problem is solved at many layers:
    - Simply use the best performing SAT solver for a given problem.
    - Optimize the symbolic executor by using caching techniques for SAT outputs and SAT models. The same constraints are likely to come up again so having them cached makes it easier to retrieve. Caching the results is also useful if a new set of constraints are a superset of a previous result. We can just retrive that result.
   - Another approach is to remove irrelevant constraints. If there are a bunch of constraints and we reach a branch where most of variables are not affecting the branch trajectory, just remove the irrelevant branch conditions before we pass to the SAT.
   - One other approach is Array_transformations:
	   - *Program transformations which can sometimes improve performance actually slow down symbolic execution analysis*
	   - In some situations when we know the layout of an array, we can use index based optimisations techniques which will convert to simple true/false that the constraint solver can use. 


## Applications

- Most applications have an "*environmental*" factor such as CLI, file inputs etc. We need a may to model these environmental factors...
	- The 2 obvious techniques are to:
		1. Concretise the environment. This leads to an under-approximation of the environment with missing paths, interactions between paths...
		2. Unconstrained environment. This is an over-approximation. The obvious problem here is infeasible paths.
- KLEE implements its own symbolic POSIX layer.
	- POSIX: Portable Operating System Interface. 
		KLEE will essentially intercept system calls like read() etc. and has its own symbolic implementation of these system calls written in plain C.
- KLEE also supports differential testing, and is particularly useful for comparing an optimized vs unoptimized version of a program.
	- *semantic difference*: **different** behaviour.
	- syntantic difference: Different source code.

	In these examples of differential testing, the unoptimized version of the program can be thought of as an *oracle* and its implementation assumed to be a reference. Differential testing allows us to quickly see **differences** between the symbolic outputs of the 2 programs. We dont even need to solve things like FP rules etc. using the solver, as long as we see a difference in the 2 symbolic outputs, we can see that there is a semantic difference. The old version **is** the oracle, so there is no need for further specifications.

	Developers are given the opportunity in KLEE to add assumptions to their code so that semantic differences can be identified properly. 
	
	One of the advantages also is **bounded verification** with DSE. Developers are given a guarentee that there is no semantic difference for a constrained input size. We can guarentee equivalence between the 2 programs up till a given point. This is due to limitations of the solvers.
