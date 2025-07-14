![1679 Max Number of KSum Pairs Mm are given an inte...](Exported%20image%2020250528103526-0.png)

![class Sotutlon public int int k unordered_nagxlnt ...](Exported%20image%2020250528103527-1.png)

This is a misleading problem to put under 2 pointers. The main issue with it is that for the 2 pointers approach to work, it relies on the array being sorted. This operation is an nlogn operation from the standard C library of std::sort. The approach that I have used is rather the unordered map approach. The principle is easy to grasp:
 
Initialise the map.
 
Iterate through the nums array. At each iteration, check if the k - nums[i] key exists in the map.
 
If the k-nums[i] key does exist then the complement to nums[i] is in the array and a pair is found. In which case decrement the k - nums[i] definition by 1.
 
If it does not exist, increment the nums[i] definition by 1 or conversely create a new key nums[i] if it does not already exist.
 
Weird terminology for hash maps but for my own understanding, the key and the definition. Think of a dictionary. Left is the key. Right is the definition.