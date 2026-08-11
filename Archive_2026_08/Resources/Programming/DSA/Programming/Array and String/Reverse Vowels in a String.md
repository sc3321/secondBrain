![345 Reverse Vowels of a String Easy Topics Compani...](Exported%20image%2020250528103532-0.png)

|   |   |
|---|---|
|Auxillary function:￼|![Code c v a Auto 1 class Solution 2 public a o 3 4 ...](Exported%20image%2020250528103532-1.png)|

![12 13 14 15 16 17 18 19 2 21 22 23 24 25 26 27 28 ...](Exported%20image%2020250528103533-2.png)

The key premise of this solution is the use of 2 pointers. A left pointer at the start of the string and a right pointer at the end of the string. The thing to spot here is that basically the left and right pointers ONLY get updated if they are NOT a vowel. While l \< r, if there is any case where say the left pointer is pointing to a vowel but the right pointer is NOT. The left pointer will continue to remain in the same position and will not be incremented. While l \< r, r will keep getting decremented until it to points to a vowel or points to the left pointer if there are no other vowels in the string.￼The left pointer not being incremented means that the first if statement in the while loop will always return true. The moment the right pointer returns true in the second if statement is when the swap will occur. If there are no other vowels then no swap occurs.