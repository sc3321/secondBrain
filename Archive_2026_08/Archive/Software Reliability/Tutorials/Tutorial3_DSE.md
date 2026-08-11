---
id: Tutorial3_DSE
aliases: []
tags: []
---

Question 1. 

As mentioned both in the EXE paper1
in Section 3.2 and in the KLEE paper2
(page 220,
footnote 9), EXE and KLEE will allocate memory at concrete addresses. Therefore, a call to malloc will
return a concrete value. Discuss whether this is an under-approximation, an over-approximation, or neither?
The addresses returned by malloc are likely to be different on different runs of EXE and KLEE. Consider
a program that stores a hash set of pointers where the pointers are obtained via distinct calls to malloc. Why
might this cause problems when:

This is an under-approximation as malloc may not always return a concrete value. In real-life this malloc call may fail.

a) Replaying a test input on the native program?


On the replay, malloc may return a different value for the pointer causing a different path to be followed. As a result of the different path, the buy may disappear giving a false-negative result. 

b) Proving that there are no errors along a particular path, given the symbolic inputs that were used?

Since malloc is an under-approximation, even if there are no errors on this under-approximated path, we cannot extrapolate this to include the whole feasible set of inputs for the program. 


Question 2. Consider the following contrived program. Assume that redundant path elimination and static
path merging are both disabled.
    int i;
    int main()
    {
      char buffer[1000];
      memset(buffer, ’-’, 1000);
      make_symbolic(&i, sizeof(i));
      if (i == 0)
        i++;
      return 0;
    }

After EXE terminates due to exploring all paths, approximately how many paths will have been explored?
Approximately how much memory will EXE use to store the buffer array for all states? Assume that EXE
will initially allocate approximately 1000 bytes for the buffer array and its metadata.

Only ~1000 bytes of memory will be used. The fork() optimization provides a copy-on-write where the same char[buffer] is used since neither state writes to the array after it has been allocated.

Question 3. What are the advantages and disadvantages of interleaving different search strategies?

An easy advantage is that using a different strategy can help if one of them gets trapped inside an infinite loop. The different strategy can be used to break out of it.

A disadvantage is the converse, where a specific bug may be well suited to be explored using a singular strategy, but compute and time is wasted through the interleaving only to yield worse results. 

Question 4. Give at least two sources of non-determinism that might exist in a program. Which of these
could be a problem for EXE (or similar tools)?

As described above, the use of malloc which must return concrete memory locations in its pointers. This is perhaps the case with a variety of different system calls such as time() etc. which are difficult to model.

Concurrency.

Reading and writing to and from external files. 


Question 5. Why do you think some constraint optimisations are done in EXE and KLEE and not in the
solver?

SAT solvers are inherently very slow and possibly the bottle-neck of the compute. Making optimisations in the SAT is very difficult and should be done before passing to the SAT. The SAT solver also deals with boolean and bitwise values, so this is very difficult (comparatively) to optimise for programmers compared to the high-level C code. 



Question 6. For each of the following search strategies, give a code example that contains an assertion failure
that is unlikely to be detected within a short time limit. Explain why this is the case.

a) Random Path Selection

  int main() {
      unsigned i; // i is symbolic
      unsigned count = 0;
      while (i < 1000) {
          count++;
          i++;
          assert(count < 1000);
      }

      for(int k = 0; k < LARGE_NUMBER; k++) {}

      return 0;
  }


b) Coverage-Optimised Search


  int main() {
      int i; // i is symbolic
      if (!i) {
          for(;;) {   // infinite loop
              if (i == 42) {
                  return 3; // unreachable, assume not optimized out
              }
          }
      } else {
          for (int c = 0; c < 10; ++c) {
              // a bunch of instructions, but no branches
          }
      }

      assert(false);

      return 0;
  }

c) Random State Selection (uniformly selecting a state at random)

  int main() {
        unsigned i; // i is symbolic
        unsigned count = 0;
        while (i < 1000) {
            count++;
            i++;
            assert(count < 1000);
        }

        for(int k = 0; k < LARGE_NUMBER; k++) {}

        return 0;
    }

Question 7. Consider whether redundant paths
can be eliminated just before the calls to foo and bar
in the following programs. Explain why in each case. Assume that foo and bar have no side-effects, behave
deterministically when given the same arguments, and branch depending on the values of their arguments.

    int main() {
    // Assume uninitialised memory is marked as symbolic
      int i, j, k;
      if (i) {
        printf("i is not 0\n");
      } else {
        printf("i is 0\n");
      }
      foo(j, k);
      bar(i, j, k);
      return 0;
    }
    // Program 2
    int main() {
    // Assume uninitialised memory is marked as symbolic
      int i, a, b, z;
      if (i) {
        a = 1;
        b = 3;
      } else {
        a = 2;
        b = 3;
      }
      foo(a, b);
      bar(b, z);
      return 0;
    }


  Not too complicated, refer to the solution for the full walk-through but the main concept is about the "liveness" of variables, i.e is it still relevant to the remainder of the computations. 

