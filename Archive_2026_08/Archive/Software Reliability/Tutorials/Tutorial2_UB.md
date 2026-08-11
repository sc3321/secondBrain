---
id: Tutorial2_UB
aliases: []
tags: []
---

Question 1. Explain language-specified behaviour, unspecified behaviour, implementation-defined behaviour,
undefined behaviour.

   Language-specific: This is the contract which specifies between programmer and language what execution behaviour to expect from programs written in this language.

   Unspecified-bahaviour: Unwritten and ambigious behaviour which arise from semantically equivalent or identical code that produces different results. These differences are due to the underlying nature of the code itself and variable evaluation order in the underlying programs.

   Implementation-defined: This is when the compilers have different behaviours as described by a third party. I.e for the target architecture what kind of machine code to produce.

   Undefined behaviour: Anything which does not fall into the above categories- code which violates the contract and cannot be explained.



Question 2. Avoiding UB can be really hard, especially when implementation-defined and unspecified
behavior enters the mix. How could the following snippets cause UB (for this question, do not assume a specific
implementation)?

    1. int i = 100000;

    2. int* p = malloc(sizeof(int));
    *p = 4;

    3. int i;
    printf("%d\n", i);
    
    - 1: int assumed signed integer, where INT_MAX = 2^15. This is because a 4 byte integer, which is 32 bits, can represent 2^32 values. 0 is taken as a positive number, therefore its 2^-16 up till 2^15
    - 2: There is no safety check on the malloc which may return NULL. Dereferencing NULL is undefined.

    - 3: The variable i is not initialised and is then asked to print. This could lead to "TRAP REPRESENTATION" 

Question 3. Consider the following C function:

    int foo(int * p) {
      if (!p) {
        printf("About to do something bad...\n");
      }
      return *p;
    }

Suppose that you compile this function using some C compiler, and then write a program that calls foo
with a null pointer. You find that the program crashes due to the null pointer deference, but you do not see
the message “About to do something bad...”. Explain whether this behaviour arises due to a compiler bug, or
whether it might be expected due to legitimate compiler optimisations.

  The compiler does not actually dereference p at the line:
      if(!p)
  This line just compares the POINTER value against NULL, no dereferencing happening yet. 
The lack of the error message is likely due to a compiler optimisation, as the compiler will likely see the final:
      return *p
Since dereferencing a NULL pointer is UB, the compiler rightly assumes that the programmer is following the terms of the contract and would not dereference something NULL. Since p is not modified inside the if statement, it may reasonably decide to optimise away the if statement since it has no bearing on the function return. The entire if removed, and only at the return is it clear that p is NULL hence the crash.  

Question 4. For the following fragments of C code, identify the undefined behavior, if any, and propose a
reason for why that behaviour is undefined.

    1. 
       int * p;
       unsigned x;
       if (p + x < p)
        ...
    
    2. 
       int arr[10];
       int * p = arr + 10;
       int * q = arr + 11;
    
    3. 
       int a = 10 / (1 << 31);
    
    4. unsigned int b = UINT_MAX + 10;

    1. 
      int p is unintialised and is a pointer to an int. Depending on the size of x, if it is large enough- the value of x + p may exceed the size of memory allocated for p which is undefined behaviour. Also, the check of < p, is only valid if value of x is so large that it overflows the size of a pointer and wraps around to an address "less than p". Pointer overflow is undefined behaviour. 

	2.
	the line:
			int * p  = arr + 10
	In C, pointer arithmetic is done in multiples of the *pointed to type* so in the case of arr, it is done in multiples of int. i.e since arr decays to type int *, then we have arr + 0 = address of arr[0] and arr + 5 = address of arr[5]

		int * p  point to arr[10]. arr is defined up to arr[9]. 

	C allows arr[10] because it is stricly 1 larger than the allocated size. This is done to allow idioms like:
		for (int *p = arr; p != arr + 10; ++p) {
		    ...
		}
	However things like arr[10] are NOT allowed to be DEREFERENCED. But that is not happening here so its okay. 
	
	Arr[11] in the case Q is simply undefined.

	3.
		Left shifting 1 by 32 bits is likely going to reduce 1 past INT_MIN (depending on the size of int for the hardware). Due to this overflow beyond the precision of int, we will get a division by 0 error.

	4.
		No UB as overflowing arithmetic is defined as well defined with wrap arounds in C.




Question 5. When I compile the following C program:
1
		#include <string.h>
		#include <stdlib.h>
		#include <stdio.h>
		void print_str(char * s) {
			static char * last = 0;
			if (s) {
				last = s;
			}
			printf("%s\n", last);
		}
		int main() {
			char * s = malloc(9);
			strcpy(s, "username");
			print_str(s);
			free(s);
			char * t = malloc(9);
			strcpy(t, "password");
			print_str(0);
			return 0;
		}
