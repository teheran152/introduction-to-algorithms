## Van Emde Boas Trees
- supporting operations for priority queue
  - binary heaps
  - red-black trees
  - Finonacci heaps
- 위 자료구조들은 적어도 하나는 BicO(log n) time 을 가짐
  - decisions on comparing keys 기반이기 때문
- van Emde Boas trees supports BicO(log log n) or BicO (log log u) operations
  - n : number of elements in the set
  - u : the range of possible values
  - search
  - insert
  - delete
  - minimum
  - maximum
  - successor
  - predecessor
- 조건
  - keys must be integers in the range 0 to n - 1
  - no duplicates

### 20.1 Preliminary approaches

#### Direct addressing
- The simplest approach to storing a dynamic set
- makde A[0 .. u - 1] for storing a dynamic set in {0, 1, 2, ..., u - 1}
```
if A[x] == dynamic set 
    A[x] = 1 
else 
    A[x] = 0
```
- BicO(1) operations
  - insert
  - delete
  - member
- Theta(u) operations
  - minimum
  - maximum
  - successor
  - predecessor
- 1 값을 연결해 가면 predecessor 를 찾아갈 수 있음

#### Superimposing a binary tree structure
- short-cut long scans shows in the bit vector
- Theta(u) worst-case time
  - minimum : root > down toward the leaves > only containing a 1 node
  - maximum : root > down toward the leaves > only rightmost containing 1 node
  - successor : x index leaf > up toward the root > enter a node (containing a 1) from left > right child z > down z (leftmost node containing a 1) 
  - predecessor : x index leaf > up toward the root > rightmost node containing a 1
- BicO(log u) in the worst-case
  - better than red black tree
    - searching a red-black tree is BicO(log n)
  - but if n < u a red-black tree would be faster 

#### Superimposing a tree of constant height
- u = 2^2k > squareRoot(u) is an integer
- let superimpose a tree of degree squareRoot(u)
  - height of the tree is always 2
- BicO(squareRoot(u)) time
  - minumun, maximum : find the leftmost/rightmost in summary (contains 1) > summary[i] > search within i index cluster 
  - successor, predecessor : search to the right/left within cluster > if find 1, position is result > else let i = [x/squareRoot(u)] ? search to the right/left within the summary array from index i
  - delete : let i = [x/squareRoot(u)] > set A[x] to 0 > set summary[i] to the logical-or in the i index cluster
- search at most two clusters of squareRoot(u) takes BicO(squareRoot(u)) time
- using a tree of degree squareRoot(u) is the key of van Emde Boas trees

### 20.2 A recursive structure
- The goal is to achieve bigO(loglogu) running time operations
- recurrence 에 대한 설명들 이해 못하겠음 ㅜㅜ

#### Proto van Emde Boas structures
- proto van Emde Boas structure or proto-vEB structure in {0, 1, 2, ..., u - 1}
  - if u == 2 > base size > contains A[0..1] of two bits
  - if u == 2^2^k > proto-vEB(u) contains these attributes
    - summary pointer 
    - cluster pointer (squareRoot(u) pointers array)
  - 이건 그림으로 이해되지 않아서 코드를 찾아봤는데 더 이해가 안된다 ㅎㅎ;

#### Operations on a proto van Emde Boas structure
- do not change the operations
  - member
  - minimum
  - maximum
  - successor
- do change operations
  - insert
  - delete
  - maximum
  - predecessor

#### Determining whether a value is in the set
```
// proto_vEB_member(vStructure, value)
if vStructure.u == 2 {
  return vStructure.A[value]
} else {
  return proto_vEB_member(vStructure.cluster[high(value)], low(value))
}
```
- The first return is base case
- The second return is recursive case, find the smller proto_vEB structure

#### Finding the minimum element
```
// proto_vEB_minimum(vStructure)
if vStructure.u == 2 {
  if vStructure.A[0] == 1 {
    return 0
  } else if vStructure.A[1] == 1 {
    return 1
  } else {
    return nil
  }
} else { 
  let minCluster = proto_vEB_minimum(vStructure.summary)
  if munCluster == nil {
    return nil
  } else {
    let offset = proto_vEB_minimum(vStructure.cluster[minCluster])
    return index(munCluster, offset)
  }
}

```
- 2-6 lines handle by brute force
- 7-11 lines handle the recursive case
- recursive case can't run in bicO(loglogu) in worst case (7-11 lines have two recursive)

#### Finding the successor
```
// proto_vEB_successor(vStructure, value)
if vStructure.u == 2 {
  if value == 0 && vStructure.A[1] == 1 {
    return 1
  } else {
    return nil
  }
} else {
  let offset = proto_vEB_successor(vStructure.cluster[high(value)], low(value))
  if offset != nil {
    return index(high(value), offset)
  } else {
    let successCluster = proto_vEB_successor(vStructure.summary, high(value))
    if successCluster == nil {
      return nil
    } else {
      return index(successCluster, offset)
    }
  }
}

```
- first line is base case
- 2-4 lines handle by brute force
- 5-12 lines handle the recursive case
- 이것도 역시 recursive 가 2번 돌 수 있으므로 worst case 에선 bicO(loglogu) 를 보장할 수 없음

#### Inserting an element
```
// proto_vEB_insert(vStructure, value)
if vStructure.u == 2 {
  vStructure.A[x] = 1
} else {
  proto_vEB_insert(vStructure.cluster[high(x)], log(value))
  proto_vEB_insert(vStructure.summary, high(x))
}
```
- insert operation runs in Theta(logu)

