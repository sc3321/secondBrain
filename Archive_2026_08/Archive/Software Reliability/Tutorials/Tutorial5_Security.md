---
id: Tutorial5_Security
aliases: []
tags: []
---

Question 1:
  
  - Answered in the sanitizers tutorial sheet.

Question 2:

    Modify code in Question 1 to perform Control-Flow Integrity (CFI) checking. Use pseudocode,
    referring to the return address as RA.
    
    1. For each of the three runs from Question 1 explain if and why would your modification report an error.
    2. What change to the program would render CFI useless in the third run?

    1 #include<stdlib.h>
    2 #include<stdio.h>
    3
    4 void foo(int n) {
    5   unsigned long arr[2];
    6
    7   while (n > 0)
    8       arr[n--] = 0x0040056b;
        
        assert(*RA == NOP && CIF_ERROR());
    9  }
    10
    11 int main(int argc, char ** argv) {
    12      printf("Main\n");
    13      foo(atoi(argv[1]));
        L1: NOP;
    14      return 0;
    15 }

    
    Well quite simply removing the call to foo and inlining that code into the function itself. This removes all control flow issues.

Also if there was an additional inclusion foo() before the printf, then there would be no way for CFI to be able to detect which return address is valid. In the third run, if the arr[] modifies the stack return address to be the printf function, then this would not be able to be detected. 


Question 3.

    1 char buf_data[128];
    2 char buf[128];
    3 char buf_init[128] = {’p’,’n’,’g’};
    4
    5 int validate_picture(char *read_data, int length) {
    6   int i;
    7   // simple memcpy()
    8   for (i = 0; i < 128; ++i) {
    9       buf[i] = buf_init[i];
    10      // buf_data[i] = buf_init[i];
    11  }
    12
    13  for (i = 0; i < length; ++i) {
    14      buf_data[i] = read_data[i];
    15  }
    16
    17  for (i = 0; i < 3; ++i) {
    18      if (buf[i] != buf_data[i])
    19          return -1;
    20  }
    21
    22  // ...
    23
    24  return 0;
    25 }

1. What are potential issues with this function?

  Since buf[] and buf_data[] are adjacent objects in memory, it is very possible(likely) that in the loop on line 13-15, buf_data could write into buf.

2. Consider applying Write Integrity Testing (WIT) to this code. How many different colours will be used
to group the objects and memory writes?
  
  3 different colours for the 3 distinct points-to sets:
  W1: {int i;} : red.
  W2: {buf = } : blue
  W3: {buf_data = } : green.
  
  Each write is to different memory distinct memory objects. The sets are formed by grouping a memory object with all the writes that legally can write into it and giving that a colour. If we find that any sets overlap, WIT cannot distinguish them. 

3. Give an example of an unsafe operation which WIT checks during runtime.

  Writes to buf and buf_data are out of bounds due to the index.

4. Extend the code from above with pseudo-code that mimics the behaviour of WIT. Ignore the writes to
variable i.

  

5. How does the points-to set change if line 10 is added? What is the implication?

Question 4. Using the same code from the previous question, answer the following questions:


1. How does data-flow integrity (DFI) help to find the issue?

  Data flow integrity assigns every valid write to a memory location with a number all mapped in shadow memory. When reading, there is a check to see if the shadow memory location is valid for that read based on that most recent valid write, if it is not, then an error is thrown.  

2. Augment the code with DFI checks.

    1 char buf_data[128];
    2 char buf[128];
    3 char buf_init[128] = {’p’,’n’,’g’};
    4
    5 int validate_picture(char *read_data, int length) {
    6   int i;
    7   // simple memcpy()
    8   for (i = 0; i < 128; ++i) {
    9       store[&buf + i] = LOC_1;
    10      buf[i] = buf_init[i];
    11      // store[&buf_data + i] = LOC_2;
    12      // buf_data[i] = buf_init[i];
    13  }
    14
    15  for (i = 0; i < length; ++i) {
    16      store[&buf_data + i] = LOC_3;
    17      buf_data[i] = read_data[i];
    18  }
    19
    20  for (i = 0; i < 3; ++i) {
    21      if (store[&buf + i] != LOC_1) error(); // KABOOM
    22      if (store[&buf_data + i] != LOC_3) error();
    23      if (buf[i] != buf_data[i])
    24      return -1;
    25  }
    26
    27  // ...
    28
    29  return 0;
    30 }


3. If line 10 is added, does DFI still find the issue?

  - [ ] It will because DFI works by using the most recent valid write, so the write on line 16 to buf_data will overwrite the write in shadow memory of "LOC_2" to buf_data made on line 11. As a result, the check on line 22 will be able to catch anything nefarious which may have potentially happened. 
