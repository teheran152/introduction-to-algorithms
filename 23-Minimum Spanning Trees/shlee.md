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

### Prim's algorithm
```python
#!/usr/bin/env python3

import sys 

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


class Heap():

    def __init__(self):
        self.array = []
        self.size = 0
        self.pos = []

    def newMinHeapNode(self, v, w):
        minHeapNode = [v, w]
        return minHeapNode

    def heapify(self, i):
        smallest = i
        left = 2 * i + 1
        right = 2 * i + 2

        if left < self.size and self.array[left][1] < self.array[smallest][1]:
            smallest = left

        if right < self.size and self.array[right][1] < self.array[smallest][1]:
            smallest = right

        if smallest != i:
            self.pos[self.array[smallest][0]] = i
            self.pos[self.array[i][0]] = smallest
            self.array[smallest], self.array[i] = self.array[i], self.array[smallest]
            self.heapify(smallest)

    def extractMin(self):
        if self.isEmpty() == True:
            return

        root = self.array[0]

        lastNode = self.array[self.size - 1]
        self.array[0] = lastNode

        self.pos[lastNode[0]] = 0
        self.pos[root[0]] = self.size - 1

        self.size -= 1
        self.heapify(0)

        return root

    def isEmpty(self):
        return True if self.size == 0 else False

    def decreaseKey(self, v, dist):
        i = self.pos[v]

        self.array[i][1] = dist

        while i > 0 and self.array[i][1] < self.array[(i - 1) // 2][1]:
            self.pos[self.array[i][0]] = (i-1)//2
            self.pos[self.array[(i-1)//2][0]] = i
            self.array[i], self.array[(i-1)//2] = self.array[(i-1)//2], self.array[i]
            i = (i - 1) // 2

    def isInMinHeap(self, v):
        if self.pos[v] < self.size:
            return True
        return False


class Graph:
    def __init__(self, vertex, edges):
        self.vertex = vertex
        self.edges = sorted(edges, key=lambda item: item[2])
        self.graph = [[]]*vertex
        for e in edges:
            u, v, w = e
            self.graph[u].append([v, w])

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
                if len(mst) >= self.vertex - 1:
                    break

        return mst

    def PrimMST(self):
        key = []
        parent = []
        heap = Heap()

        for v in range(self.vertex):
            parent.append(-1)
            key.append(sys.maxsize)
            heap.array.append(heap.newMinHeapNode(v, key[v]))
            heap.pos.append(v)

        heap.pos[0] = 0
        key[0] = 0
        heap.decreaseKey(0, key[0])

        heap.size = self.vertex

        while not heap.isEmpty():
            newHeapNode = heap.extractMin()
            u = newHeapNode[0]

            for v, w in self.graph[u]:
                if heap.isInMinHeap(v) and w < key[v]:
                    key[v] = w
                    parent[v] = u

                    # update distance value in min heap also
                    heap.decreaseKey(v, key[v])

        return parent


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

    print("Kruskal")
    mst = g.KruskalMST()

    for u, v, w in mst:
        print("%d - %d => %d" % (u, v, w))

    print("Prism")        
    mst = g.PrismMST()

    for u in mst:
        print("%d" % u)


