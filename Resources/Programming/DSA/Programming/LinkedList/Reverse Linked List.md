![206 Reverse Linked List Topics Companies Easy Give...](Exported%20image%2020250528103647-0.png)

2 main approaches: Iterative and Recursive.
 
Iterative:￼￼

![class Solution public ListNode reverseListListNode...](Exported%20image%2020250528103647-1.png)

2 key things to remember here conceptually:￼Firstly when reversing a list, we can only start at the first Node obviously. The idea of each node pointing to the one before it makes sense but it can be tricky to spot that the first node has nothing before it. In this situation, the PREVIOUS node in the very first iteration should be NULL. This is valid.
 
The second thing to be aware of is making copies of nodes so that they can be reassigned correctly. Consider the ListNode* tmp in the while loop. ￼￼When using pointers, the memory efficiency is O(1).

Recursive:￼
   
![class Solution public ListN0de reverseList ListN0d...](Exported%20image%2020250528103648-2.png)  

The most confusing part about this approach is the head-\>next-\>next line. Line by line explanation is the following::￼  
Checking the edge case where the list is empty to begin with.
 
Initialising a pointer which points to the node passed into the function argument. We check immediately if that node has a "next". If it doesn’t then we can say that it is the final node in the linked list.
 
*RECURSIVE PART*  
The newhead will point to whatever is now returned by the reverseList recursive call. This recursive call continues UNTIL head-\> next == NULL. At this point that node returned is the final node. At the penultimate function call therefore, newhead points to the final node.
 
The confusing part is head-\>next-\>next. The penultimate node-\>next = final node = newhead.  
Newhead-\>next = head-\>next-\>next. Initially it will be NULL. The key is spotting that we are going to assign this back to head.
 
At each function call working up the stack we assign the returned value back to the head until finally we reach the first Node and set its next to NULL.
 
Quite confusing!!