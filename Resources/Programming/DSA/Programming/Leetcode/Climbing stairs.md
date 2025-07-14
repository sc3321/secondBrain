![Exported image](Exported%20image%2020250528103644-0.png)   
This is a dynamic programming problem which requires a bottom up approach for its solution.
 ![Exported image](Exported%20image%2020250528103644-1.png)   
The following code which appears vague and unrelated to the problem statement comes from spotting that the possibility for the steps follows a Fibonacci pattern as illustrated:￼￼

![Exported image](Exported%20image%2020250528103645-2.png)  

The premise is as follows:￼For n = 5 in a simple case, when I am at the last step there is only 1 possible way to get to the last step which is to stay where I am. When I am the second last step there is only 1 possibly way to get to the last step which is to just take one step. These can be considered the base cases. It is clear to see that even in a scenario of 1000 steps this condition is still going to apply for the last and second last step.
 
Now when I am on the 3rd step- in this case, the third from last step, I can either take one step to step number 4 or 2 steps to step number 5. We then perceive this as basically saying- I'v already solved for those situations for when I am on the 4th step and the 5th step, so the total number of options I have is the sum of options I have on the 4th and the 5th step. Likewise when I am on the second step, I can take one step to the third step or 2 steps to the 4th step. The number of options I have therefore at the second step is basically the sum of options I have on the 3rd step and the 4th step. Working backwards with a small example such as n = 5 shows that the pattern follows a Fibonacci relationship so my code simply implements a Fibonacci sequence for the nth Fibonacci number.