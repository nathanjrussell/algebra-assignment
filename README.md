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

# Assignment

I want collaboration within groups.  Please use the same latex format we used on the first assignment. 

* Use GitHub Kanban boards to create tasks for each member to complete.  Does not have to be formally linked to repo PR, branches, etc.
* The table above should indicate the scope for the level of improvement.
* Do not making sweeping changes in all dependency repositories.
* I recommend focusing on using the existing functionality, but wrap one dependency with an extended class that implements caching.  Remember, this is a finite field of order 11^3.  Many pairwise and single element computations only need to happen once.  I recommend you consider the generic class in C++ associated with unordered maps where you have a key and a value configuration.  This will largely remove the need for you to create your own hash maps.
* The Finite Field class sequentially (deterministically) finds the "first" irreducible polynomial.  No need to consider other representations.

## Code Repo

You should create a wrapper/replacement for one of the classes.  It should not require modifying any code that uses it.  It should be nearly a drop in replacement.  Your class (wrapper for finite field??) should use the original class, just implement caching (you decide how). It is likely you only need to change the CMAKE file to use your class.  If done correctly, you likely do not even have to change the `include` statements or `using` statements. 

## MPI Implementation

Once you have found good optimization using caching, find a way to implement determinants using MPI across different machines (messages across network and not within same machine).  Understand that for small matrices, your MPI version should be slow compared to sequential on a single machine.  Identify the break even point and provide the size where MPI is better (faster) than sequential on a single machine.  You have division available with Finite Fields so make sure you consider PLU decomposition.  You will find that it requires creativity to implement PLU across multiple nodes without creating messaging bottlenecks. 

## Written Report

The group will need to submit a written report detailing the following:

* What class in the dependency chain you picked to implement the caching (lookup)?
* What improvements (memory and timing) did your caching make in the program?
* Please include graphs that explain some profiling for speed and memory.
* What approach did you take for MPI?
* What is the break even point?
* Use the IEEE LaTex template you used for the other assignment.
* Create a PowerPoint presentation and be prepared to present.  You do not have to use Beamer.

## AI Usage

I expect you to consult AI, but you need to make sure you understand the algorithms. 

## Conclusion

Algorithm timing is always the dominating factor.  It is pointless to implement complex parallel programming when the sequential program could be improved using caching and other speed up methods.  You cannot assume that you control every repository or that your clients will allow you to make sweeping changes.  You should utilize existing functionality and explain that you are not changing the functionality...only caching results.  Implement unit tests to show that your code is a drop in replacement.  If you do this correctly (sequential caching), you likely only need to change the CMAKE dependency for one class (but use existing class for computational work).  DROP IN REPLACEMENT is the key. 
