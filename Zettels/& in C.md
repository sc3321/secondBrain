& in a declaration is a reference and MUST equal an existing object.
Struct a;
Struct& b = a;

b is now basically an alias for a. Thats why when a func signature is foo(Struct& tmp) we must call it via foo(Struct existing_struct) so that tmp now binds to existing_struct as an alias.

In an expression however, & means "give me the address". 
Struct a;
Struct* b = &a;

b now points to the ADDRESS of a. b->val can be used to modify any struct values in A but it can point to something else later or even to a nullptr.

[[Pass by value]]
[[Pass by reference]]
