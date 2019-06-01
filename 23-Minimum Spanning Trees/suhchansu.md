## Minimum Spanning Trees
- acyclic + connects all vertices + tree
- w(T) = Sigma w(u, v)
  - w : weight
  - T : tree
  - e : edge
  - v : vertex
- Kruskal's algorithm
  - BicO(E log V)
  - by using ordinary binary heaps
- Prim's algorithm
  - BicO(E + V log V)
  - by using Fibonacci heaps

### 23.1 Growing a minimum spanning tree
- prior to each iteration, A is a subset of some MST
  - find (u, v) 
  - compare (u, v) with pre G
  - if safe edge > connect with G
    - safe edge : not connected with pre G

``` Swift 
// genericMst(G, w)
A = 0
while (A != spaningTree) {
  let edge = findEdge(u, v) 
  if isSafeEdge(edge) {
    A = uninon(A, (u, v))
  }
  return A
}
```
- light edge : low weight edge

#### Theorem 23.1
- G = (V, E) connected
- A : subset of E in G(MST)
- let (S, V-S) is any cut of G
- let (u, v) is light edge crossing (S, V-S)
- then (u, v) is safe for A

#### Proof
- let T is MST includes A
- T don't have light edge (u, v)
- (u, v) is safe edge for A

- (u, v) is cycle with simple path p
- one dege in T lies on simple path p because (u, v) is opposite side of cut (S, V-S)
- let (x, y) is not in A
- (x, y) break T into two components because (x, y) is unique with (u, v)
- T' = T - ((x, y) U (u, v))
- T' is MST 
  - w(u, v) < w(x, y)
  - because (u, v) is light edge crossing (S, V-S)
  - because (x, y) is cross
- w(T') = w(T) - w(x, y) + w(u, v) <= w(T)
- T is MST 
  - w(T) <= w(T')
  - T' is MST
- (u, v) is safe edge for A
- A is subset of T'
  - A is subset of T
  - (x, y) != subarray of A
  - A U (u, v) != subset of T'
  - T' is a MST && (u, v) is safe for A

#### Corollary 23.2
- G = (V, E) is connected
- let A is subset of E that is MST for G
- let C = (Vc, Ec) is connected in G = (V, A)
- (u, v) is light edge with C 
- (u, v) is safe for A

#### Proof
- cut(Vc, V-Vc) recpects A
- (u, v) is light edge for this cut
- (u, v) is safe for A

### 23.2 The algorithms of Kruskal and Prim

#### Kruskal's algorithm
- ueses disjointSet
  - uses heuristics for fast
    - union by rank
    - path compression
- while(select light edge > add edge > isGraph)

``` Swift
func kruskal(G, w) -> Graph {
  let A: Graph = nil

  for vertex in G.V {
    makeSet(vertex)
  }

  sort G.E by w

  for edge in G.E {
    if findSet(u) != findSet(v) {
      A = sum(A, (u, v))
      union(u, v)
    }
  }
  return A
}

```
- init A is BicO(1)
- sorting is BicO(E log E)
- findSet, uinon are BicO(E)
- total : BicO((V + E) alpha(V))
  - alpha : very slowly growing function 
- disjointSet operations are BicO(E alpha(V))
  - alpha(|V|) == BicO(log V) == BicO(log E)
  - total time of Kruskal : BicO(E log E)
  - |E| < |V|^2 
  - log|E| == BicO(log V)
  - Kruskal time : BicO(E log V)

#### Prim's algoritm 

``` Swift
func prim(G, w, r) {
  for u in G.V {
    u.key = infinity
    u.pi = nil
  }
  r.key = 0

  let Q = G.V
  while Q != nil {
    let u = extractMin(Q)
    for v in G.adj[u] {
      if isSubset(v, Q) 
      && w(u, v) < v.key {
        v.pi = u
        v.key = w(u, v)
      }
    }
  }
}
// v.key : minimum weight edge connecting v to other v
// v.pi : parent of v
// Q : min priority queue
```

- Prim time depends on how to imprement min priority queue
  - binary min heap
    - build min heap : BicO(V)
    - while : |V|
    - extractMin() : BicO(log V)
    - total extractMin : BicO(V log V)
    - for loop : O(E) 
    - sum of lenghs of all adjacecy list : 2|E|
    - decreaseKey() by binary min heap : BicO(log V)
    - Prim's total time : BicO(V log V + E log V) == BicO(E log V) == Kruskal time
  - finonacci heap
    - BicO(E + V log V)
