![Exported image](Exported%20image%2020250528103636-0.png)

This problem must be solved recursively.
 
EACH node must be visited and its daughter nodes reversed. This implies the recursive solution as each node must be traversed.
 
The final C++ code is as follows:
 ![Exported image](Exported%20image%2020250528103637-1.png)  

The base case is critical. Once a base case is encountered, the nullptr is returned to the stack and invertTree(root-\>right) is called.