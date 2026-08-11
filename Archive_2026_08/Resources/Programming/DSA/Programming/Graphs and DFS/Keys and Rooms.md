![Exported image](Exported%20image%2020250528103615-0.png)

![Exported image](Exported%20image%2020250528103615-1.png)

In CPP it is always easier to use a vector for declaring any indexed set. Arrays are C style data structures and they do not have methods like add, size etc.
 
We start by initializing a vector with size of the number of rooms and every room is labelled false as we have not visited any. We then initialize the first room to true because by default room 0 is unlocked. ￼￼We are kind of doing a clever BFS here where we look through available keys. On the first cycle we have the first key because the first room is unlocked. By first I mean index 0.
 
We use this key at the top of the stack (then we pop it) as an index into the rooms vector and then look at all the keys that room has inside it. If we have not seen that key before we mark the room that key opens as true and then add that key to the stack. For example 2 in the question we would be marking rooms 1 and 3 as true and adding those keys to the stack.￼￼We keep going until the stack is empty. At this point we know that we have opened all the possible rooms we had keys for. ￼￼At the end we simply check if all the rooms have been marked as true.