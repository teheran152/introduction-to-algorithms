### 16.1 An activiy-selection problem

#### The optimal substructure of the activity-selection problem
- activity-selection problem 이 optimal substructure 을 나타냄

#### Making the greedy choice
- 모든 subprolems 들을 해결하지 않고도 optimal solution 을 찾을 수 있는 방법
- activity-selection problem 을 꼭 dynamic programing 으로 풀지 않아도 됨

#### A recursive greedy algorithm
- recursive-activity-selector pseudocode 와 해설

#### An iterative greedy algorithm
- greedy-activity-selector 는 recursive-activity-selector 의 반복
- greedy-activity-selector 수도코드와 해설

### 16.2 Elemnets of the greedy strategy
- 매 순간 최선의 선택을 함
- 이 heuristic strategy 은 항상 optimal solution 을 내는건 아니지만 되는 경우도 있음
- greedy-choice property 와 optimal substructure 가 충족되면 greedy 가 통함

#### Greedy-choice property
- dynamic programming 에선 subproblems 의 solutions 에서 선택 했었음
- smaller subproblems 에서 larger subproblems 으로 bottom-up 방식
- greedy 는 매순간 최선의 선택 후 다음으로 넘어감

#### Optimal substructure
- optimal solution 이 subproblems 의 solution들을 포함하는 경우 optimal substructure 를 보임
- dynamic programming 과 greedy algorithms 에서 해당 성격을 띔

#### Greedy vs dynamic programming
- 0-1 knapsack problem 
- dynamic programming
- fracional knapsack problem 
- greedy algorithms

### 16.3 Huffman codes

#### Prefix codes
- representation for each symbol, resulting in a prefix code 
- These are not binary search trees, since the leaves need not appear in sorted order and internal nodes do not contain character keys

#### Constructing a Huffman code
- pseudocode 와 해설
- 각 문자별 빈도수 검사 > 정렬 > 이진트리 > (빈도수가 가장 낮은 두 그룹을 merge > 정렬) 반복

#### Correctness of Huffmans's algorithm
- optimal prefix code 는 greedy-choise 와 optimal substructure properties 의 성격을 띔

### 16.4 Matroids and greedy methods
- Matroid: Greedy 가 항상 optimal solution 을 찾을 수 있는 구조

#### Greedy algorithms on a weighted matroid
- Minimum-Spanning-Tree 예시: weight 가 작은 edge 부터 선택하는 방법이 greedy

#### A task-scheduling problem as a matroid
- Single processor 에서 task 를 scheduling 할때의 해설
- task 의 deadline 을 계산하는것은 independent 하고, 다음 task 계산에 영향을 끼치지 않음
- matroid 구조이고 greedy 성격을 띔
