## Maximum flow
#### 26.1 Flow networks
##### Flow networks and flows
- weight가 있는 directed graph G와 source(시작) 노드 s, sink(도착) t가 주어 졌을때 각 edge의 capacity를 고려하여 s -> t로 흘려 보낼수 있는 maximum flow를 구하는 알고리즘

- G의 각 엣지 가중치를 capacity라고 하며 c(u, v)로 씀
- u, v사의 net flow를 f(u, v)라고 하며 real-valed function

- 2가지 제약 조건이 있다.
    - Capacity contraint: For all u,v in V, require 0 <= f(u, v) <= c(u, v)
    - Flow conservation: For all u in V - {s, t}, we require Sigma(v in V)f(v, u) = Sigma(v in V)f(u, v)

##### Modeling problems with antiparallel edges
- if edge(v1, v2) in E이고 !((v2, v1) in E)라면 (v1, v2), (v2, v1)을 antiparallel이라고 한다.
- antiparallel edges를 통해 flow 문제를 풀때는 no antiparallel edgeㄹ로 바꿔야 한다.

##### Networks with multiple sources and sinks
- source와 sink가 하나인게 아니라 여러개일수 있다.
- 이문제를 해결하기 위해서 supersoruce와 super sink를 추가하여 문제를 해결한다.

#### 26.2 The Ford-Fulkerson method
- 알고리즘이 아닌 method라 부르는건 서로다른 러닝타임들을 가진 구현체들을 합쳐서 만들어졌기 때문
- 3가지 중요한 ideas들을 기반으로 만들어졌다.
    - redisual networks
    - augmenting paths
    - cuts
```
FORD-FULKERSON-METHOD(G,s,t) 
1 initialize ﬂow f to 0 
2 while there exists an augmenting path p in the residual network Gf 
3   augment ﬂow f along p 
4 return f
```

##### Redisual networks
- G와 f가 주어지면 residual network Gf는 capacity를 구성하는데 구성하면서 G의 edge들의 flow를 어떻게 바꾸는지 결정

- cf(u, v)
    - = c(u, v) - f(u, v) if(u, v) in E,
    - = f(v, u)           if(v, u) in E,
    - = 0                 otherwise.

##### Aumenting paths
- augmenting path p는 residual network Gf안의 s와 t사의 단순한 path.
- cf(p) = min{cf(u, v) : (u, v) is on p}.

##### Cuts of flow networks
- 기본적인 아이디어는 augmenting path 를 찾으면서 flow 값을 증가시키는 것이다.
- The max-flow min-cut theorem 을 가지고 maximum flow를 찾는다.

- minimum cut은 s와 t사이를 최소 capacity를 가지는 상태로 cut하여 서로 다른 집합으로 나누는 것

##### The basic Ford-Fulkerson alogorithm
```
FORD-FULKERSON(G,s,t) 
1 for each edge (u,v) in G.E 
2   (u,v).f = 0 
3 while there exists a path p from s to t in the residual network Gf 
4   cf (p) = min{cf (u,v) : (u,v) is in p}
5   for each edge (u,v) in p 
6       if (u,v) in E 
7           (u,v).f = (u,v).f + cf (p) 
8       else (v,u).f = (v,u).f - cf (p)
```
##### Analysis of Ford-Fulkerson
- residual network Big O(V + E') = O(E)
- While문은 Big O(E)
- Total Big O (E |f*|).
-  the capacities are integral and the optimal ﬂow value |f*|is small이면 good 하지만 커지면 좋지안다. 

##### The Edmonds-Karp alogorithm
- BFS로 찾는 구간인 line 3번의 augmenting path p 를 찾음으로서 ford fulkerson향상시킬수 있다.

- Big O(VE^2)time이 걸린다.

##### 26.3 Maximum bipartite matching
