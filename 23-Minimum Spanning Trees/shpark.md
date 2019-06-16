### Minimum Spanning Tree
  - Wiring problem
    - To interconnect a set of n pins, we can use an arrangement of n - 1 wires, each connectiong two pins
  - Modeling
    - G = (V, E) => connected, undirected graph 
    - V => set of pins
    - E => set of possible interconnections between pairs of pins
    - (u, v) ∈ E => weight ω(u, v) specifying the cost (amount of wire needed) to connect  u and v
  - Subject
    - To find an acyclic subset T ⊆ E that connects all the vertices and whose total weight is minimized

  - minimum-spanning-tree problem
    - Kruskal's algorithm & Prim's algorithm
    - O(E lg V) using ordinary binary heaps
    - O(E + V lg V) using Fibonacci heaps (Prim's algorithm)

#### 23.1 Growing a minimum spanning tree
  - Kruskal’s & Prim’s algorithms use greedy approach
    - Grows the minimum spanning tree one edge at a time with maintaining loop variant
    - loop invariant : Prior to each iteration, A is a subset of some minimum spanning tree
  - safe edge 
    - edge (u, v) that we can add to A without violating invariant
    - A U {(u, v)} is subset of a minimum spanning tree

  ```
  GENERIC-MST(G, w)
    A = nil
    while A does not form a spanning tree
    find an edge (u, v) that is safe for A
    A = A U {(u, v)}
    return A
  ```
  
  - loop invariant 
    - Initialization : After line 1, the set A trivially satisfies the loop invariant.
    - Maintenance : The loop maintains the invariant by adding only safe edges.
    - Termination : All edges added to A are in a minimum spanning tree.

  - cut (S, V - S) of an undirected graph G = (V, E)
    - a partition of V
    - edge (u, v) ∈ E is **crosses** the cut (S, V - S) if one of its endpoints is in S and the other is in V - S
    - a cut **respects** a set of A of edges if no edge in A crosses the cut
    - **light edge** is edge crossing a cut which is the minimum of any edge crossing the cut

  - Theorem 23.1
    - Let A be a subset of E that is included some minimum spanning tree for G, 
    - Let (S, V - S) be any cut of G that respects A
    - Let (u, v) be a light edge crossing (S, V - S)
    - Then, edge (u, v) is safe for A

  - understanding GENERIC-MST
    - set A is always acyclic
    - At any point in the execution, the graph G_A = (V, A) is a forest
    - Each of the connected components of G_A is a tree
    - Any safe edge (u, v) for A connects distinct components of G_A
    - while loop executes |V| - 1 times 
      - it finds one of the |V| - 1 edges of a minimum spanning tree in each iteration
      - when the forest contains only a single tree, the method terminates

  - Corollary 23.2
    - If (u, v) is a light edge connection C to some other component in forest, then (u, v) is safe for A

#### 23.2 The algorithms of Kruskal and Prim
##### Kruskal’s algorithm
  - at each step, Kruskal’s algorithm adds to the forest an edge of least possible weight
  - It uses a disjoint-set data structture to maintain several disjoint sets of elements
    - FIND-SET, UNION

  ```
  MST-KRUSKAL(G, w)
    A = nil
    for each vertex v ∈ G.V
      MAKE-SET(v)
    for each edge (u, v) ∈ G.E, taken in nondecreasing order by weight w
      if FIND-SET(u) != FIND-SET(v)
        A = A U {(u, v)}
        UNION(u, v)
    return A
  ```
  - running time : depends on how we implement the disjoint-set data structure
     - using disjoint-set-forest with union-by-rank and path-compression,
       - O(E lg E) for sort edges in line 4
       - O((V + E) α(V)) : |V| MAKE-SET, O(E) FIND-SET & UNION operations on |V| elements of set
         - O(m α(n)) for m disjoint-set operation on n elements (section 21.4)
       - |E| >= |V| - 1, α(V) = O(lg V) = O(lg E)
       - O(E lg E) -> (|E| < |V|^2), O(E lg V)
##### Prim’s algorithm
  - at each step, the edges in the set A always form a single tree
  -  Each step adds to the tree A a light edge that connect A to an isolated vertex
  
  ```
  MST-PRIM(G, w, r)
    for each u ∈ G.V
      u.key = infinite
      u.π = nil
    r.key = 0
    Q = G.V
    while Q != nil
      u = EXTRACT-MIN(Q)
      for each v ∈ G.adj[u]
        if v ∈ Q and w(u, v) < v.key
          v.π = u
          v.key = w(u, v)
  ```

  - loop invariant
    - A = {(v, v.π) : v ∈ V - {r} - Q}
    - The vertices already placed into the minimum spanning tree are those in V - Q
    - For all vertices v ∈ Q, if v.π != nil, then v.key has valid vaue and v.key is the weight of a light edge (v, v.π) connecting v to some vertex already placed into the minimum spanning tree

  - running time : depends on how we implement the min-priority queue Q
    - using binary min-heap,
      - BUILD-MIN-HEAP : O(V)
      - EXTRACT-MIN : O(lg V) for each |V| times => O(V lg V)
      - O(E) for line 8-11
      - test for membership in Q : constant time 
      - DECREASE-KEY : line 11, O(lg V)
      - O(V lg V + E lg V) = O(E lg V)
    - using Fibonacci heap, O(E + V lg V)







    
