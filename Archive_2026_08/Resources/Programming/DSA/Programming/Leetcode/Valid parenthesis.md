\> [!caution] This page contained a drawing which was not converted.   

![Exported image](Exported%20image%2020250528103630-0.png)

To solve this problem we need to use a combination of the stack data structure and a hashmap. Recapping what the stack DS looks like:
   

Very clever algorithm:
 
We use a hashmap where the keys are the closing parenthesis either ')', '}' or ']'.
 
Iterating through the string we encounter either closing parenthesis or opening parenthesis, nothing else.
 
We first check to see if the current character is a closing parenthesis:  
We do this by seeing if it exists in our HashMap as a key.  
If it does exist, we then check to see if the item on the top of the stack is the corresponding term for that key.  
If yes:  
Delete that item from the top of the stack and continue to the next term in the string.  
If no:  
Return false.
 
If the character is not a closing paranthesis we add that item to the top of our stack.
 
By the end of the iteration through the string, if the stack is not empty than we return false.