![[Pasted image 20250528185355.png]]

This is a really cool problem that requires a lot of lateral thinking. The first task is to understand the problem is truly a graph problem and requires the construction of an "adjacency" graph from the existing nodes. The key is to also see that the graph can be constructed with forward and backward traversal with inverse weights. The premise of the graph construction is as follows:

Say:
Equations[0] = [a/b]  and values [0] = 2. && Equations[1] = [b/c] and values[1] = 3.
It means that if we are querying the value of a/b then a useful way of representing that information is through a graph with nodes a and b and moving in the direction of a->b would represent a/b and the edge value would be 2. Now say we receive the query [a/c]. Well if we stick with our original graph construction principle, we could traverse from a->b which represents a/b. Then by traversing b->c that would represent b/c. By multiplying the "paths" together we have effectively done a/b * b/c = a/c. The values on the edges which are 2 and 3 respectively is now the effective value of a/c.
We would construct a hashmap of hashmaps. a: (b, 2) & b: (a, 1/2) would be the first 2 entries in the hashmap with a and b as the keys. Say we index 'a' then we can see in the second hashmap the different neighbours it has and the weights of the neighbours which in this case would be 'b' and 2.
It should be easy to see that after the graph itself has been constructed, a BFS can be done on every query element [src, target] to see if a viable path exists between src and target. The BFS is done using a queue structure.
