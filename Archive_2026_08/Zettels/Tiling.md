
More performant GEMM implementation. Tiles(Chunks) of each input matrix, A and B, are loaded onto shared GPU memory from global memory.