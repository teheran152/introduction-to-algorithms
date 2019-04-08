### 17.1 Aggregate analysis

#### Stack operations
- push, pop 의 cost 가 1이라고 했을때 비용은 O(1)
- n 번 발생하면 O(n)
- aggregate analysis 은 n 번 수행한 비용을 합 / n = O(1)
- aggregate analysis 에 push, pop 에 multipop 추가
- multipop 추가해서 은 push 가 일어난만큼만 할 수 있기때문에 n 번 이상 X = O(n)
- aggregate(push + pop + multipop) = O(3n)/n = O(1)
- 이 때 Amortized O(1) 로 표현 

#### Incrementing a binary counter
- 각 iteration 마다 flipped 되는 bit 비용이 다름

### 17.2 The accounting method

#### Stack operations
- 접시 예제
- push 할때마다 1$ 의 credit 을 저장
- 그러므로 pop, multipop 일때 credit 으로 비용 낼 수 

#### Incrementing a binary counter
- 비트 하나를 1로 만드는데 2달러의 amortized cost 부과
- 비트 하나가 1이 될 때 1달러 지불, 비트가 나중에 0으로 바뀔때 나머지 1달러 사용
- 모든 1은 1달러의 credit 을 가지고, 비트를 0으로 바꿀때 그 비트위에 1달러로 지불
- 1인 비트의 개수는 결코 음수가 될 수 없으므로, total amortized cost 은 O(n)이고 이는 total actual cost  

### 17.3 The potential method
- 과비용을 초기에 부과하고 나중에 이보다 낮은 값으로 청구하여 이를 보상

#### Stack operations
- push 때 potential 비용을 저장, 합산되어 있다가 나중에 pop, mulitipop 때 사용
- push 에서 potential difference 가 1
- push 의 amortized cost 는 2
- pop, multipop amortized cost 는 0

#### Incrementing a binary counter
- 잘 모르겠음

### 17.4 Dynamic tables
- dynamically expanding and contracting a table.

#### Table expasion
- full table 에 insert 하려면 더 큰 table 을 만들고 copy 해야 함

