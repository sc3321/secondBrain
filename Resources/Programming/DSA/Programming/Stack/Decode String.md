![394 Decode String Topics Companies Medium Given an...](Exported%20image%2020250528103724-0.png)
 
class Solution {  
public:  
string decodeString(string s) {  
stack\<char\> st;  
for (int i = 0; i \< s.size(); i++) {  
if (s[i] != ']') {  
st.push(s[i]);  
} else {  
string curr_str = "";  
while (st.top() != '[') {  
curr_str = st.top() + curr_str;  
st.pop();  
}  
st.pop(); // for '['  
string number = "";  
// for calculating number  
while (!st.empty() && isdigit(st.top())) {  
number = st.top() + number;  
st.pop();  
}  
int k_time = stoi(number); // convert string to number  
while (k_time--) {  
for (int p = 0; p \< curr_str.size(); p++)  
st.push(curr_str[p]);  
}  
}  
}  
s = "";  
while (!st.empty()) {  
s = st.top() + s;  
st.pop();  
}  
return s;  
}  
};

The code is not the prettiest but is a solution that I could not come up with. The main premise is to keep adding to the stack until we encounter a close bracket then to keep popping from the stack until we get to an open bracket. Since the stack is for characters we need multiple for loops and while loops to build up strings and numbers properly.
 
As stacks operate under LIFO format, when "reading" from a stack by repeatedly popping, we will be reading backwards. To avoid having to then use a two pointer loop to reverse a string, the following method gives an inline solution:￼￼string curr_str = "";￼while(){  
Curr_str = stack.top() + curr_str;  
Stack.pop();  
}
 
We pop from the stack and add to the left.