![1372 Longest ZigZag Path in a Binary Tree Topics C...](Exported%20image%2020250528103552-0.png)   ![Example 1 Input root Output 3 Explanation Right Le...](Exported%20image%2020250528103552-1.png)

![12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28...](Exported%20image%2020250528103553-2.png)

Pretty elegant solution that I mostly derived. The one part which I missed out on was not accounting for the fact that the root of the zigzag could technically start from anywhere in the tree. The way to account for this was to include the  
std::max(findzigzag(root-\>right, max+1, true),findzigzag (root-\>left, 0, false))  
This line allows the possibility of IF I have already taken a left, I can either follow this zig zag and go right next and continue incrementing my current depth OR I can take another left and start a new zig zag from my current node.￼