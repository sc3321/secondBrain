Designated by the "shared" specifier in CUDA. 
All threads in a given thread block have access to memory allocated with this specifier
Useful in tiling applications, every thread can read/write to this shared memory that is handled by cuda.
Effective to 1 global memory fetch in a block.

[[Tiling]]
[[GPU]]
[[CUDA Model]]

