### B-Trees
- Usage
  - B-trees are balanced search trees designed to work well on disks or other direct access secondary storage devices.
  - Many database systems use B-trees, or variants of B-trees, to store information.

- Attributes
  - B-tree nodes may have many children, from a few to thousands.
  - We can use B-trees to implement many dynamic-set operation in time O(lg n)
  - If an internal B-tree node x contains x.n keys, then x has x.n + 1 children
    - x.c1 < x.key1 < x.c2 < x.key2 < ... < x.c(t-1) < x.key(t-1) < x.c(t)

- Data structures on secondary storage
  - A B-tree node is usually as large as a whold disk page, and this size limits the number of children a B-tree node can have.

#### Definition of B-trees
- Every node x has the following attributes:
  - x.n : the number of keys current stored in node x
  - x.keys : stored in nondecreasing order (x.key1 <= x.key2 <= ... <= x.key(x.n)
  - x.leaf : indicates it is leaf or not
- Each internal node x also contains x.n+1 pointers (x.c(1) ... x.c(x.n+1)) to its chtildren
- The keys x.key(i) seperates the ranges of keys stored in each subtree
  - if k_i is any key stored in the subtree with root x.c_i then
  - k_1 <= x.key_1 <= k_2 <= ... <= x.key_x.n <= k_x.n+1
- All leaves have the same depth, which is the tree's height h
- Nodes have lower and upper bounds on the number of keys they can contain => t (>= 2, minimum degree of B-tree)
  - Every node other than the root must have at least t-1 keys. (at least t childeren)
  - Every node may contain at most 2t-1 keys

- Although the height of the tree grows as O(lg n) in both B-trees and red-black trees, B-trees the base of logarithm can be many times larger.

#### Basic operations on B-trees
##### Searching a B-tree
- disk operations : O(h) = O(log_t n)
- total CPU time : O(t*h) = O(t*log_t n)
#### Creating an empty B-tree
- disk operations : O(1)
- CPU time: O(1)
##### Inserting a key into a B-tree
- Splitting a node in a B-tree
- Inserting a key into a B-tree in a single pass down the tree

#### Deleting a key from a B-tree
- Must ensure that a node dosn't get too small during deletion
