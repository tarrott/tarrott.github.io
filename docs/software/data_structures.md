# Data Structures

---
## Strings


---
## Arrays
- Strenghts:
    - fast lookups
    - fast appends
    - cache-friendly
- Weaknesses:
    - fixed size
    - costly inserts and deletes

- space -> O(n)
- lookup -> O(1)
- append -> O(1)
- insert -> O(n)
- delete -> O(n)

### Static
- fast lookups O(1), but each item in the array needs to be the same size, and you need a big block of uninterrupted free memory to store the array
- pointer-based arrays are not reflected in time cost, even though:
    - it's slower because it's not CPU cache-friendly (continious in RAM)
    - pointer-based array requires less uninterrupted memory and can accommodate elements that aren't all the same size

### Dynamic
- don't have to specify the size ahead of time, but the disadvantage is that some appends can be expensive
- Strenghts:
    - fast lookups
    - variable size
    - cache-friendly
- Weaknesses:
    - slow worst-case appends
    - costly inserts and deletes

- space -> O(n)
- lookup -> O(1)
- append -> O(1)
- insert -> O(n)
- delete -> O(n)

---
## Linked Lists
- have faster prepends and faster appends than dynamic arrays O(1), but they have slower lookups O(n)

---
## Hash Tables
- use hash function to translate key into an index
- handle hash collisions by having the value at the index be a pointer to a linked list with all keys that collide (considered rare enough with complicated balancing to still have O(1) lookups)

- Strenghts:
    - fast lookups O(1) on average
    - flexible keys allowing for most data types
- Weaknesses:
    - slow worst-case lookups O(n)
    - unordered keys
    - single-directional lookups (O(n) key lookup)
    - not cache-friendly (due to using linked lists)

- space -> O(n)
- lookup -> O(1) (Worst-case: O(n))
- insert -> O(1) (Worst-case: O(n))
- delete -> O(1) (Worst-case: O(n))

---
## Sets
- similar implentation of hash maps but keys do not store associated values
- useful for tracking groups of items—nodes visited in a graph, characters seen in a string, or colors used by neighboring nodes

---
## Trees

### Binary Tree
- a tree where each node has at most 2 children
- the number of total nodes on each level doubles as we move down the tree
- the number of nodes on the last level is equal to the sum of the number of nodes on all other levels (plus 1)
- total nodes = 2^h - 1
- height = lg(n+1)

### Binary Search Tree (BST)
- ordered binary tree
- the nodes to the left are smaller than the current node
- the nodes to the right are larger than the current node

Strengths:
- good performance across the board
    - lookups O(lg(n))
    - inserts O(lg(n))
    - deletes O(lg(n))
    - better worst-case performance than a hash table O(n) but not as good as its average case O(1)
- if balanced:
    - sorted in O(n) time using an in-order traversal
    - finding an element closest to a value can be done in O(lg(n))

Weaknesses:
- poor performance if unbalanced
- no constant time O(1) operations

### AVL

### Red-Black

### Perfect Tree
- height = lg((n+1)/2) + 1 = log(n+1)

---
## Graphs
- Representing links. Graphs are ideal for cases where you're working with things that connect to other things. Nodes and edges could, for example, respectively represent cities and highways, routers and ethernet cables, or Facebook users and their friendships
- Scaling challenges. Most graph algorithms are O(n*lg(n))O(n∗lg(n)) or even slower. Depending on the size of your graph, running algorithms across your nodes may not be feasible.

- directed vs. undirected (links with direction)
- cyclic vs. acyclic (cyclic graphs contains at least one unbroken series of nodes with no repeating nodes or edges that connects back to itself)
- weighted vs. unweighted (each edge contains a weight)
- legal coloring vs. illegal coloring (no adjacent nodes have the same color)