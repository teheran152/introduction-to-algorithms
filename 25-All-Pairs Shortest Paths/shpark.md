### 25. All-Pairs Shortest Paths
  - W : edge weights of an n-vertex directed graph G = (V, E)
    - w_ij
      - if i == j -> 0
      - if i != j and (i, j) ∈ E -> the weight of directed edge (i, j)
      - if i != j and (i, j) !∈ E -> infinite 
  - predecessor matrix `PI`
    - π_ij : the predecessor of j on some shortest path from i

#### 25.1 Shortest paths and matrix multiplication
##### The structure of a shortest path
  - shortest path p from vertex i to vertex j
  - p contains at most m edges
  - no negative-weight cycles, and m is finite
  - if i == j -> p has weight 0 and no edges
  - if i != j -> p : i ~> k -> j with path p' from i to k, p' contains at most m-1 edges and p' is a shortest path from i to k

##### A recursive solution to the all-pairs shortest-paths problem
  - l(m)_ij : minimum weight of any path from vertext i to vertex j that contains at most m edges
  - l(m)_ij = min { l(m-1)_ik + w_kj } (for all k s.t. 1 <= k <= n)

##### Computing the shortest-path weights bottom up
  EXTEND-SHORTEST-PATHS(L, W)
  1  n = L.rows
  2  let L' = (l'_ij) be a new n x n matrix
  3  for i = 1 to n
  4    for j = 1 to n
  5      l'_ij = infinite
  6      for k = 1 to n
  7        l'_ij = min(l'_ij, l_ik + w_kj)
  8  return L'
  - time complexity of extending shortest paths : Theta(n^3)
  - relation to matrix multiplication
    - matrix multiplication : c_ij = sum(a_ik * b_kj)
    - l(m-1) -> a
    - w -> b
    - l(m) -> c
    - min -> +
    - + -> *
  - time complexity of computing all pairs shortest paths : Theta(n^4)

##### Improving the running time
  - using the fact that L(m) = L(n-1) for all integers m >= n-1, compute L(n-1) with only roundup(lg(n-1)) matrix products
  - repeated squaring
    - L(1) = W
    - L(2) = W^2 = W * W
    - L(4) = W^4 = W^2 * W^2 ...
  FASTER-ALL-PAIRS_SHORTEST-PATHS(W)
  1  n = W.rows
  2  L(1) = W
  3  m = 1
  4  while m < n - 1
  5    let L(2m) be a new n x n matrix
  6    L(2m) = EXTEND-SHORTEST-PATHS(L(m), L(m))
  7    m = 2m
  8  return L(m)
  
  - time complexity : Theta(n^3 * lg n)

#### 25.2 The Floyd-Warshall algorithm
  - time complexity : Theta(V^3)
##### The structure of a shortest path
  - intermediate vertex of a simple path p = <v_1, v_2, ..., v_l> : any vertex of p other than v_1 or v_l, {v_2, v_3, ..., v_l-1}
  - V = {1, 2, ..., n}
  - for any pair of vertices i, j ∈ V, consider all paths from i to j whose intermediate vertices are all drawn from {1, 2, ..., k}
  - The Floyd-Warshall algorithm exploits a relationship between path p and shortest paths from i to j with all intermediate vertice 
    - If k is not an intermediate vertex of path p, then all intermediate vertices of path p are in the set {1, 2, ..., k-1}.
    - If k is an intermediate vertex of path p, then p : i ~> k ~> j
      - p1 : shortest path form i to k, all intermediate vertices are in the set {1, 2, ..., k-1}
      - p2 : shortest path from k to j, all interemdiate vertices are in the set {1, 2, ..., k-1}
##### A recursive solution to the all-pairs shortest-paths problem
  - d(k)_ij : the weight of a shortest path from vertex i to vertex j for which all intermediate vertices are in the set {1, 2, ..., k}
    - w_ij    if k == 0
    - min(d(k-1)_ij, d(k-1)_ik + d(k-1)_kj)   if k >= 1
##### Computing the shortest-path weights bottom up
  FLOYD-WARSHALL(W)
  1  n = W.rows
  2  D(0) = W
  3  for k = 1 to n
  4    let D(k) = (d(k)_ij) be a new n x n matrix
  5    for i = 1 to n
  6      for j = 1 to n
  7        d(k)_ij = min(d(k-1)_ij, d(k-1)_ik+d(k-1)_kj)
  8  return D(n)

  - time complexity : Theta(n^3)

##### Constructing a shortest path
  - Π : predecessor matrix
  - π(k)_ij : the predecessor of vertex j on a shortest path from vertex i with all intermediate vertices in the set {1, 2, ..., k}
    - π(k-1)_ij    if d(k-1)_ij <= d(k-1)_ik + d(k-1)_kj
    - π(k-1)_kj   if d(k-1)_ij > d(k-1)_ik + d(k-1)_kj

##### Transitive closure of a directed graph
  - transitive closure of G : graph G* = (V, E*), where 
    - E* = {(i, j) : there is a path from vertex i to vertex j in G}
  - Theta(n^3) time with assigning a weight of 1 of each edge of E and run Floyd-Warshall algorithm
  - If there is a path from vertex i to vertex j, d_ij < n. Otherwise, d_ij = infinite
  - use logical operations - t(k)_ij = t(k-1)_ij OR (t(k-1)_ik AND t(k-1)_kj)

#### 25.3 Johnson's algorithm for sparse graphs
