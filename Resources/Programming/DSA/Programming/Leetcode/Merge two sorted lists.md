![Exported image](Exported%20image%2020250528103635-0.png)   
The first line dummy = ListNode() is a technique used to avoid the edge case of an empty list. I accounted for this edge case but my code was a lot more clunky.
 
The while loop is pretty self explanatory and my solution included it.
 
The final if statement is useful. Instead of iterating through the remainder of list1 OR list2, if we simply take the first remainding element we know by default the other other list is empty and the current list is sorted. This way, just adding the first element is sufficient.