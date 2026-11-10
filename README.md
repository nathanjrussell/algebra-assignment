# Finite field determinants

Scientific software often spans several interdependent repositories. Its code may follow the algorithms and mathematical definitions of a scientific field. Computer scientists routinely learn about fields outside their formal training. Clients may be reluctant to approve a complete refactor.

Focus on one component where caching can replace repeated expensive calculations. Use unit tests to show that the cached implementation produces the same results as the original.

This program uses a finite field of prime power order 11³ = 1331. It represents elements as polynomials of degree less than three with coefficients modulo 11. The field has relatively few elements, but repeated polynomial arithmetic is expensive.

Implement a cached finite field representation compatible with the existing matrix operations.

The program computes C = A × B and calculates det(A), det(B), and det(C) independently. It then checks that

det(C) = det(A) × det(B)

Approximate requirements on an Apple M3 Max MacBook Pro using one CPU thread

| Matrix size | Implementation | Runtime | Peak memory |
| --- | --- | --- | --- |
| 4000 × 4000 | Original | 6 hours 40 minutes | 2.6 GB |
| 4000 × 4000 | Cached | 20 minutes 16 seconds | 0.53 GB |
| 6000 × 6000 | Original | 22 hours 30 minutes | 5.8 GB |
| 6000 × 6000 | Cached | 68 minutes | 1.2 GB |

The 4000 × 4000 cached figures come from a benchmark. Peak memory includes A, B, C, and one determinant working copy.

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DALGEBRA_ASSIGNMENT_SIZE=4000
cmake --build build --parallel
./build/src/algebra_determinants
```
