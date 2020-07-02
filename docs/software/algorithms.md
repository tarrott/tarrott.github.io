# Algorithms
- algorithm categories
- how long does the algorithm run with 1,000 records and does it scale to 1 million?
- common sense estimation
    - O(n): simple loops (1 to n)
    - O(n^2): nested loops
    - O(lg(n)): binary chop (halves the set it considers each time it starts the loop)
    - O(nlg(n)): divide and conquer (partition input, work on partitions separetly, and then combine results)
        - degrades to O(n^2) when fed sorted imput
    - O(2^n): combinatoric (permutations)

---
## Patterns
1. Brute Force
    - enumerate all possible solutions, and check them for correctness
2. Greedy
    - Keep track of the base answer so far, in one pass through the input
3. Hash Table
    - Spend space to save time by using a hash map or sometimes just an array

---
## Sorting

---
## Recursion
- even if the algorithm does not create any data structures, the call stack will generate O(n) space complexity

---
## Dynamic Programming


---
## Graph

### BFS
- queue
- will find shortest path
- usually takes more memory than DFS

### DFS
- stack
- can easily be implemented with recursion