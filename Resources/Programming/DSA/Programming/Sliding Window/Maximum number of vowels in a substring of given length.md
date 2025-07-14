![1456 Maximum Number Of Vowels in a Substring Of Gi...](Exported%20image%2020250528103707-0.png)

![0 d d](Exported%20image%2020250528103708-1.png)

Very clever algorithm which uses both the sliding window AND the 2 pointers approach.
 
Initialise the left and the right pointer to 0. In the code, the iterator [i] , is the right pointer.
 
Initialise num = 0 where num is the number of vowels in the current window.
 
For the first few iterations, we check to see if s[i] is a vowel and if it is, increment the num variable.
 
The clever part comes next when we check to see if we are greater than the window length. This check is done by the if statement: if( I - L + 1 \> k)  
If we are greater, then we know to increment the left pointer. BUT before we do this, if the left pointer was previously pointing at a vowel, we need to decrement the number of vowels in the current window.
 
We do a final check to see if the number of vowels in the current window is bigger than the maximum and if it is, we update the max!