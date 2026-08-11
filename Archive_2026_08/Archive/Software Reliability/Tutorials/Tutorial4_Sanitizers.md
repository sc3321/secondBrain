---
id: Tutorial4_Sanitizers
aliases: []
tags: []
---

Question 1. When I compile the following C program:

    1 #include<stdlib.h>
    2 #include<stdio.h>
    3
    4 void foo(int n) {
    5   unsigned long arr[2];
    6
    7   while (n > 0)
    8       arr[n--] = 0x0040056b;
    9   }
    10
    11 int main(int argc, char ** argv) {
    12  printf("Main\n");
    13  foo(atoi(argv[1]));
    14  return 0;
    15 }


a) Provide a reasonable explanation for the program printing Main only once and then crashing in 2.

  
b) Provide a reasonable explanation for the program printing Main twice and then crashing in 3.

c) Suppose that this program contains function formatAllDrives, which does something potentially dangerous
and permanent. Change this program minimally to call function formatAllDrives for a large enough n.


This is all to do with the stack which looks something like:

    +--------------------+  higher addresses
    |  caller stuff ...  |
    +--------------------+
    |  return address    |  <- where to jump back in main
    +--------------------+
    |  saved registers   |  (maybe)
    +--------------------+
    |  arr[1]            |
    +--------------------+
    |  arr[0]            |
    +--------------------+  lower addresses

C does NOT provide any safety checks to see which parts of the stack are being accessed and if this is actually allowed.
arr is an unsigned long array so each element is 8 bytes === 64 bits which is one frame on the stack.

a:
  n is provided large enough to where we have overwritten the return address with this value 0x0040056b. When the program finishes, we use this new value as the location to jump back to. If this address is a defined valid executable address we will get a segfault here.

b:
  In this situation, it just so happens that when we overwrite the return address with a large n, then this 0x0040056b corresponds to the address of executable instruction printf("Main"). The locations surrounding printf have also been overwritten with this bogus x0040056b so the program will segfault at the foo(....) instruction.

c:
  Using something like gdb and breakpoints, we can work out the address of the function formatAllDrives. We can use this information, and potentially other stuff to hardcode this address in our arr[--n] instead of the 0x0040056b.


Question 2. Consider the following list of dynamic-analysis tools:

  • ASan • MSan • TSan • UBSan

And the following list of possible bugs:
  
  • Heap buffer overflow
  • Stack buffer overflow
  • Global buffer overflow
  • Heap use after free
  • Use after return
  • Uninitialised memory read
  • Data race
  • Other undefined behaviour
  • Memory leak

For each of the following code snippets, there is at least one sanitizer from the first list which will report a
bug from the second list. Indicate which these are.

  a) TSan - Data race
  b) ASan heap use after free
  c) ASan - Stack yse after return * CHECK THIS *
  d) UBSan - signed integer overflow.
  e) UBSan- Array out bounds access.
  f) ASan - heap buffer overflow.
  g) Lsan - memory leak.
  h) UBSan - signed integer overflow.
  i) UBSan - array out of bounds access.
  j) MSan - use of Uninitialised value



Question 3. Consider the code below. Under what circumstances will ASan be able to spot the bug contained
within?
    int main(int argc, char **argv) {
      int *array1 = new int[100];
      int *array2 = new int[100];
      int a = array1[argc];
      delete [] array1;
      delete [] array2;
      return a;
    }

A-San is a dynamic analysis tool that is sensitive to program input(s). Combining A-San with something like fuzzing to get large values of argc could initiate an out-of-bounds array access. If the value of argc is really large however, we could surpass the red-zone for safe memory acceses.


Question 4. 

Valgrind works on binaries so it cannot instrument things like red-zones for safe memory access for example. A-San works on source code so it can provide this instrumentation and see the out-of-bounds access. 

Question 5.

    void foo() {
      char redzone1[8]; // 8-byte aligned
      char a[2]; // 8-byte aligned, with 6 bytes missing
      char redzone2[6]; // add the extra 6 bytes
      char redzone3[8]; // 8-byte aligned
      char *shadow_base = MemToShadow(redzone1);
      shadow_base[0] = -1; // poison redzone1
      shadow_base[1] = 2; // poison redzone2, unpoison ’a’
      shadow_base[2] = -1; // poison redzone3
      ...
      shadow_base[0] = shadow_base[1] = shadow_base[2] = 0; // unpoison all
      return;
    }

shadow_base[i] = k ===> This means we allocate the first k bytes to be addressable memory and the remaining 8-k bytes to be negative and unaddressable.

