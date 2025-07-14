![450 Delete Node in a BST Topics Companies Medium S...](Exported%20image%2020250528103604-0.png)

![13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29...](Exported%20image%2020250528103605-1.png)

One of the most tricky and less-intuitive problems so far. The basic premise of the solution is as follows:
 
1. Traverse the tree until the key value is found. This is the first few if statements where we move to the left or right child based on the key value and the node value.
2. Now if a node with the key is actually found. There are 4 scenarios that must be accounted for. Scenarios 1-3 are fairly straightforward. If the key node is a leaf then just NULL needs to be returned as we effectively remove the node in question. If either the left or right subtree is NULL we can just return the right or left subtree respectively. 
Remember that this is a recursive function so during step 1 we assign the right or left subtree to whatever the deleteNode function call returns for the right or left subtree respectively.

5. The tricky step to spot is if the key node has 2 children. In this scenario what we do is change the key node value to one of the 2 options: The LARGEST value in the left subtree or the SMALLEST value in the right subtree. The intuition here is that the new returned tree has to still be a BST. Therefore the smallest value in right subtree when it takes the place of the key node value will still follow the BST rule of everything in the right subtree being greater than it and everything in the left subtree being smaller than it. Alternatively, the largest value in the left subtree will also follow the same rules.
   

Once this smallest value in the right subtree(RST) is found and has replaced the keyNode value, the root-\>right will have to point to whatever is returned by deleteNode BUT this time the key is that smallest value in the RST. This is to remove this smallest value in the tree.

2. Finally the original root is returned after all the recursive calls are completed.