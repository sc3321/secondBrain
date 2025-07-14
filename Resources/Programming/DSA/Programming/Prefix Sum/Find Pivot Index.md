![724 Find Pivot Index Q Hint Easy O Topics Companie...](Exported%20image%2020250528103658-0.png)

![3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 ...](Exported%20image%2020250528103702-1.png)

The trick here was to spot the two for loops. The first one just increased the rightsum. In the second for loop, on each iteration subtracting nums[i] from the rightsum would return the righsum for everything to the right of that index. Then being able to check if it was equal to the the leftsum would tell us if the leftsum == rightsum. The reason why it doesn’t take into account the index itself, is because the leftsum is only incremented after this comparison.