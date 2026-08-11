
General Matrix Multiplication.

Each thread calculates its elemental position in the output matrix.
Every Thread fetches A[elemental position] and B[elemental position] from memory
before completing multiplication.

[[GPU]]
