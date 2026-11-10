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

## Assignment

Work together within your group and use the same IEEE LaTeX template as in the first assignment.

- Use a GitHub Kanban board to assign tasks to each group member. Standalone cards are sufficient.
- Use the timing and memory figures above to gauge the scope for improvement.
- Keep the caching changes focused on one dependency and reuse its existing arithmetic.

### Code repository

Wrap one class with a cached implementation that can serve as a drop-in replacement. The finite field class is a good candidate. Aim to preserve its public interface, header paths, and type names so callers can switch implementations by changing the dependency in CMake.

Operations on the same field element or pair of elements can reuse a previously computed result. Consider `std::unordered_map` to store results keyed by their operands. Keep the irreducible polynomial selected by the finite field class's deterministic search.

### MPI implementation

After improving the sequential version with caching, use MPI to compute determinants across multiple machines. Consider PLU decomposition, which uses the field's division operation. Pay attention to how pivot selection and matrix updates affect network communication.

Compare determinant runtimes for the same matrices using the cached sequential implementation and the distributed MPI implementation. For small matrices, communication overhead can outweigh the gains from parallel computation. Report the tested sizes and the smallest size at which MPI is faster, if one occurs.

### Written report

Submit a group report in the IEEE LaTeX template. Include the following information.

- The class you chose to cache and where it sits in the dependency chain
- Runtime and peak memory before and after caching, with profiling graphs
- Your MPI approach and the results of the break-even measurements

Prepare a PowerPoint presentation and be ready to present your results. Beamer is optional.

### AI usage

Consult AI during the assignment and make sure you can explain the algorithms you use.

### Conclusion

Improve the sequential implementation before measuring what parallel execution adds. A drop-in replacement lets you focus on the expensive computation when you have limited control over the dependency chain. Use the original arithmetic on cache misses and demonstrate equivalent results with unit tests.
