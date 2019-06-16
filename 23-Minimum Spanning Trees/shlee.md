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
```python
#!/usr/bin/env python3

class DisjointSet:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find_parent(self, v):
        if self.parent[v] == v:
            return v
        return self.find_parent(self.parent[v])

    def union(self, x, y):
        xroot = self.find_parent(x)
        yroot = self.find_parent(y)

        if self.rank[xroot] < self.rank[yroot]:
            self.parent[xroot] = yroot
        elif self.rank[xroot] > self.rank[yroot]:
            self.parent[yroot] = xroot
        else:
            self.parent[yroot] = xroot
            self.rank[xroot] += 1

class Graph:
    def __init__(self, vertex, edges):
        self.vertex = vertex
        self.edges = sorted(edges, key=lambda item: item[2])

    def KruskalMST(self):
        disjoint = DisjointSet(self.vertex)

        mst = []
        for e in self.edges:
            u, v, _ = e
            x = disjoint.find_parent(u)
            y = disjoint.find_parent(v)

            if x != y:
                mst.append(e)
                disjoint.union(x, y)
                if len(mst) >= self.vertex -1:
                    break

        return mst


if __name__ == "__main__":
    g = Graph(9, [
        [0, 1, 4],
        [0, 7, 8],
        [1, 2, 8],
        [1, 7, 11],
        [2, 3, 7],
        [2, 8, 2],
        [2, 5, 4], 
        [3, 4, 9], 
        [3, 5, 14],
        [4, 5, 10], 
        [5, 6, 2], 
        [6, 7, 1], 
        [6, 8, 6], 
        [7, 8, 7]]
    )

    mst = g.KruskalMST()

    for u, v, w in mst:
        print("%d - %d => %d" % (u, v, w))

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

