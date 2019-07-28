## Multithreaded Algorithms
#### Dynamic multithreaded programming
- programmers don't worry about communication protocols, load balancing
- two features
  - nested parallelism : subroutine to be “spawned”
  - parallel loops : like an ordinary for loop
- advantages
  - simple extension of serial programming 
    - three “concurrency” keywords
      - parallel
      - spawn
      - sync
  - quantify parallelism based on `work`, `span'
  - follow divide-and-conquer paradigm

### 27.1 The basics of dynamic multithreading
- example : Fibonacci numbers recursively
``` Swift
func fibo(n: Int) {
  if n <= 1 {
    return n
  } else {
    let x = fibo(n - 1)
    let y = fibo(n - 2)
    return x + y
  }
}
```
- this computation does much repeated work
- but good example for illustrating key concepts
```Swift
// parallel Fibo
func pFibo(n: Int) {
  if n <= 1 {
    return n
  } else {
    // spawn: Nested parallelism
    let x = pFibo(n - 1)
    let y = pFibo(n - 2)
    // sync
    return x + y
  }
}
```

#### A model for multithreaded execution
- multithreaded computation == computation dag
- in dag
  - strand
    - u
    - v
  - u -> v 
    - u, v are in series
    - u, v are in parallel
- continuation edge (u, u')
  - u, u' are connected within same procedure instance
- sequentially consistent: shared memory

#### Performance measures
- uses two metrics
  - work
    - total time to execute the entire computation on one processor
    - sum of the times taken by each of the strands
  - span
    - the longest time to execute the strands in the dag
    - equals the number of vertices on a longest or critical path in the dag
- actual running time
  - work
  - span
  - how the scheduler allocats strands to processors
- work, span provice lower bounds on the running time
- work law
```
T_p >= T_1 / P`
```
- span
```
T_p >= T_inifinity
```
- three perspectives in parallelism
  - As a ratio, average amout of work that can be performed in parallel for each step along the critical path
  - As an upper bound, gives the maximum possible speedup that can be achived on any number of processors
  - provides a limit on the possibility of attaining perfect linear speedup

#### Scheduling
- good performance : minimizing work, span
- no way to specify which strands to execute on which processors
- Instead, the concurrency platform's scheduler to map the dynamically unforlding computation to individual processors
- The scheduler maps the strands to static threads
- OS shcedules the threads on the processors
- centralized scheduler : uses greedy

##### Theorem 27.1
- greedy scheduler executes a mutithreaded computation 
```
T_p <= T_1 / P + T_inifinity
```

##### Proof
- considering the complete steps
```
P([T_1 / P] + 1)
== P[T_1] + P
== T_1 - (T_1 mod P) + P  (by equation 3.8)
> T_1             (by equation 3.9)
```
- contradiction : P processors would perform more work tha computation requires
- considering the incomplete step
- uses G (dag), G', G''
- Theorem 27.1 shows that a greedy scheduler always performs well

##### Corollary 27.2, 27.3 and Proofs
- about work, span 

#### Analyzing multithreaded algorithms
- P-Fibo example
```
T_infinity(n)
== max(T_infinity(n - 1), T_infinity(n - 2)) + Bictheta(1)
== T_infinity(n - 1) + Bictheta(n)
== T_infinity = BicTheta(n)
```

#### Parallel loops
- uses spawn, sync 
```Swift
func matrixVector(A, x) {
  let n = A.rows
  var y = Vector(length: n)

  // parallel : respective loops may be run concurrently
  for i in n {
    y_i = 0
  }

  // parallel : respective loops may be run concurrently
  for i in n {
    y+i = y_i + a_ijx_j
  }
  return y
}
```
- compiler can implement each parallel for loop as a divide and conquer subroutine using nested parallelism

```Swift
func matrixVectorMainLoop(A, x, y, n, i, i') {
  if i == i' {
    for j in n {
      y_i = y_i + a_ijx_j
    }
  } else {
    let mid = (i + i') / 2
    // spawn 
    // recursively spawns the first half of the iterations
    matrixVectorMainLoop(A, x, y, n, i, mid)
    matrixVectorMainLoop(A, x, y, n, mid + 1, i')
    // sync
    // creating a binary tree of execution
  }
}
```
- the overhead of recursive spawning
  - increase the work of a parallel loop compared with that of its serialization, but not asymptotically


#### Race conditions
- bane of concurrency 
- `determinacy race` : two parallel instrucions acess the same memory, performs a write
```Swift
func raceExample() {
  var x = 0
  // parallel
  for 1...2 {
    x = x + 1
  }
  print("\(x)")
}
```
- sequence of instructions
  - 1. read x from memory
  - 2. increase the value
  - 3. write the value into x in memory
- races can be extremely hard to test for
- a faulty implementation 
```Swift
func matrixVectorWrong(A, x) {
  let n = A.rows
  var y = Vector(length: n)
  // parallel
  for i in n {
    y_i = 0
  }
  // parallel
  for i in n {
    // parallel
    for j in n {
      y_i = y_i + a_ijx_j
    }
  }
  return y
}
```

#### A chess lession
- the example a real story
- developed chess progream in 32 processor and then adapt superComputer which have 512 processor
- arrise race condition

### 27.2 Multithread matrix multiplication

### 27.3 Multithreaded merge sort
