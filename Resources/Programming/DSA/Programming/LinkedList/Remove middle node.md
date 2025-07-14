![2095 Delete the Middle Node of a Linked List O Him...](Exported%20image%2020250528103652-0.png)

![11 12 class Solution pub L it head if head return ...](Exported%20image%2020250528103653-1.png)

The main thing to understand here is the two pointer approach to solving the problem. The fast pointer moving at twice the speed means that by the time it reaches the end, by definition the slow pointer will have traversed half the length of the list and be at the middlenode.￼The back pointer initially pointing to NULL will be the node just before the middle node which is what we want because we can assign back-\>next = (middlenode)-\>next. Then proceed to delete the middlenode.