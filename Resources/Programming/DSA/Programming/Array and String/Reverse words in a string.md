![Exported image](Exported%20image%2020250528103535-0.png)

![Exported image](Exported%20image%2020250528103536-1.png)

The intuition of the problem is to use 2 pointers:  
Technically 3 but one of them is an iterator.￼

1. Reverse the string as a whole.
2. Clear any leading spaces in the new reversed string.
3. If a non-empty space is found, keep incrementing a "right" pointer until we encounter the end of a word- i.e when a space is encountered.
4. At this point, reverse the reversed string to return back to the original.
5. Add a trailing space to the string.
6. Set the left pointer to the right pointer to mark the start of finding a new word.
7. Repeat until the end of the string.
8. Remove the final trailing space at the end of the final word.  
Steps 2 and 3 can be best illustrated by the following picture:￼￼

![Exported image](Exported%20image%2020250528103539-2.png)