![1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20...](Exported%20image%2020250528103534-0.png) ![334 Increasing Triplet Subsequence Topics Companie...](Exported%20image%2020250528103535-1.png)

Really silly from me. Actually a very simple problem that can be solved with if statements. The main complexity lies in being able to handle the subsequence condition.
 
The premise is as follows- setting the first and second number to max.  
If we encounter a number which is smaller than the first then we assign first to the number.  
The first else-if statement is encountered if a number is bigger than the first and also smaller than or equal to the second and if it is, then assign it to the second.  
The final else condition is therefore only implicitly triggered if the first and second conditions are NOT met therefore the number nums[i] is bigger than the first and bigger than the second. Since we are iterating through the array incrementally, the triplet MUST therefore exist.
 
The confusion then arises if say in an example like [2,3,1,4,5]  
When the number 1 is encountered, in position 2 of the array, what does this do for the "first" and "second" values. The answer is: IT DOES NOT MATTER. When the number 1 is encountered, 2 was "first" and 3 was "second". At the number 1, the "first" variable is updated to 1 and that’s that. It does not change the fact that 3 was bigger than 2 so by definition it is also bigger than 1. When 4 is encountered therefore, 4 is bigger than 3 in "second" and and 1 in "first" and the else condition would trigger returning true. The SAME is also true if "first" was 2 as 4 is bigger than 2 and bigger than 3.