with a recent version of gcc and then execute it, I get this output:
username
password
At first sight this might seem curious: print_str is invoked with argument 0 after password is copied into
the buffer to which t points. When passed a null pointer, print_str should print whatever string is backed up
by the static variable last, representing what was printed previously. Thus we might have expected to see the
output:
username
username
Give the simplest possible explanation why the output I see is legitimate. Additionally, give a plausible
explanation as to why I actually see this output.

	Well, the simplest possible explanation is UB. Since we call free(s), the memory pointed to by s is now freed and last which is set to point to s, can now point to anything. This undefined behaviour could sometimes lead to printing password.
	
	The more likely explanation is that since we call free(s) and s is a buffer allocated with 9 bytes, and we immediately allocate t to a buffer of the same size, the compiler may indeed allocate the SAME 9 bytes to the buffer t. Now since last still points to that same location (the old s and the new t), when we print last it will print "password" because password is loaded into the t buffer.

Question 6. In the following C++ examples, identify the undefined behaviour, if any, and propose a reason
for why that behaviour is undefined:

	1. 
		extern int authenticated;
		bool isNotAuthenticated() {
			authenticated == 0;
		}
		int bar() {
			if (isNotAuthenticated())
			return -1;
			// yeah
			return 1;
		}
	
	2. // Extract top-level domain name from url
	void get_top_level(char * input, size_t length,
	char * result) {
		for (size_t index = 0; index < length; ++index) {
			if (input[index + 1] == ’.’) {
				index += 1;
				memcpy(result, input + index, length - index);
			}
		}
	}
	char input[] = {’w’, ’w’, ’w’, ’.’, ’i’, ’m’, ’p’, ’e’, ’r’, ’i’, ’a’, ’l’, ’.’, ’a’, ’c’,
	’.’, ’u’, ’k’};
	int length = 18;
	char result[1024];
	get_top_level(input, length, result);
		
	3. 
	std::vector<int> my_list = {0,1,2,3,4};
	for (auto &item: my_list) {
		if (item == 2) {
			my_list.insert(my_list.begin(), 42);
		}
	}
	
	4. volatile int barrier_reached = 0;
		// Thread 1:
		void t1() {
			// do hard work;
			barrier_reached = 1;
		}
		// Thread 2:
		void t2() {
			// Wait for t1 to finish
			while(!barrier_reached) {}
			// start hard work
		}
	
	5. 
	int a = 5;
	a = ++a + 2;
	
	6.int a = 3;
	a = a++ + 2;


Question 7. Consider the following code fragment:
	
	int * p;
	unsigned int y;
	...
	*p = 72/y;
	if (CONDITION) {
		abort();
	}
	
• Give two examples of code in CONDITION that result in the call to abort being optimised away. What is
the intuition for each condition?
• What is the elimination query that STACK generates for each example?

		If p == NULL
		If q == 0
		
		Before we get to the if condition evaluation, we have done *p = 72/y. We have dereferenced p which is only valid when p != NULL. The compiler assumes no UB written by the programmer so it will optimise away something like p == NULL.
		
		Similarly, doing *p = 72/y, we are dividing by y. Since div by 0 is undefined behaviour, the compiler can assume that y would never be 0. The condition y == 0 will therefore be optimised away.
		
		 
		
		
		
		

Question 8. Consider the following code fragment:

	int a[5] = {1, 2, 3, 4, 5};
	int x, y;
	...
	int q = x<<y;
	a[q] = 100;
	if (CONDITION)
		...
		
• Give two examples of code in CONDITION that result in the condition being simplified due to undefined
behaviour. What are the conditions simplified to? What is the intuition for each condition?
• What is the simplification query that STACK generates for each example?

		if(y < 0)
		if(q >= 5)

		Doing a left shift with y, y must be greater than 0, so checking if y < 0 should be optimised to always evaluate "false". Similarly, indexing an element out of bounds of an array is also UB so the compiler may optimise a condition like q >= 5 to be false.

Question 9. For compiler developers, not handling undefined behaviour allows them to optimise code.
Analyse the following code examples. Explain which undefined behaviour could be triggered by which input?
Assuming this input is never provided, which optimisations could be enabled?
		
		1. int i = ... ; // initialized
		int y = i * 5 / 5;
		
		2. for (int i = 0; i <= N; ++i) {
			...
		}

	1. Provided that "i" is not too large (less than INT_MAX / 5) which would lead to signed integer overflow, we can optimise assignment to y = i.
	2. If N = INT_MAX, at the penultimate iteration, we get i = N = INT_MAX, so when the loop does the final increment before doing the termination check, i becomes i++ which is signed integer overflow.
	   Without this input, we can do all the regular induction variable elimination. This is basically optimisations based on assumptions of monotonically increasing variables so things like variables can be simplified, array indexes organised more optimally etc.



