### Greedy Algorithms

- 항상 그 순간의 최선으로 보이는 선택을 함
- locally optimal choice -> globally optimal solution
- examples
  - Minimum-spanning-tree algorithm
  - Dijkstra's algorithm for shortest paths from a single source
  - Chratal's greedy set-covering heuristic

#### Elements of the greedy strategy
  - Greedy-choice property
    - loaclly optimal (greedy) choice 함으로써, globally optimal solution을 얻을 수 있음
    - <=> subproblem의 결과를 고려할 필요 없이 현재 문제의 최선의 선택을 한다
    - dp 
      - subproblem의 solution에 의존
      - subproblems 해결 후 현재 문제 해결
    - greedy
      - 현재 문제 상태만 고려
      - 현재 문제애 대한 선택 후 subproblem 문제에 대한 선택
  - Optimal substructure
    - An optimal solution to the problem contains within it optimal solutions to subproblems

  - Greedy versus dynamic programming
    - Greedy로 dp보다 효율적이게 풀 수 있는 문제도 있고, Greedy를 적용할 수 없는 문제도 있다.
    - ex. 0-1 knapsack problem, fractional knapsack problem
