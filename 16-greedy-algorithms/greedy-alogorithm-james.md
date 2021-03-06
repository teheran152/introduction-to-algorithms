### Greedy Algorithm
- 최적해를 구하는 데에 사용되는 근사적인 방법으로, 여러 경우 중 하나를 결정해야 할 때마다 그 순간에 최적이라고 생가되는 것을 선택해 나가는 방식으로 진행하여 최종적인 해답에 도달. 순간마다 하는 선택은 그 순간에 대해 지역적으로 최적이지만, 그 선택들을 계속 수집하여 최종적(전역적)인 해답을 만들었다고 해서, 그것이 치적이라는 보장은 없다.

- 탐욕 알고리즘이 잘 작동하는 문제는 대부분 탐욕스런 선택 조건(greedy choice property)과 최적 부분 구조 조건(optimal substructure)이라는 두 가지 조건이 만족된다. 탐욕스런 선택 조건은 앞의 선택이 이후의 선택에 영향을 주지 않는다는 것이며, 최적 부분 구조 조건은 문제에 대한 최적해가 부분문제에 대해서도 역시 최적해라는 것이다.

##### 안되는 상황
- 지금 선택하면 1개의 마시멜로를, 1분뒤에는 2개의 마시멜로를 받는 문제에서는 Greedy는 항상 1개 밖에 받지 못함

#### 활동 선택 문제
- 활동 선택 문제는 쉽게 말하면 한 강의실에서 여러개의 수업을 하려고 할 때 한번에 가장 많은 수업을 할 수 있는 경우를 고르라는것
- 다이나믹은 
  - 제일 처음게 종료된 후부터
  - 마지막 시작하기전의 집합을 고른다.
  - 하나고르면 2개로 쪼개서 계속 이짓을 한다.
- 그리디는
  - 첫번째 활동이 최대한 일찍 끝나면 됨
  - 그다음으로 일찍 끝나는거 
  - 그 다음으로 일찍 끝나는거

#### 분할 가능 배낭 문제
- 물건이 무거울 경우 쪼개서 넣을수 있다. 무게가 초과할 거 가으면 물건을 쪼개서 일부만 넣을수 있다. 

#### 허프만 코드
- prefix code를 만들어라.
- 문자 COUNT후 BINARY SEARCH TREE로 만든다. 

16.3-2

#### Theoretical foundations for greedy methods
- 그리디 알고리즘이 항상 optimal한 solution이 되는 이론이 있다. 

##### Matroids
- M=(S, l)
- S= 유한한 비어있지 않는 집합
- l = S의 부분 집합의 비어있지 않는 family(S의 독립적인 부분 집합)
> ?? 이해가..

####  A task-scheduling problem
- 각각의 태스크는 데드라인을 가지고 있다. 데드라인을 놓치면 패널티를 받는다. 
- single processor에서 unit-time 태스크를 스캐쥴링을 할때 좋다. 
- unit-time task는 job이다. 정확하게 한유닛만에 완료를 해야 하는 
- S는 유한한 unit-Time Tasks의 집합
- deadling은 d
- 패널티는 p
- 데트라인보다 늦게 끝났으면 late
- 제시간에 끝냈으면 early
- early-first form: ai는 early aj는 late이고, aj이후에 ai가 들어온다면 두개 위치를 바꿔서 ai를 먼저하고 aj를 나중에 한다.
- 문제를 해결한다면 weight가 제일 높은애를 알맞는 자리에 배치 하고, 그다음 weight를 놓는 형태
