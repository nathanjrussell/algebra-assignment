# Finite field determinants

Scientific repositories often form complex dependency chains and express algorithms directly from their scientific domain. Computer scientists routinely learn domains outside their formal training. Clients may be reluctant to approve complete refactoring.

Focus on one component where caching can replace repeated expensive calculations. Provide unit tests demonstrating that the enhanced implementation preserves the original results.

This program uses a field of prime power order 11³ = 1331. Its elements are represented by polynomials of degree below three with coefficients modulo 11. The field has relatively few elements, but repeatedly computing polynomial arithmetic is expensive.

Implement a cached finite field representation compatible with the existing matrix operations.

The program computes C = A × B and independently calculates all three determinants to verify

det(C) = det(A) × det(B)

Approximate requirements on an Apple M3 Max MacBook Pro using one CPU thread

| Matrix size | Implementation | Runtime | Peak memory |
| --- | --- | --- | --- |
| 4000 × 4000 | Original | 6 hours 40 minutes | 2.6 GB |
| 4000 × 4000 | Cached | 20 minutes 16 seconds | 0.53 GB |
| 6000 × 6000 | Original | 22 hours 30 minutes | 5.8 GB |
| 6000 × 6000 | Cached | 68 minutes | 1.2 GB |

The 4000 cached result was measured. Memory includes A, B, C, and one determinant working copy.

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DALGEBRA_ASSIGNMENT_SIZE=4000
cmake --build build --parallel
./build/src/algebra_determinants
```

