![Exported image](Exported%20image%2020250528103639-0.png)

Can be solved using HashMap's or using something called "Floyd's tortoise and hare algorithm".
 
Algorithm is as follows:
  - Use two pointers. One fast pointer(hare) and one slow pointer(tortoise).
- They are both initialized to head which is the starting pointer to the top of the list.
- Fast is incremented by 2: fast = head-\>next-\>next. While slow is incremented by 1.
- If at any point again, fast and slow are equal, i.e they are pointing to the same node, we can be certain that there was a cycle.
    
    - Just think about it logically, if fast is being incremented by 2, it should never equal slow again so if it does we know that there must have been a cycle in the list.
- If at any point, fast or fast-\>next is null, we can be sure that the list is NOT cyclical. Important to make the second check of if fast-\>next not being null so we do not run into segmentation issues.    

Here is the python solution:
 ![Exported image](Exported%20image%2020250528103643-1.png)