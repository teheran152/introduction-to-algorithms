#### Data structures on secondary storage
- disks much slower because they have moving mechanical parts
  - platter ratation
  - arm movement
- 대량의 데이터를 처리할때 하나의 노드에 많은 데이터를 가지면 장점
- 대량의 데이터는 메모리 보다는 디스크에 저장되는데 이때 블록 단위로 I/O 발생
- 하나의 노드를 블록의 크기가 되게 조절하면 I/O 가 효율적
- B Tree는 많은 DB 에서 인덱스 저장 방법으로 사용됨

### 18.1 Definition of B-trees
- 모든 노드는 최대 n 개의 자식들을 가짐
- branch node 들은 최소 n/2 개의 child 가짐
- roof 는 최소 2 개의 chile 가짐 (균형 때문인가?)
- 모든 leaf node 들은 같은 height
- 모든 node 들은 key, child node pointer 를 가짐

#### The height of a B-tree
- 동일한 데이터에서 B-tree 는 red-black tree 보다 height 가 작음
- 디스크 I/O 가 더 save 할 수 있음

### 18.2 Basic operations on B-trees

#### Searching a B-tree
- chile node 의 숫자가 다르다는걸 제외하곤 binary search 와 비슷

#### Creating an empty B-tree
- B-tree T 확인하기 위해 create empty node 를 하고, insert(at: 0) 해보자
- new node 만드는데 필요한 시간 : O(1) 

#### Inserting a key into a B-tree
- binary search 에 key 넣는것보다 더 복잡
- new key 는 leaf node 에 insert
- node 가 full 일땐 median key 로 split > median key 가 parent 로
- pseudo code 와 설명

### 18.3 Deleting a key from a B-tree
- 한 node 가 완전히 delete 되면 rearrange 해야 함 > insert 보다 더 복잡
- delete 방법이 다름
  - internal node
  - leaf node 
- pseudo code 와 설명