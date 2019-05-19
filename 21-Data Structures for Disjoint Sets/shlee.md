### Data Structures for Disjoint Sets

#### Disjoint-set operations
- disjoint-set data structure : maintains a collection S = {S1, S2, ..., Sk} of disjoint dynamic sets.
- identify each set by a `representative`, which is some member of the set.

- `MAKE-SET(x)` create a net set
- `UNION(x,y)` unites the dynamics sets that contain x and y.
- `FIND-SET(x)` return a pointer to the representative of the set containnig x

#### Linked-list representation of disjoint sets
- `MAKE-SET` : O(1)
- `FIND-SET` : O(1)
- `UNION(x,y)` : O(n)

##### weighted-union heuristic
append the shorter list onto the longer

#### Disjoint-set forests
- `MAKE-SET`
```c
x.p = x
x.rank = 0
```

- `FIND-SET`
```
if x != x.p
    x.p = FIND-SET(x.p)
return x.p
```

- `UNION(x,y)` 
```
LINK(FIND-SET(x), FIND-SET(y))
```

- `LINK(x,y)`
```
if x.rank > y.rank
    y.p = x
else
    x.p = y
    if x.rank == y.rank
        y.rank == y.rank + 1
```

##### Heuristics to improve the running time
- `union by rank`
make `the root of the tree with fewer nodes` point to `the root of the tree with more nodes`. `rank` : for each node, an upper bound on the height of the node. 

- `path compression`


