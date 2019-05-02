## Fibonacci Heaps
- fibonacci heap 은 최소-힙 특성을 만족하는 트리들을 모아 놓은 것 > 최소 키가 항상 root 에 있음
- 목적
  - 'mergeable heap' 의 몇가지 연산 지원
  - 분할 상환을 빈번히 수행하는 application 에 적합한 constant amortized running time 을 가짐

#### Mergeable heaps
- mergeable heap 은 5가지 operations 들을 가짐
  - makeHeap() -> return a new heap
  - insert(heap, element)
  - minimum(heap) -> return a pointer to element whose key is minimum
  - extractMin(heap) -> return a pointer, delete the element whose key is minimum
  - union(heapA, heapB) -> return a new heap that contains both heaps

- fibonacci heaps 위 5가지에 2가지 operations 들을 더 가짐
  - decreseKey(heap, element, key) heap 에 key 를 가진 element 를 assign 함
  - delete(heap, element) 
- binary heap 과 fibonacci heap 의 7가지 operations 들의 time complexity 비교
  - fibonacci heap 은 extractMin, delete 을 제외한 operations 에서 amortized time 이 constant 인 Theta(1)

#### Fibonacci heaps in theory and practice
- a theoretical standpoint 
  - graph 를 활용하는 몇가지 algoritms 에서는 extractMin, delete. operations 이 Theta(1) 이냐 Theta(log n) 이냐 에 따라서 amortized time 을 개선 가능
    - computing minimum spanning trees (Chapter 23)
    - finding single source shortest paths (Chapter 24)
    - dijkstra. 
- a practical point
  - finonacci 구현이 복잡함

### 19.1 Structure of Fibonacci heaps
- fibonacci heap 은 min-heap ordered 된 tree 들의 collection
- node x 는 x 의 parent 와 child 의 pointer 를 가짐 > circular, doubly linked list
- circular, doubly linked list 의 advantages
  - circular 안에서 어디든지 insert/remove 가능
  - 2 개의 list 를 결합/분리를 BicO(1) 에 가능
- 각 node 는 2개의 attributes 를 가짐
  - degree : store the number of children
  - mark : lost a child

#### Potential function
- the sum of the potentials of its constituent fibonacci heaps
- Potential(heap) = t(number of trees in heap) + 2m(number of marked nodes)

### 19.2 Mergeable-heap operations

#### Creating a new Fibonacci heap
- newHeap.n = 0
- newHeap.min = nil
- no trees in newHeap
  - newHeap.t = 0 (number of trees)
  - newHeap.m = 0 (number of marked nodes)
- Potential(newHeap) = 0
- The amortized cost = BicO(1)

#### Inserting a node
- pseudo code 와 설명
  - initialize a new node
  - if heap.min == nil 
    - create a root list for heap containing x
    - heap.min = x
  - else 
    - if x.key < heap.min.key
      - heap.min = x
  - heap.n = heap.n + 1
- The amortized cost is BicO(1)

#### Finding the minimum node
- heap.min pointer is the minimum node
- The azmortized cost is BicO(1)

#### Uniting two Fibonacci heaps
- pseudo code 와 설명
  - create a new heap
  - newHeap.min = heapA.min
  - concatenate the root lists of heapA, heapB into a new root list newHeap
  - if (heapA.min == nil) || (heapB.min != nil, heapB.min.key < heapA.min.key)
    - newHeap.min = heapB.min
  - newHeap.n = heapA.n + heapB.n
  - return newHeap
- The armortized cost is BicO(1)

#### Extracting the minimum node
- The most complicatd opertaion
- pseudo code 와 설명
  - z = heap.min
  - if z != nil
    - for _ in z.child
      - add x to the root list of heap
      - x.p = nil
    - remove z from the root list of heap
    - if z == z.right
      - heap.min = nil
    - else
      - heap.min == z.right
      - consolidate(heap)
    - heap.n = heap.n - 1
  - return z
- consolidate(heap) is reducing the number of trees
  - find two roots x, y in same degree
  - link y to x : increse a x.degree, clear the makr of y
    - remove y
    - make y a chiild of x
- consolidate(heap) 의 pseudo code 와 설명
  - 길어서 책 참조 (516 page)
  - for 문과 axiliary array 활용해서 tree 들을 rearrangement 함
- The amortized cost is BicO(log n)

### 19.3 Decreasing a Key and deleting a node

#### Decreasing a key
- decreseKey(heap, element x, key) 의 pseudo code 와 설명
  - if k > x.key
    - error
  - x.key = k
  - y = x.p
  - if (y != nil) && (x.key < y.key)
    - cut(heap, x, y)
    - cascadingCut(heap, y)
  - if x.key < heap.min.key
    - heap.min = x
- cut(heap, element x, element y) 의 pseudo code 와 설명
  - remove x from the child list of y
  - decrementing y.degree
  - x.p = nil
  - x.mark = false
- cascadingCut(heap, element y) 의 pseudo code 와 설명
  - z = y.p
  - if z != nil
    - if y.mark == false
      - y.mark = true
    - else cut(heap, y, z)
      - cascadingCut(heap, z)
- The amortized cost is BicO(1)

#### Deleting a node
- pseudo code 와 설명
  - decreaseKey(heap, element x, -infinite)
  - extractMin(heap)
- The amortized cost is BicO(log n)

### 19.4 Bounding the maximum degree
- prove why extractMin(), delete() is BicO(log n)
- 증명 ...ㅎ 