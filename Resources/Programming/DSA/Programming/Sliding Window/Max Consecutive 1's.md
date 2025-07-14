![1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20...](Exported%20image%2020250528103709-0.png) ![1004 Max Consecutive Ones Ill Topics Companies Q H...](Exported%20image%2020250528103709-1.png)

This is an interesting problem which requires the use again of a sliding window and 2 pointer approach. This seems to be a common pattern that should be remembered going forward. The premise is as follows:
 
Increment the right pointer till the end of the vector.
 
If we encounter a 0, increment the flipped variable.
 
Check if the flipped amount is greater than k. If it is, then we need to modify the window size..  
We will be incrementing the left pointer in a while loop with a check at each incrementation if flipped \> k. If the left pointer == 0, then we can decrement the flipped variable. This continues until we break out the while loop. This step will end with flipped = k.
 
The final window size is calculated and if larger than the max then max updated.

Basically keep increasing the window size till we max out the number of flips that we can make. Then, move the left of the window until we reach a 0 to essentially unflip that flipped 0. Then keep repeating at each increment of the right pointer for every new 0 encountered. This method ensures we are moving the left of the window by the smallest amount possible at each iteration.