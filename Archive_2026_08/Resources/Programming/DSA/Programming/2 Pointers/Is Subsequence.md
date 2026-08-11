![Given two strings s and t return Itrue if sus a su...](Exported%20image%2020250528103523-0.png)

![c v a Auto 1 class Solution 2 public if s left tri...](Exported%20image%2020250528103524-1.png)

2 pointer approach. If the first pointer is out of bounds by the end of iteration, it must have been found. Second pointer is always incremented, no final check on its position, just kept inside bounds to avoid seg faults.
 
This method useful when only one string needs to be checked in "order".