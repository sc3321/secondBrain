![Exported image](Exported%20image%2020250528103634-0.png)

There may be a recursive solution but the optimal way to solve this problem is using pointers. Recursion works best when the parametres of the function are modified slightly. The recommended approach:
   

Set a left pointer to the start of the vector and a right pointer to the end of the vector.
 
Enter a while loop where the only condition that we are keeping track of is the right pointer having a value greater than or equal to the left pointer.
 
We calculate the midpoint as ((l + r) / 2)  
see if the value at the midpoint index is greater than the target. If it is, then we move the right pointer to be midpoint - 1. likewise if the value at the midpoint index is smaller than the target we set the left pointer to be midpoint + 1. The only other possible scenario is the value at the midpoint index = target in which case we return midpoint.
 
If we break out of the while loop we can be sure the target was not found so we return -1.