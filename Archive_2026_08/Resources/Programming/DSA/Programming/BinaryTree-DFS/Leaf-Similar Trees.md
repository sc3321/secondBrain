![Consider all the leaves of a binary tree from left...](Exported%20image%2020250528103554-0.png)

![14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30...](Exported%20image%2020250528103555-1.png)

A few key points to know:￼

1. It is always okay to write my own function to go inside another function.
2. DFS is usually easier to implement than BFS. The basic DFS algorithm addresses the base cases, then the algorithm is called recursively on the left subtree and right subtree.
3. In this case, there is a vector passed as argument to the function. If this vector is to be updated in each function call dynamically, then it must be passed by reference using the &.