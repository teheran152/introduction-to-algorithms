## Maximum Flow
- Flow networks model
  - linquids flowing through pipes
  - electrical networks
  - communication networks
- flow conservation : equal the rate enters, leaves
  - Kirchhoff's current law

#### Chapter outline
- 26.1 : formalize the notions of flow networks
- 26.2 : `Ford, Fulkerson` algorithms for finding maximum-flow
- 26.3 : finding maximum matching undirected bitpartile graph 
- 26.4 : `push-relabel` algorithm for network-flow
- 26.5 : using `relabel-to-front` technique in `push-relabel` run in BicO(V^3)

### 26.1 Flow networks

#### Flow networks and flows
- directed graph, nonnegative capacity c(u, v) >= 0
- two vertices in a flow network
  - s : source
  - t : sink
- two properties
  - capacity constraint : 0 <= f(u, v) <= c(u, v)
  - flow conservation 
- nonnegative quantity f(u, v) : from u to v
  - |f| = Sigma(f(s, v)) - Sigma(f(v, s))

#### An example of flow
- factory to the warehouse example
  - capacity constraint : care only leaving and arriving p per day
  - flow conservation : care equal rate, if not crates would accumulate

#### Modeling problems with antiparallel edges
- antiparallel
  - edge(v1, v2) subset E && edge(v2, v1) not subset E
  - (v1, v2), (v2, v1) is antiparallel
- antiparallel edge (v1, v2) > ((v1, v`), (v`, v2))
  - set original edge capacity to both new edges 

#### Networks with multiple sources and sinks
- adding supersource s supersink t
- shows in detail at 26.3

### 26.2 The Ford-Fulkerson method
- call 'method' rather than 'algorithm' : encompasses several implementations with differing running times
- three ideas
  - residual netwroks
  - augumenting paths
  - cuts
- iteratively increase the value of the flow
  - At each iteration, increase the flow value by finding an `augmenting path` in an associated `residual network`
  - repeatedly augment the flow until the residual network has no more augmenting paths

``` Swift
func fordFulkersonMethod(G, s, t) {
  // flow f to 0
  initialize(f)

  var f: Flow
  // exists an augmenting path p in the residual network G_f
  while(isExits(p, G_f)) {
    // augment flow f along p
    augmentFlow(f, p)
  }

  return f
}
```

#### Residual networks
- residual network Gf
  - consists edges : can change the flow on edges of G
  - additional flow == edge’s capacity - edge flow
  - if positive, residual capacity : c_f(u, v) = c(u, v) - f(u, v)
  - can admit more flow : (u, v) == c_f(u, v) = 0
- G_f contain edges but not in G
- for increasing the total flow, decrease a particular edge
  - positive flow f(u, v)
    - place edge(v, u) in G_f with c_f(v, u) == f(u, v)
    - can admit flow in the opposite direction to (u, v)
  - can cancel out of flow (u, v)
- c_f(u, v)
  - c(u,v) - f(u,v) 
    - if (u, v) subset E
  - f(v, u) 
    - if (v, u) subset E
  - 0


### 26.3 Maximum bipartite matching

### 26.4 Push-relabel algorithms

### 26.5 The relabel-to-front algorithm
