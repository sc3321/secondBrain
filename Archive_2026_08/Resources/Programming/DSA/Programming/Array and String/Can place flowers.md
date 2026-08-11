![Exported image](Exported%20image%2020250528103541-0.png)

2 ways to go about doing this which rely on spotting a key pattern in the problem statement. The main difference between the two ways is that one of them is slightly more memory efficient as it does NOT involve making a copy of the original vector flowerbed. Here are the two approaches:
 ![Exported image](Exported%20image%2020250528103541-1.png)  

This is the method which requires making the copy of the original vector. The main thing to spot in this question is the fact that in order to plant a flower we NEED **3** contiguous empty spaces. The edge cases arise at the ends of the flowerbed at positions flowerbed[0] and flowerbed[size-1]. This is because we cannot check an out of bounds element to see if it is 0 even though we know it is in practice. To get around this, we make a copy of the vector but add 2 0 elements. One at the start of the vector before flowerbed[0] and one at the end of flowerbed[size-1]. We then do the intuitive for loop.

![Exported image](Exported%20image%2020250528103542-2.png)

Very similar except we are using the empty variable to keep track of contiguous empty spaces. At the beginning, if the first element of the vector flowerbed is 0, we initialise empty to 1. This way when we iterate through the flowerbed vector beginning at element 0, we continue to increment empty for each 0 we encounter. The moment we encounter a 1, we can check how many empty spaces preceeded it. Using int rounding, n is only decremented when empty is multiples of 3. ￼￼The first element check to see if it is 0 is important in making sure we are looking for that 3 contiguous block.
 
At the end of the iteration we also have an idea of how many empty spaces we had at the end of the vector. This time we are effectively seeing if empty is 2 because we already know the first out of bounds elements is a 0. So if empty is 2, we can also decrement n.