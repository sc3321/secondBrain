Function signature:

void foo(Struct& myStruct);

It is called by:

Struct myStruct;
foo(myStruct);

Identical in how it is called to pass by Value despite different signature. In this situation however, there is no copy made inside foo. Any changes to myStruct persist out of the scope of foo. 

[[Pass by value]]
