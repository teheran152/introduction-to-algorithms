## Data Structures for Disjoint Sets
#### Disjoint-set operations
- representative에 의해 각 SET을 구분
- 어떤 구성원이 representative로 사용되는지 중요하지 않는다면 set을 조정하지 않는다면 
set의 대표자를 두번 물으면 동일한 답을 얻을수 있다.

- MAKE-SET(x) : create a new set whose only member is x.
- UNION(x, y): x, y를 합치는 operation.합쳐진 set의 대표자는 Sx, Sy의 집합중 하나의 멤버가 된다.
- FIND-SET(x): x가 들어있는 set을 return

- UNION을 하면서 set을 합치고 지우기 때문에 최종적으로 최대 n-1개 만큼 돈다.
- MAKE-SET의 operations는 전체 operation(MAKE-SET, UNION, FIND-SET)수 안에 포함된다. m >=n일때 MAKE-SET 처음 n번 실행

##### An application of disjoint-set data structures
- unidirect graph에서 많이 사용
```
CONNECTED-COMPONENTS(G) 
for each vertex v ∈ G:V 
  MAKE-SET(v) 
for each edge (u, v) ∈ G:E 
    if FIND-SET(u) != FIND-SET(v) 
        UNION(u,v)

SAME-COMPONENT(u,v) 
if FIND-SET(u) == FIND-SET(v) 
    return TRUE 
else return FALSE
```

#### Linked-list representation of disjoint sets
- Linked-list로 disjoint set을 만들었을때 
    - MAKE-SET, FIND-SET은 O(1)
- Linked-list로 만들면 원소들을 가르키는 ```head```,```tail```가 있는 point 변수가 존재
- UNION(x, y)시에 tail pointer를 사용해서 x의 리스트의 어디에 붙일지 빠르게 찾을수 있지만 실재로는 BigTheta(n^2)만큼 걸린다.
    - 합쳐지는 set의 모든 원소의 update가 필요하다.

- MAKE-SET을 n 번하고 각각 원소들을 하나씩 UNION을 n-1하면 2n-1번 걸리기 때문에 Big theta (n)이 된다. 

##### A weighted-union heuristic
- UNION은 worst-case의 경우 Bit theta (n)걸린다.
- 엄청 긴 list를 아주 짧은 list에 치는 케이스

- 해결방법으로는 짧은 list가 긴리스트랑 합쳐지게 조정

#### Disjoint-set forests
- 더 빠른 속도를 내기 위해서 rooted tree집합
- 각 노드들은 하나의 멤버를 가지고 있고 그 것은 집합을 대표
- 각 트리의 root는 대표

##### Heutistics to improve the running time
- 유니온 바이 랭크union by rank방법: 항상 작은 트리를 큰 트리 루트에 붙이는 방법 트리의 깊이(depth)가 실행 시간에 영향을 주기 때문에, 깊이가 적은 트리를 깊이가 더 깊은 트리의 루트 아래에 추가그러면 두 트리의 깊이가 같을 경우에만 깊이가 증가
- path compression)으로 파인드 연산을 수행 할 때마다 트리의 구조를 평평하게 만드는 방법. 이 방법은 루트 노드까지 순회중 방문한 각 노드들이 직접 루트 노드를 가리키도록 갱신하는 것으로, 모든 노드들은 같은 대표 노드를 공유

