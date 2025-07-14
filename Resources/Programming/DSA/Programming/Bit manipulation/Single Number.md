![[Pasted image 20250529113108.png]]

Ingenious solution which leverages the power of XOR gates. Consider the array to be checked is of the form:

[A, B, C, B, C, A, Z, D, D] where Z is obviously the unique element. 

Consider the folowing solution:

```
class Solution { 
public: int singleNumber(vector<int>& nums) { 
	int ans=0; 
	for(auto x:nums) 
	ans^=x; 
	return ans; } 
};
```

Using the commutivaty and associativity of the bitwise XOR operation: Anything XOR with itself = 0. Also anything XOR with 0 gives itself. i.e A ^ 0 = A and A ^ A = 0. Over the entire XOR chain across the whole array only the unique element will remain: Z.