#### Deleting an element
- always determine whethers cluster is 1 

### 20.3 The van Emde Boas tree
- 와... 진짜 어렵네 ㅎㅎ; 그림과 설명 이해못함

#### van Emde Boas trees
- van Emde Boas tree == vEB tree
- min, max attributes reduce the number of recursive calls
  - minimum, maximum operations don't need recursive, just return values
  - successor operation don't need recursive call to determine high(value)
  - using min, max attributes, we knows 3 case in vEB tree
    - empty
    - just 1 element
    - over two elements
  - if vEB tree is emptry, insert, delegte operations run in constant time
- 2개의 attributes 들을 추가한 결과, vEB tree 를 이용하면 empty 한 tree 를 만들때 constant 시간 안에 만들 수 있음, red_black_tree 도 마찬가지임

#### Finding the minimum and maximum elements
```
// vEB_tree_minimum(vStructure)
return vStructure.min

// vEB_tree_maximum(vStructure)
return vStructure.max
```

#### Determining whether a value is in the set The
```
// vEB_tree_member(vStructure, value)
if value == vStructure.min || value == vStructure.max {
  return true
} else if vStructure.u == 2 {
  return false
} else {
  return vEB_tree_member(vStructure.cluster[high(value)], low(value))
}
```
- runs in bicO(loglogu)

#### Finding the successor and predecessor
```
// vEB_tree_successor(vStructure, value)
if vStructure.u == 2 {
  if value == 0 && vStructure.max == 1 {
    return 1
  } else {
    return nil
  }
} else if vStructure.min != nil && value < vStructure.min {
  return vStructure.min
} else {
  let maxLow = vEB_tree_maximum(vStructure.cluster[high(value)])
  if maxLow != nil && maxLow > low(value) {
    let offset = vEB_tree_successor(vStructure.cluster[high(value)], low(value))
    return index(high(value), offset)
  } else {
    let successCluster = vEB_tree_successor(vStructure.summary, high(value))
    if successCluster == nil {
      return nil
    } else {
      let offset = vEB_tree_minimum(vStructure.cluster[successCluster])
      return index(successCluster, offset)
    }
  }
}

// vEB_tree_predecessor(vStructure, value)
if vStructure.u == 2 {
  if value == 1 && vStructure.min == 0 {
    return 0
  } else {
    return nil
  }
} else if vStructure.min != nil && value > vStructure.max { 
  return vStructure.max
} else {
  let minLow = vEB_tree_minimum(vStructure.cluster[high(value)])
  if minLow != nil && minLow < low(value) {
    let offset = vEB_tree_predessor(vStructure.cluster[high(value)], low(value))
    return index(high(value), offset)
  } else {
    let predCluster = vEB_tree_predecessor(vStructure.summary, high(value))
    if predCluster == nil {
      if vStructure.min != nil && vStructure.min < x {
        return vStructure.min
      } else {
        return nil
      }
    } else {
      let offset = vEB_tree_maximum(vStructure.cluster[predCluster])
      return index(predCluster, offset)
    }
  }
}
```
- successor, predecessor operations runs in bicO(loglogu) in worst case 

#### Inserting an element
```
// vEB_empty_tree_insert(vStructure, value)
vStructure.min = value
vStructure.max = value

// vEB_tree_insert(vStructure, value)
if vStructure.min == nil {
  vEB_empty_tree_insert(vStructure, value)
} else if value < vStructure.min {
  swap(value, vStructure.min)

  if vStructure.u > 2 {
    if vEB_tree_minimum(vStructure.cluster[high(value)]) == nil {
      vEB_tree_insert(vStructure.summary, high(value))
      vEB_empty_tree_insert(vStructure.cluster[high(value)], low(value))
    } else {
      vEB_tree_insert(vStructure.cluster[high(value)], low(value))
    }
  }
  if value > vStructure.max {
    vStructure.max = value
  }
}
```
- vEB_tree_insert runs in bicO(1) in base case
- vEB_tree_insert runs in BicO(loglogu) in recursive case

#### Deleting an element 
```
// vEB_tree_delete(vStructure, value)
if vStructure.min == vStructure.max {
  vStructure.min = nil
  vStructure.max = nil
} else if vStructure.u == 2 {
  if value == 0 {
    vStructure.min = 1
  } else {
    vStructure.min = 0
    vStructure.max = vStructure.min
  }
} else if value == vStructure.min {
  let firstCluster = vEB_tree_minimum(vStructure.summary)
  value = index(firstCluster, vEB_tree_minimum(vStructure.cluster[firstCluster]))
  vStructure.min = value
  vEB_tree_minimum(vStructure.cluster[high(value)], low(value))

  if vEB_tree_minimum(vStructure.cluster[high(value)]) == nil {
    vEB_tree_delete(vStructure.summary, high(value))
    if value == vStructure.max {
      let summaryMax = vEB_tree_maximum(vStructure.summary)
      if summaryMax == nil {
        vStructure.max = vStructure.min
      } else {
        vStructure.max = index(summaryMax, vEB_tree_maximum(vStructure.cluster[summaryMax]))
      }
    }
  } else if value == vStructure.max {
    vStructure.max = index(high(valeu))
    vEB_tree_maximum(vStructure.cluster[high(value)])
  }
}

```
- vEB_tree_delete runs in bicO(loglogu) in worst case
- The recursive call on line 13 took constant time
- The recursive call on line 15 did not occur