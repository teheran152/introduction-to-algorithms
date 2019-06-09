## Elementary Graph Algorithms
- Searching a graph means systematically following the edges of the graph so as to visit the vertices of the graph.

#### 22.1 Representations of graphs
- ```G = ( V, E )```
  - adajacency lists or adjacency matrix
- Directed and undircted graphs
- Adajacency lists가  sparse graphs이기 때문에 list 형태로 자료구조를 선택해서 사용하고 있음
  - sparse graphs ( |E| <= |V|^2 )

- 하지만 이 책에서는 adjacency-matrix representation을 선호하고 있음 
  - graph가 dense할때 ( |E| is close to |V|^2 )
  - edge가 서로 connect 되어있는지 빠른판단을 할때

- list로 표현시 directed graph의 경우 list의 lengths의 합은 |E|
- list로 표현시 undirected graph의 경우 list의 lengths의 합은 2|E|
- 메모리는 Big Theta(V + E)
- Weighted graphs 표현시 w(u, v)로 쉽게 표현이 가능하다.
- list는 표현은 많은 다른 그래프 변형을 지원하도록 수정할 수 있다는 점에서 매우 강력
- 하지만 list에서 vertex를 검색하는것보다 엣지가 존재하는지 안하는지를 판단하는게 느리다.

- Matrix는 list의 그런 단점을 커버해 준다. 하지만 메모리가 더 든다.
- 특히 unweighted일때는 matrix가 더 장점이 많다.


#### 22.2 Breadth-first search
- vertex를 발견하기 위해 G의 edge들을 다 탐색

```
BFS(G,s) 
1 for each vertex u in G.V - {s}
2   u.color = WHITE 
3   u.d = Infinity 
4   u.phi = NIL 
5 s.color = GRAY 
6 s.d = 0 
7 s.phi = NIL 
8 Q = undefined 
9 ENQUEUE(Q,s) 
10 while Q != undefined, 
11  u = DEQUEUE(Q) 
12  for each v 2 G.Adj[u] 
13      if v.color == WHITE 
14          v.color = GRAY 
15          v.d = u.d + 1 
16          v.phi = u 
17          ENQUEUE(Q,v) 
18      u.color = BLACK
```
- running time Big O (V+E)

#### 22.3 Depth first search
- to search “deeper” in the graph whenever possible
```
DFS(G) 
1 for each vertex u in G.V 
2   u.color = WHITE 
3   u. = NIL 
4 time = 0 
5 for each vertex u 2 G.V 
6   if u.color ==WHITE 
7       DFS-VISIT(G,u)

DFS-VISIT(G,u) 
1 time = time + 1 // white vertex u has just been discovered 
2 u.d = time 
3 u.color = GRAY 
4 for each v 2 G.Adj[u] // explore edge (u,v) 
5   if v.color == WHITE 
6       v.phi = u 
7       DFS-VISIT(G,v) 
8 u.color = BLACK // blacken u, it is ﬁnished 
9 time = timeC1
10 u.f = time 
```
- running time Big theta (V+E)

#### 22.4 Topological sort
- depth-first search를 써서 topological sort를 하는 (directed acyclic graph or dag)
- Topological sort란 어떤일을 하는 순서를 찾는 알고리즘
- 즉 directed graph의 존재하는 각 정점들의 선행 순서를 위배하지 않으면서 모든 정점을 나열하는것

```
TOPOLOGICAL-SORT(G)
1 call DFS(G) to compute ﬁnishing times v.f for each vertex v 
2 as each vertex is ﬁnished, insert it onto the front of a linked list 
3 return the linked list of vertices
```
- Big theta (V+E)

#### 22.5 Strongly connected components
- 두 정점 u,v사이의 u->v경로와 v->u 경로가 항상 존재
- u와 외부의 임의의 정점 v 사이에 u -> v 경로와 v -> u경로가 동시에 존재하는 경우는 없다.
- 내부의 모든 정점에서 내부의 다른 모든 정점으로 갈수 있는 최대 그룹
```
STRONGLY-CONNECTED-COMPONENTS(G) 
1 call DFS(G) to compute ﬁnishing times u.f for each vertex u 
2 compute G^T 
3 call DFS(G^T), but in the main loop of DFS, consider the vertices in order of decreasing u.f (as computed in line 1) 
4 output the vertices of each tree in the depth-ﬁrst forest formed in line 3 as a separate strongly connected component 
```
