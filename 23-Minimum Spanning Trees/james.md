## Minimum Spanning Tree
- undirected graph G  = (V, E)
 - V는 pins의 집합
 - E는 각 핀을 연결해 준는 선들의 집합
 - (u, v) in E, weight가 있다면 w(u, v) in E
- acycle한 subset을 찾을려면 아래 수식을 만족하면 최소 가중치를 얻는다.
(수식)
-  T가 acyclic하고 모든 vertics들이 연결 됬다면, Tree다
 - 이걸 spanning tree라고 부른다.

#### 23.1 Growing a minimum spanning tree
- greedy 전략으로 접근 (generic method)
- 한 타임마다 mst(minimum spanning tree)의 엣지를 잇는다.
- generic method는 loop를 maintainggkrh  edges의 A 집합을 관리
 - Prior to each iteration, A is a subset of some mst
```
GENERIC-MST(G, w)
1 A = null 
2 while A does not form a spanning tree 
3  find an edge(u, v) that is safe for A
4  A = A U {(u, v)}
5 return A
```
- **Initialization**: After line 1, the set A tribially satisties the loop invariant.
- **Maintenance**: The loop in lines 2-4 maintains the invariant by adding only safe edges.
- **Termination**: All edges added to A are in a minimum spanning tree, and so the set A returned inline 5 must be a minimum spanning tree.

- 증명....

#### 23.2 The algotithms of Kruskal and Prim
##### Kruskal's algorithm
-  safe edge를 찾는 알고리즘
- Kruskal의 알고리즘은 포레스트에 있는 두 개의 나무를 연결하는 모든 가장자리 중에서 최소한의 무게의 가장자리 U를 발견하여 증가하는 숲에 추가 할 안전한 가장자리를 찾음.
- disjint-set data structure를 사용.

```
MST-KRUSKAL(G, w) 
1 A = null
2 for each vertex v in G.V 
3  MAKE-SET(v) // 21.1 section에 나오는 method
4 sort the edges of G.E into nondecreasing order by weight w 
5 for each edge (u, v) in G.E, taken in nondecreasing order by weight 
6  if FIND-SET(u) != FIND-SET(v)
7   A = A sum {(u, v)}
8   UNION(u, v) 
9 return A
```

- running time
 - O(ElgE)

 ##### Prim's algorithm
 - Dikstra's algorithm과 비슷
 - 항상 single  tree 형태를 한 집합의 edges들을 정보인 property를 가지고 있다.

```
MST-PRIM(G,w,r) 
1 for each u in G.V 
2   u.key = Infinity
3   u.phi D NIL
4 r.key = 0 
5 Q = G.V 
6 while Q != null
7   u = EXTRACT-MIN(Q)
8   for each v 2 G.Adj[u]
9     if v in Q and w(u, v) < v.key
10      v.phi = u
11      v.key = w(u, v)
```
- running time: O(VlgV +  ElgV) = Big O(ElgV)
