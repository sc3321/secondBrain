All matrices are stored in row-major format in memory as a flat contiguous array.

N * M matrix is stored as an N * M long linear flat array.

Memory stride is what is used to "index" into rows:

A(x)(y) = *A(x * stride + y)

[[GPU]]
[[Shared memory]]
[[Tiling]]
[[CUDA Model]]
