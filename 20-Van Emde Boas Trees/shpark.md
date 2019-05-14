### van Emde Boas Trees
  - support the priority-queue operations in O(lglg u) worst-case time
    - Search(Memv, Insert, Delete, Minimum, Maximum, Successor and Predecessor
  - n : number of elements in the set
  - u (universe size) : range of possible values

#### Preliminary approaches
- to gain insights for understanding van Emde Boas trees
##### Direct addressing
  - maintain an array A[0 .. u - 1] of u bits
  - A[x] == 1 -> x is in the dynamic set
  - Insert, Delete and Member time complexity : O(1)
  - Minimum, Maximum, Succeesor, and Predecessor worst case time complexity : Theta(u)
##### Superimposing a binary tree structure
  - The entries of the bit vector form the leaves of the binary tree, and each internal node represents whether subtree contains 1
  - Member time complexity : O(1)
  - Insert, Delete, Minimum, Maximum, Succeesor and Predecessor time complexity: O(lg u)
##### Superimposing a tree of constant height
  - superimpose a tree of degree square root of u
  - Member and Insert time complexity : O(1)
  - Minimum, Maximum, Successor, Predecessor and Delete time complexity : O(square root of u)
#### A recursive structure
  - shrink the universe size by the square root at each level of recursion
  - x (lg u-bit binary integer)
    - high(x) : rounddown(x / (squre root of u))
    - low(x) : x mod (square root of u)
    - index(x, y) = x * (squre root of u) + y
##### Proto van Emde Boas structure
  - summary field (pointer to a proto-vEB(root(u)) structure) 
  - cluster array (root(u) pointers, each to a proto-vEB(root(u)) structure)
##### Operations on a proto van Emde Boas structure
  - Member(x) : O(lg lg u) time
  - Minimum(V) : O(lg u) time 
    - requires two recursive call on proto-vEB(root(u)) structures
      - one for finding minimum cluster
      - one for finding minimum element in cluster
  - Successor(V, x) : Theta(lg u lg lg u)
    - calls itself recursively twice on proto-vEB(root(u)) structures
      - find Successor in same cluster
      - find Successor cluster using summary
    - additinal Minimum(V) call
  - Insert(V, x) : Theta(lg u) time
    - two recursive call
      - insert into cluster
      - update summary
  - Delete(V, x) : Theta(lg u) time (if add attribute n counting hw many elements it has)
#### The van Emde Boas Tree
  - high(x) : rounddown(x / (downed square root of u))
  - low(x) : x mod (downed squre root of u))
  - index(x, y) : x * (downed squre root of u) + y
##### van Emde Boas trees
  - add two attributes to proto-vEB structures 
    - min : minimum element in the vEB tree
    - max : maximum element in the vEB tree
  - the element stored in **min** does not appear in any of the recursive vEB(downed square root of u) trees that the cluster array point to
  - advantage of using additinal attributes 
    1. Minimum and Maximum operations do not need to recurse
    2. Successor operation can avoid making a recursive call to determine whether the successor of a value x lies within same cluster
    3. min and max helps determine vEB tree has no elements, exactly one element, or at least two elements in constant time
    4. If vEB tree is empty, we can insert an element into it by updating only its min and max attributes
##### Operations on a van Emde Boas tree
  - Minimum(V) and Maximum(V) : O(1) time
  - Member(V, x) : O(lg lg u) time
  - Successor(V, x) : O(lg lg u) time
    - using maximum value in a vEB tree, we can avoid making two recursive calls
    - recursive call on either on a cluster of on the summary, but not on both
  - Predecessor(V, x) : O(lg lg u) time
    - additonal case - predecessor is not reside in x's cluster, then must reside in min value
  - Insert(V, x) : O(lg lg n) time
    - Do not need to update cluster when cluster already has another element
  - Delete(V, x) : O(lg lg n) time
