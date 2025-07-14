![struct TreeNode p struct TreeNode q 10 11 12 13 14...](Exported%20image%2020250528103606-0.png) ![236 Lowest Common Ancestor of a Binary Tree Topics...](Exported%20image%2020250528103606-1.png)

This is an interesting problem that requires some lateral thinking. Not much any new knowledge on DSA but an unique approach to solving the problem. The premise is as follows:
 
If either the p or the q value is equal to the current node value then that node must either be the lowest common ancestor. This is the first call made on the root of the whole tree.￼After that we recursively call the function on the left child and the right child.  
The main premise is that if the current node is equal to the value of p or q then it MUST be the lowest common ancestor and is returned as the root. This is the node that is returned upwards on the call stack to the first function call of left or right.  
If left exists but right does not, then left must be lowest common ancestor and vice versa. The reason we can make this assumption is because there is ALWAYS a LCA in the context of this question.