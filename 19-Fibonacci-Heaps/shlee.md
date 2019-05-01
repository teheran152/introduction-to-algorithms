### Fibonacci Heaps

#### Purpose
- mergeable heap
- const amortized time operation

#### Mergeable heaps
- ADT
    - MAKE-HEAP()
    - INSERT(H,x)
    - MINIMUM(H)
    - EXTRACT-MIN(H)
    - UNION(H1, H2)
- Fibonacci Heap(+ADT)
    - DECREASE-KEY(H,x,k)
    - DELETE(H,x)

#### Structure of Fibonacci heaps
- Fib-heap is a collection of rooted trees that are `min-heap ordered`
    - `min-heap property`
        - parent.key <= this.key
- circular doubly linked list
    - insertion : O(1)
    - concatenation : O(1)


#### Mergeable heap

##### NODE
- degree
- parent
- child
- mark

##### MAKE-HEAP()
```kotlin
H.n = 0
H.roots = null
H.min = null
```

##### INSERT(H,x)
```kotlin
if H.min == null
    H.roots = listOf({x})
    H.min = x
else 
    H.roots += x
    if x.key < H.min.key
        H.min = x
H.n++
```   

##### MINIMUM(H)
```
return H.min
```

##### UNION(U, V)

```kotlin
val H = MAKE-FIB-HEAP()
H.min = U.min
H.roots = (H1.roots + H2.roots)
if (H1.min == NIL) or (H2.min != NIL and H2.min.key < H1.min.key)) 
    H.min = H2.min
H.n = H1.n + H2.n
return H
```

##### EXTRACT-MIN(H)

```kotlin
val z = H.min
if (z != NIL) 
    for x in z.childs
        H.roots += x
        x.parent = null
    H.roots -= z
    if z == z.right
        H.min = NIL
    else
        H.min = z.right
        CONSOLIDATE(H)
    H.n--

return z
```

##### CONSOLIDATE(H)
```kotlin
val A = arrayOfNulls(D(H.n))
for w in H.roots
    var x = w
    val d = x.degree
    while A[d] != null
        val y = A[d]
        if x.key > y.key
            exchange(x, y)
        FIB-HEAP-LINK(H, y, x)
        A[d] = null
        d++
    A[d] = x
H.min = NIL
for i=0 to D(H.n)
    if A[i] != null
        if H.min == nil
            H.roots = listOf({A[j]})
            H.min = A[i]
        else
            H.roots += A[i]
            if A[i].key < H.min.key
                H.min = A[i]

```

##### FIB-HEAP-LINK(H, y, x)
```kotlin
assert(x.key < y.key)
H.roots -= y
x.childs += y
x.degree++
y.mark = false
```

##### FIB-HEAP-DECREASE-KEY(H,x,k)
```kotlin
x.key = k
val y = x.parent
if y != null and x.key < y.key
    CUT(H,x,y)
    CASCADING-CUT(H,y)
if x.key < H.min.key
    H.min = x
```

##### CUT(H, x, y)
```kotlin
y.childs -= x
y.degree--
x.p = null
x.mark = false
```

##### CASCADING-CUT(H, y)
```kotlin
val z = y.p
if z != null
    if y.mark == false
        y.mark = true
    else
        CUT(H, y, z)
        CASCADING-CUT(H,z)
```


##### DELETE(H,x)
```
FIB-HEAP-DECREASEK-KEY(H,x, -inf)
FIB-HEAP-EXTRACT-MIN(H)
```
