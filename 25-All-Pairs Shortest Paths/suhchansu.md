## All-Pairs Shortest Paths
- need shortest path weights, predecessor matrix (pi_ij)

``` Swift
func printAllPairsShortestPath(pi, i, j) {
  if i == j {
    print i
  } else if pi_ij == nil {
    print "no path"
  } else {
    printAllPairsShortestPath(pi, i, pi_ij)
    print j
  }
}
```

#### Chapter outline
- 25.1 : presents a dynamic programming
  - using technique, repeated squaring
  - BicTheta(V^3 lg V)
- 25.2 : gives another dp
  - Floyd-Warshall algorithm
  - BicTheta(V^3)
- 25.3 : Johnson's algorithm
  - BicO(V^2 lg V + VE)

### 25.1 Shortest paths and matrix multiplication
- dynamic programming algorithm for the all-pairs shortest paths
- operation in loop is similar to matrix multiplication
- briefly recap dynamic programming 
  - Characterize the strcture of an optimal solution
  - Recursively define the value
  - Compute the value of an optimal solution in a bottom-up fashion

#### The structure of a shortest path
- start by characterizing the structure of an optimal solution

#### A recursive solution to the all-pairs shortest-paths problem
- if i == j { return 0 }
- if j != j { return infinity }

#### Computing the shortest-path weights bottom up
``` Swift
func extendShortestPaths(L, W) {
  let n = L.rows
  var primeL = matrix()

  for i in n {
    for j in n {
      let primel_ij = infinity
      for k in n {
        let primel = min(primel_ij. primel_ij + w_kj)
      }
    }
  }
  return primeL
}

func squareMatrixMultiply(A, B) {
  let n = A.rows
  let C = matrx(n, n)

  for i in n {
    for j in n {
      var c_ij = 0
      for k in n {
        c_ij += (a_ik * b_kj)
      }
    }
  }
}

func slowAllPairsShortestPaths(W) {
  let n = W.rows
  let L = W

  for m in 2 ... n - 1 {
    var L = matrix(n, n)
    L = extendShortestPahts(L, W)
  }
  return L
}
```

#### Improving the running time
```
func fasterAllPairsShortestPaths(W) {
  let n = W.rows
  let L(1) = W
  let m = 1

  while (m < n - 1) {
    let L(2m) = matrix(n, n)
    L(2m) = extendShortestPath(L(m), L(m))
    m = 2m
  }
  return L(m)
}
```

### 25.2 The Floyd-Warshall algorithm

#### The structure of a shortest path
- consider the intermediate vertices of a shortest path in intermediate vertex

#### A recursive solution to the all-pairs shortest-paths problem
- let d(k)_ij is the weight of a shortest path from i to j
  - if k == 0 { return w_ij }
  - if k >= 1 { return min(d(k-1)_ij, d(k-1)_ik + d(k-1_kj))}

#### Computing the shortest-path weights bottom up
``` Swift
func floydWarshall(W) {
  let n = W.rows
  let D(0) = W
  for k in n {
    let D(k) = matrix(n, n) 
    for i in n {
      for j in n {
        let d(k)_ij = min(d(k-1)_ij, d(k-1)_ik + d(k-1_kj)
      }
    }
  }
  return D(n)
}
```

#### Constructing a shortest path
- ...

#### Transitive closure of a directed graph
``` Swift
func transitiveClosure(G) {
  let n = G.V
  let T0 = matrix(n, n)

  for i in n {
    for j in n {
      if i == j {
        t_ij = 1
      } else {
        t_ij = 0
      }
    }
  } 

  for k in n {
    let T = matrix(n, n)
    for i in n {
      for j in n {
        t_ij = Union(t_ij, Intersection(t_ik, t_kj))
      }
    }
  }
  return Tn
}
```

### 25.3 Johnson's algorithm for sparse graphs
- faster than either repeated wuaring of matrix, Floyd-Warshall
- uses as subroutins both Dijkstra, Bellman-Ford
- uses thechnique 'reweighting'
- new set of edge weights w^ must satisfy two properties
  - u, v, path p is shortest path from u to v using weight function w, if p is also shortest path from u to v using weight function w^
  - new weight w^ is nonnegative

#### Preserving shortest paths by reweighting
- how easily reweight the deges satisfy the first property abobe

#### Lemma 25.1 (Reweighting does not change shortest paths)
- w^(u, v) = w(u, v) + h(u) - h(v)

#### Proof
- w^(p) = w(p) + h(v_0) - h(v_k)
- h(v_0), h(v_k) do not depend on the path

#### Producing nonnegative weights by reweighting
- we want w^(u, v) is nonnegative for all edges
- if define w^ by reweighting according to equation, satisfy the second property

#### Computing all-pairs shortest-paths
- uses the Bellman-Ford, Dijkstra as subroutines
- implicitly that the edges are sorted in adjacency lists
- return |V| x |V| matrix 
- assmue the vertices are 1 to |V|

``` Swift
func johnson(G, w) {
  let G' = compute(G)

  if !bellmanFord(V', w, s) {
    print("the input graph contains a negative-weight cycle")
  } else {
    for v in G'.V {
      set h(v) to the value of delta(s, v)
      compted by Bellman-Ford 
    }

    for edge in G'.E {
      let w^(u, v) = w(u, v) + h(u) - h(v) 
    }

    let D = matrix(n, n)
    for u in G.V {
      run dijkstra(G, w^, u)
      for vertex in G.V {
        let d_uv = delta^(u, v) + h(v) - h(u)
      }
    }
  }
}

// if implementation min-priority queue 
// by Fibonacci heap: BicO(V^2 lg V + VE)
// by binary min heap: BicO(VE lg V)
```