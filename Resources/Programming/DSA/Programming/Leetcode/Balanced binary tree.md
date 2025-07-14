![Exported image](Exported%20image%2020250528103638-0.png)

Height balanced means that for any given node, the left and right subtrees cannot differ in depth by more than 1.
 
Walking through briefly why it works:
 
Declaring a sub-function called dfs- depth first search, returns two parametres. A boolean and a height value. The code uses a "bottom-up" approach. This is the effect of the line where:  
Left, right = dfs(root.left), dfs(root.right).  
The bottom most nodes are traversed to first. The base case being the nullptr.
 
It will keep plunging the tree until it gets as far left as it possibly can. Until it reaches a root-\>left which points to null. At this point- the base case, it will return True and 0 to the node just above it. That node will then perform dfs(root-\>right).
 
The moment that at any point the balanced condition becomes false, the function will return [false, 1 + …]. It will keep going doing its calculation for every node up to the root node but the false value will never change. This is what is eventually returned.

![Exported image](Exported%20image%2020250528103638-1.png)