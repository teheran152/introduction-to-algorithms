# Greedy Algorithms

## An activity-selection problem
- Each activity a_i has a start time s_i and a finish time f_i, where 0<= s_i <= f_i < inf
- compatible : the intervals of a_i and a_j do not overlap
  - s_i >= f_j or s_j >= f_i
- select a maximum-size subset of mutually compatible activities

- assume : f_1 <= f_2 <= f3 <= .. <= f_n (sorted)

- algorithm
  - activity set 중 f_i가 가장 작은 activity를 선택
  - compatible하지 않은 activity 삭제
  - 위의 반복

- 가장 빨리 끝나는 a_i을 넣어도 optimal solution을 구성할 수 있다.
  - a_i 없이 optimal solution을 구성할 수 있다면, f_i 이후에 끝나는 activity를 제거하고 a_i를 넣어서 구성할 수 있음
  
## Elements of the greedy strategy

1. Determine the **optimal substructure** of the problem
2. Develop a recursive solution
3. Show that if we make the greedy choice, then only one subproblem remains
4. Prove that it is always afe to make the greedy choice.
5. Devleop a recursive algorithm that implements the greedy strategy

## Huffman codes

## Matroids and greedy methods
A **matroid** is an ordered pair M = (S,I) satifying the following conditions.

1. S is a finite selection
2. I is a nonempty family of subsets of S, called the independent subsets of S, such that if B b < I and A <B, then A<I. We say that I is hereditary if it satifies this property. Note that the empty set phi is necessarily a member of I.

## A task-scheduling problem as a matroids

## Off-line caching


