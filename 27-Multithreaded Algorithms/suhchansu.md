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
- 

### 27.2 Multithread matrix multiplication

### 27.3 Multithreaded merge sort