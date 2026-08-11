dim3 block(x, y)
dim3 grid(m, n)

genericKernel<<<grid, block, ..., stream>>>(*params);

blockId.x and blockId.y are used to identify which block in the grid a thread is in & threadId.x and threadId.y are used to identify which thread in that specific block.

While tempting to think of HARD rows, columns and dimensions like x, y and z, these params like blockId and threadId can be used to index and refer to anything. They are commonly used as indexes into an array on device memory, but CUDA does not impose that. We just do it because it makes sense to do so.

[[CUDA Model]]
[[GPU]]
[[Tiling]]
[[GEMM]]


