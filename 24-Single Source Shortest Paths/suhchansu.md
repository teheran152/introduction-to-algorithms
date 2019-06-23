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
- solves the single source shortest paths in which edge weights may be negative 
  - returns a bool indicating a negative weight cycle from the source
  - if cycle != nil > no solution
  - if cycle == nil > shortest paths
- uses relaxes edges, progressively decreasing v.d

``` Swift
func bellmanFord(G, w, s) {
  // initializing d, pi vlaues of all vertices
  // Bictheta(V)
  initializeSingleSource(G, s)

  // BicTheta(E)
  for i in (1..<(|G.V|-1)) {
    for edge(u, v) in G.E {
      relax(u, v, w)
    }
  }

  // check negative weight cycle, return bool
  // BicO(E)
  for edge(u, v) in G.E {
    if v.d > (u.d + w(u, v)) {
      return false
    }
  }
  return true

  // runs in time BicO(VE)
}
```

#### Lemma 24.2
- let G = contains no negative weight cycles
- after relax all edges, v.d == delta(s, v) in vertices

#### Proof
- path relaxation property
- v.d == v_k.d == delta(s, v_k) == delta(s, v)

### 24.2 Single-source shortes paths in directed acyclic graphs
- relaxing edges of weighted dag (directed acyclic graph)
  - uses topological sort > sortest paths in BicTheta(V + E)

``` Swift
func dagShortestPaths(G, w, s) {
  // BicTheta(V + E)
  topologicallySort(G)

  // BicTheta(V)
  initializeSingleSource(G, s)

  // G.V is topologically sorted order
  // each loop Theta(1)
  for vertex u in G.V {
    for vertex v in G.adj[u] {
      realx(u, v, w)
    }
  }

  // runs in BicTheta(V + E)
}
```

#### Theorem 24.5
- let G = weighted, directed, no cycles
  - dagShortestPaths(:) procedure
  - v.d = delta(s, v)
  - prdecessor subgraph G_pi = shortes path tree

#### Proof
- v.d = delta(s, v)
- v is not reachable form s
- v.d == delta(s, v) == infinity (no path property)
- let v = reachable from s
- let p = [v_0, v_1 ... v_k]
- path relaxation path peroperty > v_i.d = delta(s, v_i)
- by the predecessor subgraph property, G.pi is shortes paths tree 

### 24.3 Dijkstra's algorithm
- solves the single source shortest paths
  - weighted
  - dircted graph
  - all edges are nonnegative
- lower running time than Bellan Ford
- uses a min priority queue Q

``` Swift
func dijstra(G, w, s) {
  // initializes d, pi values
  initializeSingleSource(G, s)

  let S = emptySet
  let Q = G.V

  // extracts a vertex u from Q = V - S
  // runs in |V|
  while (Q != emptySet) {
    let u = extractMin(Q)

    // adds extracted vertex in S > maintaining the invariant
    // vertex u has the smallest shortest path in V - S
    let S = union(S, {u})

    // updating v.d, predecessor v.pi
    for vertex in G.Adj[u] {
      relax(u, v, w)
    }
  }
}

```

#### Theorem 24.6(Correctness of Dijkstra's algorithm)
- weighted
- dircted graph
- all edges are nonnegative
- u.d == delta(s, u) in all vertices

#### Proof
- use loop invariant, while(Q != emptySet) { }

#### Initialization
- let S = emptySet > invariant is trivially true

#### Corollary 24.7
- Dijkstra algorithm 
  - directed graph
  - w : nonnegative weight
  - s : source
- predecessor subgraph G_pi : shortest paths tree

#### Proof
- uses Theorem 24.6, predecessor subgraph property

#### Analysis
- How fast Dijkstra?
  - insert()
  - extractMin()
  - decreseKey()
- insert(), extractMin() ared called once per vertex
  - each vertex is added to set S
  - each edge in Adj[u] is examined for loop 
  - total edges : |E|
  - total for loop time : |E|
- Dijkstra's running time depends on how to imprement minPriorityQueue

### 24.4 Difference constraints and shortest paht
- shows special case of linear programming

#### Linear programming
- general linear programming does not always run in polynominal time
- two reasons
  - cast a given problem as a polynomial size
  - there is fater algorithm for many case
    - single path shortest path problem
    - maximum flow problem
- there is a feasible solution

#### Systems of differece constraints
- each row A contains 1 and more 
- not A is 0

#### Lemma 24.8 
- let x = solution of diffrence constraints
- let d = any constant
- x + d = (x_1 + d, x_2 + d, ..., x_n + d)

#### Proof
- (x_j + d) - (x_i + d) = x_j - x_i
- if x satisfies Ax <= b
- x + d

#### Constraint graphs
- constraint graph gas additional vertex v_0

### 24.5 Proofs of shortest-paths properties
- prove properties
  - triangle inequality
  - upper bound
  - no path
  - covergence
  - path realxation
  - predecessor subgraph
