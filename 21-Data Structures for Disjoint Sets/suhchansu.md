## Data Structures for Disjoint Sets
- disjoint sets need 2 operations
  - find
  - union

### 21.1 Disjoint-set operations
- Support operations
  - makeSet(x) : create a new set
  - union(x, y) : sum a two sets
  - findSet(x) : return a pointer

#### An application of disjoint-set data structures
- disjoint-set uses in undirected graphs
```
// connectedComponents(graph)
for vertex in vertexs {
  makeSet(vertex)
}

for edge(units, vertex) in edges {
  if findSet(units) != findSet(vertex) {
    union(units, vertex)
  }
}

// sameComponent(units, vertex) 
if findSet(units) == findSet(vertex) {
  retrun true
} else {
  return false 
}
```

### 21.2 Linked-list representation of disjoint sets
- disjoint sets can make by linked list
- BicO(1) 
  - makeSet
  - findSet

#### A simple implementation of union
- Theta(n^2) or Theta(n)
  - makeSet > union using linked list
- The total number of operations is 2n - 1
  - the amortized tiem is Theta(n)

#### A weighted-union heuristic
- Theta(n) is too long time
- Using a weighted-union heuristic, makeSet, union, findSet operations running time is
  - BicOmega(n)
  - BicO(m + nlogn)
- smaller set 이 lager set 에 union 하는거 > BicO(nlongn)
- m 개의 operations 들 수행 > BicO(m + nlongn)

### 21.3 Disjoint-set forests
- use tree to make disjoint set 

#### Heuristics to improve the running time
- union by rank
  - similar to the weighted-union heuristic
  - degree 가 smaller tree 를 bigger tree 에 uninon 하는거
  - degree 를 `rank` 에 저장
  - tree 의 높이를 일정하게 유지할 수 있음
- path compression
  - findSet operation 에서 실행
  - each node on the find path point directly to the root
  - do not change rank

#### Pseudocode for disjoint-set forests
```
// makeSet(x)
x.p = x
x.rank = 0

// union(x, y)
link(findSet(x), findSet(y))

// link(x, y)
if x.rank > y.rank {
  y.p = x
} else {
  x.p = y
  if x.rank == y.rank {
    y.rank == y.rank + 1
  }
}

// findSet(x)
if x != x.p {
  x.p = findSet(x, p)
}
retrun x.p
```

#### Effect of the heuristics on the running time
- only union by rank 
  - BicO(mlogn)
  - worst case : Theta(n + f(1 + log2+f/n n))
- union by rank + path compression
  // alpha(n) : very slowly growing function
  - worsk case : BicO(m alpha(n)) 

### 21.4 Analysis of union by rank with path compression

#### A very quickly growing function and its very slowly growing inverse
  - disjoint operation 들의 평균 시간은 BicO(Ackeramnn's function(n))
  - 거의 모든 N 에서 4 이하의 값을 가짐
  - 모든 입력값에 대해 상수시간을 보장

#### Properties of ranks
- using union by rank + path compression is BicO(m * Ackeramnn's function(n)) 증명

#### Proving the time bound
- ㅎ;

#### Potential function
- ㅎㅎ;

#### Potential changes and amortized costs of operations
- ㅎㅎㅎ;