Compilers have 2 important jobs:
    
    1) Generate code that respects the language specification.
    2) Generate efficient code.

Compilers will assume that the programmer's code does NOT exhibit undefined behaviour. If it does, then it is fair game as to what the compiler will output. This is the cause of undefined behaviour.

Languages allow for undefined behaviour because:
    
    Standardising behaviour for all hardware would be expensive and difficult. Different hardware will offer support and protection for different forms of undefined behaviour. E.g Signed integer overflow is handled as a wrap in x86 but as a trap in MIPS. Specifying this specific behaviour would be expensive in LoC therefore for different hardwares.

    Allowing UB allows the programs to make powerful optimisations. This is especially useful for things like loops:

    bool find(int *Array, int x, int n){
        for(int i = 0; i < n; i++){
            if(Array[i] == x}
                return true;
            }
            return false;
        }
    }


    For a 64 bit system, pointers are 64 bit as well. Therefore for the line Array[i], it is actually doing:
        *(Array + i)
    Now Array is a 64 bit number so i must also be extended to be a 64 bit number. By making the assumption that there is no signed overflow, this optimisation is allowed to take place without any issue.


