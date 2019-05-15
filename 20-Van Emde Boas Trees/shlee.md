### van Emde Boas tree

``
Given a set S of elements such that the elements are taken from universe {0, 1, …. u-1}, perform following operations efficiently.

- insert(x) : Adds an item x to the set+ S.
- isEmpty() : Returns true if S is empty, else false.
- find(x) : Returns true if x is present in S, else false.
- insert(x) : Inserts an item x to S.
- delete(x) : Delete an item x from S.
- max() : Returns maximum value from S.
- min() : Returns minimum value from S.
- successor(x) : Returns the smallest value in S which is greater than x.
- predecessor(x) : Returns the largest value in S which is smaller than x.
``

``
- RB tree, AVL tree : O(lg n)
- bitvector : insert, delete, find in O(1), but other operations in O(u)
- vEB : insert, delete, find, successor, predecessor in O(lglogu), max, min in O(1)
``

- SEARCH, INSERT, DELETE, MINIMUM, MAXIMUM, SUCCESSOR, PREDECESSOR in O(lg lg n)
- MEMBER(S, x) : which returns a boolean indicating whether the value x is currently in dynamics set S
- n : the number of elements in the dynmic set
- u : the range of the possible values.
- van Emde Boas tree operation runs in O(lglg u) time


### Preliminary approaches

#### Direct addressing
- Hashing
- To store a dynamic set of values from the universe {0,1,2, ..., u-1}, we maintain an array A[0..u-1] of u bits
```kotlin
A[x] = if (x in set),
   1
 else 
   0
```   
  - INSERT, DELETE, MEMBER operation in O(1)
  - MINIMUM, MAXIMUM, SUCCESSOR, PREDECESSOR in Θ(u)
 
#### Superimposing a binary tree structure
- O(lg u)
- the entries of the bit viector form the leaves of the binary tree
- each internal node contains a 1 if and only if any leaf in its subtree contains a 1
- MINIMUM : `starting at the root` and `head down` toward the leaves, always taking the `leftmost` node containing a 1
- MAXIMUM : `starting at the root` and `head down` toward the leaves, always taking the `rightmost` node containing a 1
- SUCCESSOR(x) : `starting at the leaft indexed by x`, and `head up` toward the root until we enter a node from `the left and this node has a 1 in its right child z`. Then `head down` through node z, always taking the `leftmost` node containing a 1
- PREDECESSOR(x) :

#### Superimposing a tree of constant height
- O(lg sqrt(u))
- summary : sqrt(u)-bit subarray of A the ith cluster

### A recursive structure
- recursive structure that shinks by a factor of sqrt(u)
- index(x,y)
    - HIGH(x) = int(x/sqrt(u)) : the number of x's cluster
    - LOW(x) = x mod sqrt(u) : provides x's position 
    - x = index(HIGH,LOW) = HIGH * sqrt(u) + LOW

#### proto vEB structures
- u : universe size
- summary : pointer to proto-vEB structure(sqrt(u))
- cluster : sqrt(u) pointers array to proto-vEB structure(sqrt(u))

#### Operations on a proto vEB structure

##### MEMBER

##### MINIMUM

##### SUCCESSOR

##### INSERTING

##### DELETING

### vEB tree
- u : universe size
- summary : pointer to proto-vEB structure(sqrt(u))
- cluster : sqrt(u) pointers array to proto-vEB structure(sqrt(u))
- min : the minimum element in the vEB tree
- max : the maximum element in the vEB tree

##### MEMBER

##### MINIMUM

##### SUCCESSOR

##### INSERTING

##### DELETING

``
One of the reason is that complexity is defined not on the size of the set you store but on the size of the universe of values. Another difference is that keys can't be arbitrary types for which you have comparison operation but must be integers. You should not see vEB as an alternative for BST but rather as an alternative for arrays. An array have O(1) store and look up costs for object keyed by integers. vEB offer O(log log M), where M is size of the universe of your values. Now, you see vEB is not better than regular array for look ups and store but it offers O(1) min, max operations and O(log log M) prev next key operations which array does not. It's worth to mention that the layout of vEB trees has a properties which make possible to create cache oblivious trees which are far more interesting developments of modern CS.
``
