## Van Emde Boas Trees
- supporting operations for priority queue
  - binary heaps
  - red-black trees
  - Finonacci heaps
- 위 자료구조들은 적어도 하나는 BicO(log n) time 을 가짐
  - decisions on comparing keys 기반이기 때문
- van Emde Boas trees supports BicO(log log n) or BicO (log log u) operations
  - n : number of elements in the set
  - u : the range of possible values
  - search
  - insert
  - delete
  - minimum
  - maximum
  - successor
  - predecessor
- 조건
  - keys must be integers in the range 0 to n - 1
  - no duplicates

### 20.1 Preliminary approaches

#### Direct addressing
- The simplest approach to storing a dynamic set
- makde A[0 .. u - 1] for storing a dynamic set in {0, 1, 2, ..., u - 1}
```
if A[x] == dynamic set 
  A[x] = 1 
else 
  A[x] = 0
```
- BicO(1) operations
  - insert
  - delete
  - member
- Theta(u) operations
  - minimum
  - maximum
  - successor
  - predecessor
- 1 값을 연결해 가면 predecessor 를 찾아갈 수 있음

#### Superimposing a binary tree structure
- short-cut long scans shows in the bit vector
- Theta(u) worst-case time
  - minimum : root > down toward the leaves > only containing a 1 node
  - maximum : root > down toward the leaves > only rightmost containing 1 node
  - successor : x index leaf > up toward the root > enter a node (containing a 1) from left > right child z > down z (leftmost node containing a 1) 
  - predecessor : x index leaf > up toward the root > rightmost node containing a 1
- BicO(log u) in the worst-case
  - better than red black tree
    - searching a red-black tree is BicO(log n)
  - but if n < u a red-black tree would be faster 

#### Superimposing a tree of constant height
- u = 2^2k > squareRoot(u) is an integer
- let superimpose a tree of degree squareRoot(u)
  - height of the tree is always 2
- BicO(squareRoot(u)) time
  - minumun, maximum : find the leftmost/rightmost in summary (contains 1) > summary[i] > search within i index cluster 
  - successor, predecessor : search to the right/left within cluster 
  > if find 1, position is result > else let i = [x/squareRoot(u)] ? search to the right/left within the summary array from index i
  - delete : let i = [x/squareRoot(u)] > set A[x] to 0 > set summary[i] to the logical-or in the i index cluster
- search at most two clusters of squareRoot(u) takes BicO(squareRoot(u)) time
- using a tree of degree squareRoot(u) is the key of van Emde Boas trees

### 20.2 A recursive structure

### 20.3 The van Emde Boas tree
