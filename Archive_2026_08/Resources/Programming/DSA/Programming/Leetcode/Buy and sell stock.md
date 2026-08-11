![Exported image](Exported%20image%2020250528103629-0.png)   
Sliding window algorithm:
 
Using 2 pointers left and right.
 
We initially set the right pointer to be one ahead of the left pointer.
 
We then perform right - left.  
If this result is negative we can tell that the right value must be lower than the left value so we immediately set the left pointer to the right pointer and the right pointer to be one more than the left pointer.
 
If the result is positive, we know that the right number is larger than the left number. We then store this result as the max profit..   We then continue to increment the right pointer, noting the subsequent profits and updating the max result if necessary.
 
The moment we encounter a negative profit is when we reassign the left pointer and restart the process while still keeping track of the max profit.