The only path that can be pruned is one of the paths in Program 2 for the call to bar. At this point, a and i are no longer used or relevant. b is still 3 at the time of calling foo or bar so there are no side effects. Both paths will reach the same bar call and outcome. 


Question 8. What are the advantages and disadvantages of statically merging4 paths?

This reduces the number of actual branches that the program takes exponentially. However, by combining these conditions together we are effectively delegating this work to the SMT solver to solve "longer" queries. Due to optimisations and how the SMT solver may be implemented, without the explicit branches, we may inadvertently reduce the test coverage.


Question 9. The following code example checks whether foo and bar are equivalent.
    int main() {
      // Assume i, j and k are symbolic
      int i, j, k;
      int result1 = foo(i, j, k);
      int result2 = bar(i, j, k);
      assert(result1 == result2);
      return 0;
    }
Think of two cases in which the SMT solver may not need to be called at the assert statement. Hint: for
one of the cases, recall that EXE and KLEE store symbolic values as expressions (trees).

 - If foo and bar are both concrete returns then there is no need to call the SMT solver.
 - Likewise, if the symbolic outputs are exactly equivalent, we know they are equal and no need to invoke the solver.

 Question 10. In the following code example, give the expressions (e.g. as a tree) that EXE would use for i
and j at the call to foo. Assume that expressions are simplified.
    int main() {
      // Assume uninitialised memory is marked as symbolic
      int i, j, k;
      if (i == 0) {
        j = (k + i) + 1;
        foo(i, j);
      }
    }

If we enter the condition to call foo, implicit is i == 0. Therefore the call can be simplified to:
  foo(0, k+1);

Question 11. Assume the current path condition (shown as a constraint set) is:
  {i < j, l > i + n, k > 0, l > 4, n > 0}
Which constraints must be submitted to the solver if we wish to query whether i = 20?

All EXCEPT k > 0 since it has no bearing on i. 


Question 13. Consider the following program:
  
  int main() {
    // Assume uninitialised memory is marked as symbolic
    int i = 0;
    int j;
    i = i / j;
    assert(i == 0);
    return 0;
  }

Show how the exe-cc compiler could instrument the program to check for dangerous operations. Ignore
any other instrumentation it must perform. Assume that you can call fail() to terminate execution when an
error occurs. There should be no assert statements.


  int main() {
    // Assume uninitialised memory is marked as symbolic
    int i = 0;
    int j;
    if(j == 0) fail(); // new
    i = i / j;
    if(i != 0) fail(); // new
    return 0;
  }

Question 14. In KLEE, if the program under test reads from a concrete file that exists in the actual file
system, the file is opened and the data in the file is read. The same applies for writing to a concrete file.
On page 215 of the KLEE paper, look at Figure 3 and footnote 5. When reading from a concrete file, why
can’t KLEE’s modelling code use the offset in the file descriptor stored by the KLEE process?
Why might a program that reads from and writes data to a concrete file cause problems? Can you think of
a way to address this?


Files are read using an offset value in the file-descriptor. A file descriptor stores an offset, which points to the next character that will be returned when the read function is called. Multiple reads from different states make this difficult to manage as the same offset is used. Writing is even more complex.

Could be fixed by making read only, or copying the file contents into each state's allocated memory.

Question 15. What are the main trade-offs between concolic and non-concolic (EGT-style) symbolic execution? Why is concolic execution considered a form of white-box fuzzing?



Concolic = Concrete + Symbolic.

Concolic execution workflow:

    Pick any concrete input
    Run program concretely
    Collect symbolic constraints for taken branches
    Negate one constraint 
    Solve for a new concrete input 
    Run again
    Repeat until coverage is good or budget exhausted

Concolic execution has the advantage that it can explore deep paths very quickly. The problem is the redundant execution of the early already explored paths of the program repeatedly which is wasted compute. non-concolic execution forks execution at every possible state and holds these all simultaneously in memory which is intensive. It is also computationally difficult to parse through states and select which one to explore next. 

Concolic execution is a form of fuzzing as there is a mutation of program input to generate new inputs for exploration. This mutation is guided by properties of the program that we learn from gathering path constraints on each run. This intelligent mutation and knowledge of the underlying program properties make it a white-box fuzzing approach.  