Question 6.
  
  The different negative values correspond to different accesses into different poisoned memory. For example a -1 might be a HEAP_ACCESS or -2 to be a STACK_ACCESS.


Question 7.
  
  The compiler has likely optimised away the array b since it is not accessed in the function.

Question 8.
  
  
Question 9.

  Review shadow_memory and how it is allocated. Shadow memory occupies 1/ 2^scale of the virtual address space.

Question 10.

  Assume we statically allocate a chunk of memory which we will manually manage. In the
  following program, there are two invalid memory accesses. Identify which one will be found by ASan and which
  one will not. How could we alter the program to inform ASan of our manual memory management?

    int mem[200] = {0};
    int offset = 0;
    int *custom_alloc(int size) {
      int *newptr = mem + offset;
      offset += size;
      if (offset > 200)
        return 0;
      return newptr;
    }
    int main(void) {
      int *p = custom_alloc(100);
      int *q = custom_alloc(100);
      p[100] = 1;
      q[100] = 1;
      return 0;
    } 

  This is basically a pseudo heap implementation. mem[200] represents the heap memory available. *p points to the 0th element of mem[] and *q points to the 100th element of mem[]. Custom alloc is intended to allocate 100 sizeof(int) spaces for array p and q respectively.

The line p[100] = 1, is technically an out of bounds access since p is only defined p[0...99] but A-San will not pick this up as an out of bounds access since it p is referenced against the mem[] array so since mem is defined up to [200] there is no redzone access. 

for q[100] however we would be accessing mem[200] which doesnt exist hence an out of bounds would be thrown.

Question 11.

  MSan only triggers an error when the Uninitialised value affects the control flow of the program and affect the visible behaviour. 

Question 12.

Question 12. Assuming a 4-byte alignment, find the line where MSan will detect the use of uninitialised
memory. Explain why.
    1 #include <stdio.h>
    2 #include <stdlib.h>
    3
    4 typedef struct {
    5 char c;
    6 int a;
    7 } mystruct;
    8
    9 int main() {
    10 printf("char size: %ld\n", sizeof(char)); // 1
    11 printf("int size: %ld\n", sizeof(int)); // 4
    12
    13 mystruct *s1 = malloc(sizeof(mystruct));
    14 s1->c = ’a’;
    15 s1->a = 1;
    16
    17 mystruct *s2 = malloc(sizeof(mystruct));
    18 *s2 = *s1;
    19
    20 char *pb = (char*) s2;
    21 if (pb[2])
    22  printf("b\n");
    23 else printf("not b\n");
    24
    25 return 0;
    26 }

Assuming 8 byte alligned memory the object s1 would be stored in memory as:
0st byte - 'a'
1...3 byte are all padded with unitialised values.
4..7 are used to hold the int 1.

Since s2 is copied with the values of s1(line 18), we are technically copying some unitialised memory but this does not affect the program flow so MSan will not report it.
On line 21 however, we are using the second byte after the location of s2 which is a byte that contains padded data. This being unitialised and affecting the control flow of the program causes MSan to be triggered.

Question 13.

  Valgrind disassembles binaries and rewrites programs into its own intermediate representation. By doing this it can simulate every single instruction and monitor every memory access- including externally linked libraries! This process is intense and places a huge runtime overhead on analysis compared to MSan and ASan. MSan and Asan must be applied at compile time with the compiler compared to Valgrind which works on binaries, agnostic to platform and compiler. MSan and Asan cannot be combined. In particular, MSan works poorly when it is combined with uninstrumented code. This will be revealed in question 14. 
Tldr/ MSan and ASan require recompilation to be properly instrumented to detect memory or access faults.


Question 14.

  Consider:
  
  foo.h:
        
        int* foo(int *);
  foo.c:
      
        void foo(int* x){
          *x = 4;
        }

  main.c:
        
        #include foo.h 
        int main(){
          int x;
          foo(&x);
          if(x > 2){
            return 1;
          }
          return 0;
        }

If for example, foo.c is NOT compiled with MSan, then it is NOT instrumented. 
If main.c IS COMPILED with Msan, then it will not see the fact that int x gets initialised in foo. Since it is used for control flow in if(x > 2), it sees this as use of unitialised memory and will throw an error.
This is a FALSE POSITIVE, since there actually isnt a bug as foo initialises x, but since it has not been compiled with Msan that section of code is uninstrumented and main.c is non-the-wiser.




      
            































































































































































