## Elementary Graph Algorithms
- searching a graph : following the edges to visit the vertices

### 22.1 Representations of graphs
- G = (V, E) 
  - adjacency lists
    - sparse graph
    - weighted graph
    - undirected graph 
      - stores edge twice
      - N lists, N arrays, 2E nodes
  - adjacency matrix
    - dense graph
    - requires Theta(V^2) memory
    - undirected graph
      - (u, v) == (v, u) same edge, symmetry 

#### Representing attributes
- need to maintain vertex, edges attributes for graph
- implemeting these in real program, differs for languages
- 저장을 2번 하게되는점, Vertex class 가 기본으로 제공될땐 subclass 해야하는 점 등등

### 22.2 Breadth-first search
- Prims's minimum-spanning-tree
- Dijkstra's simple-source shortes-paths
- uses Queue for saving visited vertices
- BFS psedocode, 그림과 설명 

#### Analysis
- enqueueing, dequeueing is BicO(1)
- total queueing is BicO(V)
- the sum of the lengths of adjacency lists is Theta(E)
- scanning adjacency lists is BicO(E)
- overhead initialization is BicO(V)
- BFS total time is BicO(V + E)

#### Shortest paths
- Lemma, Proof ...

### 22.3 Depth-first search
- uses `backtracks` to explore all vertices
- uses timestamps for `backtracks` that visited vertices
- DFS psedocode, 그림과 설명

#### Properties of depth-first search
- characterize the parenthesis structure
- Theorem, Proof.. 

#### Classification of edges
- directed graph is acyclic if DFS don't have Back edges
- Tree edges : edges in dfs tree
- Back edges : connecting a vertex u to an ancestor v (u, v)
- Forward edges : connecting a vertex u to an descendant v (u, v) but not tree edge
- Cross edges : all other edges

### 22.4 Topological sort
- dag == directed acyclic graph 
- `Professor Bumstead` 옷입는 방법 예시
- psedocode 와 그림과 설명
  - backtracts > push stack > finish dfs > print stack

### 22.5 Strongly connected components
- how to decomposing graph to scc
  - vertices u, v in C
  - u -> v, v -> u
  - u, v are reachable from each other
- a partition into subgraphs that are themselves strongly connected
- Theta(V + E)