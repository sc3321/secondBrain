![Exported image](Exported%20image%2020250528103654-0.png)

![11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27...](Exported%20image%2020250528103654-1.png)

Honestly not the most involved problem with regards to different data structures etc. Just have to be very careful with keeping track of the correct -\>next pointers. The main issue I was facing was reassigning next pointers by doing odd and even in separate while loops which meant that I was leaving behind lots of dangle pointers. ￼Correct idea of taking copy of head of even list and just settings its -\>next to the evenhead.