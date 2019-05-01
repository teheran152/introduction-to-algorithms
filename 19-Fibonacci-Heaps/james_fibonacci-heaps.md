## Fibonacci heap
#### Structure of Fibonacci heap
- a collection of min-heap-ordered trees
- parent pointer p[x], child pointer child[x]를 가지고 있음
- children of x는 서로를 circular, doubly linked list(child list of x라 부름)
- 각 child는 (left[x], right[y])를 가지고 있다. 
- 만약 노드 y사 하나의 자식만 가지고 있다면 left[y] = right[y] = y이다.

- Circular, doubly linked lists 2가지 이점
  - remove node시 O(1)시간이 걸림
  - concat시 O(1)시간이 걸림

- degree[x]는 node x의 자식 수가 저장됨
- mark[x]는 x가 다른 노드의 자식이 된 이후 x노드가 자식을 잃어 버렸는지 여부
- 새로이 만들어졌다면 un marked, 도따른 노드의 자식이 만들어질경우 unmarked

- Fibonacci Heap H는 pointer min[H]에 의해 접근이 가능하고 root에는 minimum key를 포함

##### Potential function
- 피보나치 힙의 잠재 비용
(H) = t(H) + 2m(H)

- t는 피보나치 힙 내의 트리수
- m은 mark된 노드의 수

##### Maximum degree
- amortized analyses는 D(n) upper bound로 알려져 있다.(노드의 maximum degree)


#### Mergeable-heap operations
- Merageble heap operation은 가능한한 미뤄두는것을 의미 
- 다양한 operations들은 trade-off가 있다.(insert add등)
- EXTRACT-MIN을 한다면 모든 노드들을 돌면서 최소 노드를 찾기 위해통합하는 작업이 생김

##### Creating a new Fibonacci heap
- H 객체를 return(비어있는 heap을 만들때)
- H.n = 0, H.min = null;
- t(H)) = 0, m(H) = 0이기 때문에 potential function에 의하면 0이나옴 그래서 O(1)이 나옴)

##### Insering a node
```
FIB-HEAP-INSERT(H, x)
x.degree = 0
x.p = NIL
x.child = NIL
x.mark = FALSE
if H.min == NIL
 create a root list for H containing just x
 H.min = x
else insert x into H’s root list
  if x.key < H.min.key
    H.min = x
H.n = H.n + 1
``` 
- O(1) 걸림

##### Finding the minimum node
- H.min에 있기 때문에 O(1)이 걸림

##### Uniting two Fibonacci heap
```
FIB-HEAP-UNION(H1, H2)
H = MAKE-FIB-HEAP()
H.min = H1.min
concatenate the root list of H2 with the root list of H
if (H1.min == NIL) or (H2.min != NIL and H2.min.key < H1.min.key)
  H.min \ H2.min
H.n = H1.n + H2.n
return H
```
- O(1) 걸림)

##### Extracting the minimum node
```
FIB-HEAP-EXTRACT-MIN(H)
z = H.min
if z != NIL
  for each child x of z
    add x to the root list of H
    x.p = NIL
  remove ´ from the root list of H

  if z == z.right
    H.min = NIL
  else H.min = z.right
    CONSOLIDATE(H)
  H.n = H.n - 1
return z
```

```
CONSOLIDATE(H)
let A[0 . . D(H.n)] be a new array
for i = 0 to D(H.n)
    A[i] = NIL
for each node w in the root list of H
    x = w
    d = x.degree
    while A[d] != NIL
        y = A[d]    // another no=e with the same degree as x
        if x.key > y.key
            exchange x with y
        FIB-HEAP-LINK(H, y, x)
        A[d] = NIL
        d = d + 1
    A[d] = x
H.min = NIL
for i = 0 to D(H.n)
    if A[i] != NIL
        if H.min == NIL
            create a root list for H containing just A[i]
            H.min = A[i]
        else insert A[i] into H’s root list
            if A[i].key < H.min.key
                H.min = A[i]
```
- amortized cost of extracting the minimum node O(lgN)

#### Decreasing a key and deleting a node
##### Decreasing a key
```
FIB-HEAP-DECREASE-KEY(H, x, k)
if k > x.key
    error “new key is greater than current key”
x.key = k
y = x.p
if y != NIL and x.key < y.key
    CUT(H, x, y)
    CASCADING-CUT(H, y)
if x.key < H.min.key
    H.min = x

CUT(H, x, y)
remove x from the child list of y, decrementing y.degree
add x to the root list of H
x.p = NIL
x.mark = FALSE

CASCADING-CUT(H, y)
z = y.p
if z != NIL
    if y.mark == FALSE
        y.mark = TRUE
    else CUT(H, y, z)
        CASCADING-CUT(H, z)
```
- O(1) 걸림

##### Deleting a node
```
FIB-HEAP-DELETE(H, x)
  FIB-HEAP-DECREASE-KEY(H, x, -무한대)
  FIB-HEAP-EXTRACT-MIN(H)
```
