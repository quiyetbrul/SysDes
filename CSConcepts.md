# Computer Science Concepts

## Data Structures

### Arrays vs Linked Lists

- Arrays
  - Fixed size
  - Random access
  - Cache-friendly
  - Insertion and deletion are O(n)
- Linked Lists
  - Dynamic size
  - Sequential access
  - Not cache-friendly
  - Insertion and deletion are O(1)

### Stacks vs Queues

- Stacks
  - LIFO
  - Push and pop are O(1)
- Queues
  - FIFO
  - Enqueue and dequeue are O(1)

### Trees

#### Binary Trees

- Each node has at most two children
- Perfect binary tree: all leaves are at the same level
- Full binary tree: each node has either 0 or 2 children
- Complete binary tree: all levels are completely filled except possibly for the last level, which is filled from left to right

#### Binary Search Trees

- Left child is less than parent
- Right child is greater than parent
- In-order traversal is sorted
  - `left -> root -> right`
- Pre-order traversal
  - `root -> left -> right`
- Post-order traversal
  - `left -> right -> root`

#### AVL Trees

- Self-balancing binary search tree
- Height difference between left and right subtrees is at most 1

#### Red-Black Trees

- Self-balancing binary search tree
- Each node is either red or black

#### B-Trees vs LSM-Trees

- B-Trees
  - Balanced tree
  - Each node has at most `m` children
  - Used in databases and file systems
- LSM-Trees
  - Log-structured merge tree
  - Optimized for write-heavy workloads
  - Used in databases

#### Tries

- Tree-like data structure
- Used for storing strings
- Each node represents a common prefix

### Graphs

- Vertices and edges
- Directed vs undirected
- Cyclic vs acyclic
- Weighted vs unweighted

#### Breadth-First Search

- Explore all neighbors before moving on to the next level

#### Depth-First Search

- Explore as far as possible along each branch before backtracking

### Hash Tables

- Key-value pairs
- O(1) average time complexity for insertion, deletion, and lookup

## Big O Notation

- the worst-case scenario for an algorithm in terms of time and space complexity

## Sorting Algorithms

### Comparison-based

- Bubble sort
- Selection sort

### Divide and Conquer

- Merge sort
- Quick sort

### Linear Time

- Counting sort
- Radix sort
