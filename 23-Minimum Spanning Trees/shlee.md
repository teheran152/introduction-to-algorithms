## Minimum Spanning Trees

> acyclic subset T that connects all of the vertices and whose total weight w(T) = \sum w(u,v) is minimized

### Growing a minimum spanning tree

```
A = {}
while A doest not form a spanning tree
    find an edge (u,v) that is safe for A
    A = A union {(u,v)}
return A
```

### Kruskal's algorithm
```
A  = {}
for each vertex v of V
    MAKE-SET(v)
sort the edges of E into nondecreaing order by weight w
for each edge(u,v) takin in nondecreasing order by weight
    if FIND-SET(u) != FIND-SET(v)
        A = A union {(u,v)}
        UNION(u,v)
return A
```

### Prism's algorithm
```
for each u of V
    u.key = inf
    u.pi = nil
r.key = 0
Q = V
while Q is not empty
    u = EXTRACT-MIN(Q)
    for each v which adjacent u
        if v in Q and w(u,v) < v.key
            v.pi = u
            v.key = w(u,v)

