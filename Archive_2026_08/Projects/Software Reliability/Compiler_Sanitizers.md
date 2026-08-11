---
id: Compiler_Sanitizers
aliases: []
tags: []
---

# C/C++ Dominance
- performance
- legacy
- Memory control.



# False positive vs Negative

- False positives could be worse as it means that code will be stopped from running on first false positive.
  This could be a nightmare as it stops the whole program from running. 
- For false positives, there is still interception of uninstrumented code which allocates to the heap.
- 



struct {
  
  char c;   
  int i;

}x, y


This struct is initialised with 8 bytes in memory NOT 5. This is because compilers like to make ints for example 4 byte aligned to improve performance (cache line performance...)

y = x;

This is therefore not an error as it is okay to copy and assign those padded bytes.


