![2390 Removing Stars From a String Q Hint Topics Co...](Exported%20image%2020250528103717-0.png)

![4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22...](Exported%20image%2020250528103717-1.png)

Stack operations for the first time. Stacks offer a L-I-F-O approach, meaning, last in first out. Think of like a stack of plates. The last plate to be put on the stack is the first plate that is to be removed.
 
The thinking is that since we are traversing right to left, we keep adding chars to the top of the stack. The moment we encounter a '*' character, we can just remove the most recent item from the top of the stack. The final stack is in reverse order so it must be reversed to get the in-order string.
 
Stacks use an O(n) space complexity. The way to avoid even that is by using a 2 pointer approach:￼

![class Solution public string removestarsstring s i...](Exported%20image%2020250528103718-2.png)

The basic premise here is that we keep incrementing the left and right pointer. The moment the right pointer encounters a '*' character, we decrement the left pointer by 1. Then as we encounter more non '*' characters we overwrite portions of the remaining string. We can then return just the substring of s(0,j) which includes just the necessary portion.