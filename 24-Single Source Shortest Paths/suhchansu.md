## Single Source Shortest Paths
- ex) shortest route from Phoenix to Indianapolis 
  - enumerate all the routes 
  - add up the distances on each route
  - poor choice : passes through Seattle, several hundred miles out of the way

- this chapter shows how to solve such problems efficiently
  - uses wighted
  - a shortes path from u to v (vertices) is defines weighted p
  - w(p) = delta(u, v)
  - vertices : intersections
  - edges : road segments between intersections
  - edge weights : metrics
    - road distance, time, cost, penalties, loss, any other quantity

#### Variants
- Single-destination shortes-paths problem
  - destination t from each v (vertices)
  - by reversing the direction of each edge
- Single-pair shortest-path problem
  - find (u, v) in (u, v) vertices
  - worst-case asymptotic running time
- All-pair shortes-paths problem
  - find (u, v) for every pair (u, v)
  - Chapter 25 address the all-pair problem in detail

#### Optimal substructure of a shortes path
- shortes path between two vertices contains other shortest paths
  - Chapter 26 Edmonds-Karp maximum flow algorithm
- uses dynamic programming, greedy method, FloydWarshall algorithm (Chater 25)

#### Lemma 24.1 (Subpaths of shortes paths are shortes paths)
- G = (V, E) with weight function w: E -> R
- let p = shortes path from v0 to vk
- let i, j = 0 <= i, j <= k
- let p_ij = shortes path from v_i to v_j

#### Proof
- decompose path : v_0 -> v_i -> v_j -> v_k
  - p_0i, p_ij, p_jk
- w(p) = w(p_0i) + w(p_ij) + w(p_jk)
- let p'_ij < p_ij
  - w(p') < w(p) 
  - find contradicts that p is a shortes path from v_0 to v_k

#### Negative-weight edges
- edges has negaive weights
- let graph G has no negative weight cycles from s
  - shortes-path weight delta(s, v) remains well defined
  - even if it thas a negative value
- let graph G has negative weight cycle from s
  - shortes path weights are not well defined
- let no path from s to vertex on the cycle
  - that is shortes path
  - travers the negative weight cycle s to v
  - it difin delta(s, v) is minus infinity
- Dijksta algorithm auume that all edge weights are nonnegative
- Bellman-Ford algorithm allow negative weight edges

#### Cycles
- shortes path contains a cycle, cannot contain a negative weight cycle
- let p = path in (v_0 .. v_k)
- let c = positive weight cycle in (v_0 .. v_j)
- let p' = weight p, w(p') = w(p) - w(c) < w(p)
  - p cannot be a shortes path from v_0 to v_k
- remove a 0 weight cycle from any path
  - shortest path from s to v contains a 0 weight cycle
  - there is another shortes path form s to v without this cycle
  - shortest paths have no cycles
  - G = (V, E)
    - edges = |V| - 1
    - shortes paths are in |V| - 1 edges

#### Representing shortes paths
- compute vertices on shortest path
- shortest path simular with breadth first trees in 22.2
  - G = (V, E)
  - v is in V a `predecessor` v.pi is either another vertex of nil
  - printPath(G, s, v) from Section 22.2 will print shortest path form s to v

- in BFS, `predecessor subgraph` G_pi = (V_pi, E_pi) induced by the pi values
- V_pi, E_pi are indued by the pi values
  - V_pi : set of the vertices
  - E_pi : set of the directed edgs

- rooted tree containing a shortes path from the soruce s to vertex is reachable from s
  - but, BTS tree contains shortest paths from the source defined in terms of edge weights instead of number of edges
  - let G = (V, E) be a weighted, directed graph
  - G contains no negative weight cycles
  - shortes paths are well defined
  - `shortest paths tree` shows any edge from root s has sortest path

- shortes paths are not necessarily unique

#### Relaxation
- `relaxation` is echnique
- let v.d = upper bound on the weight of a shortes path from s. o v
- v.d is a `shortest path estimate`

``` Swift
func initializeSingleSource(G, s) {
  for vertex in vertices {
    v.d = infinity
    v.pi = nil
  }
  s.d = 0 
}

// initializeSingleSource(:) runs in BicTheta(V)
```

- process of `relaxing` in an edge (u, v) 
  - updating v.d, v.pi
  - decrese the value of the shortes path estimate v.d, v.pi

``` Swift
func relax(u, v, w) {
  if v.d > (u.d + w(u, v)) {
    v.d = u.d + w(u, v)
    v.pi = u
  } 
}

// relax(:) runs in BicO(1)
```
- 24.3 shows two example
  - shortest path estimate decrease
  - no estimate changes
  - theses examples use initializeSingleSource(:), relax(:)


#### Properties of shortest paths and relaxation
- Section 24.5 proves properties of shortest path and relaxation
- Triangle inequality
- Upper bound property
- No path property
- Convergence property
- Path relaxation property
- Predecessor subgraph property

#### Chapter ouline
- 24.1 Bellman Ford algorithm
  - solves the single source shortest paths in general case in which edges have negative weight
  - simple, can detect whether a negative weight cycle is reachable from the source
- 24.2 linear time algorithm 
  - compute shortest paths form a single source in a directed acycle graph
- 24.3 Dijksta's algorithm has a lower time than the Bellman Ford but requires the edge weights to be nonnegative
- 24.4 use Bellman Ford to solve a special case of linear programming
- 24.5 prove the properties of shortes path and relaxation

### 24.1 The Bellman-Ford algorithm

### 24.2 Single-source shortes paths in directed acyclic graphs

### 24.3 Dijkstra's algorithm

### 24.4 Difference constraints and shortest paht

### 24.5 Proofs of shortest-paths properties