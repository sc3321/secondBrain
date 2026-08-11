Function signature:

void foo(Struct myStruct);

It is called like:

Struct myStruct;
foo(myStruct);

A copy of myStruct is made inside the function and used for the duration of that scope.

[[Pass by reference]]


