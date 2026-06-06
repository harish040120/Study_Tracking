# 🗺️ Graph — Complete 5-Phase Course (Java Handbook)

**Language:** Java | **Level:** Complete Beginner → SDE Ready
**Style:** Concept → Visual → Code → Practice Problems

> You have NEVER done graph problems.
> This handbook teaches you every concept from scratch,
> in the exact order an SDE interview expects you to know them.

---

# 📌 Contents

**Phase 1 — Graph Foundations**
- 1.1 What is a Graph?
- 1.2 Types of Graphs
- 1.3 Graph Representations
- 1.4 How to Build a Graph in Java
- 1.5 Java Syntax Reference

**Phase 2 — Graph Traversal (Core)**
- 2.1 Depth First Search (DFS)
- 2.2 BFS (Breadth First Search)
- 2.3 DFS vs BFS — When to Use Which
- 2.4 Graph on a Grid (Matrix DFS/BFS)
- 2.5 Connected Components

**Phase 3 — Cycle Detection & Bipartite**
- 3.1 Cycle in Undirected Graph
- 3.2 Cycle in Directed Graph
- 3.3 Bipartite Graph Check

**Phase 4 — Topological Sort & Union Find**
- 4.1 Topological Sort (DFS + Kahn's BFS)
- 4.2 Union Find (Disjoint Set)
- 4.3 Cycle Detection with Union Find

**Phase 5 — Shortest Path Algorithms**
- 5.1 BFS Shortest Path (Unweighted)
- 5.2 Dijkstra's Algorithm (Weighted, non-negative)
- 5.3 Bellman-Ford (Negative Weights)
- 5.4 Floyd-Warshall (All-Pairs)
- 5.5 Minimum Spanning Tree (Prim's + Kruskal's)

---

# 🟢 Phase 1 — Graph Foundations

---

## 1.1 What is a Graph?

### Real-World Analogy

Think of cities and roads:
- **Nodes (Vertices):** cities
- **Edges:** roads connecting cities
- **Weight on edge:** distance between cities

A graph is any structure where THINGS (nodes) are CONNECTED (edges).

```
Examples of graphs in real life:
  Google Maps      → cities = nodes, roads = edges
  Social Media     → people = nodes, friendships = edges
  Internet         → pages = nodes, links = edges
  University       → courses = nodes, prerequisites = edges
```

### Graph Vocabulary

```
Node / Vertex  → a point in the graph (usually labeled 0,1,2... or 'A','B'...)
Edge           → a connection between two nodes
Neighbor       → a node directly connected to another node
Degree         → number of edges a node has
Path           → sequence of nodes connected by edges
Cycle          → path that starts and ends at the same node
Connected      → every node can reach every other node
Disconnected   → some nodes cannot reach each other
Weighted       → edges have a cost/distance value
Unweighted     → all edges have equal cost (cost = 1)
Directed       → edges have a direction (one-way)
Undirected     → edges go both ways (two-way)
DAG            → Directed Acyclic Graph (no cycles, directed)
```

### Visual Example

```
Undirected Graph:        Directed Graph:
  0 --- 1                0 → 1
  |     |                ↓   ↓
  2 --- 3                2 → 3

Node 0's neighbors: 1, 2    Node 0 can reach: 1, 2, 3
Node 1's neighbors: 0, 3    Node 2 can reach: 3 only
```

---

## 1.2 Types of Graphs

### By Direction

**Undirected:** edge between A and B means you can go BOTH ways.
```
A -- B   means A→B AND B→A
Used for: social networks, roads (usually)
```

**Directed:** edge A→B means you can only go FROM A TO B.
```
A → B   means A→B only, NOT B→A
Used for: course prerequisites, web links, task dependencies
```

### By Weight

**Unweighted:** all edges cost the same (cost = 1).
```
Used for: find if path exists, count hops
Algorithm: BFS gives shortest path
```

**Weighted:** edges have different costs.
```
Used for: shortest route by distance/time/cost
Algorithm: Dijkstra, Bellman-Ford
```

### By Structure

**Connected:** you can reach any node from any other node.

**Disconnected:** some nodes are isolated — you need to run DFS/BFS for EVERY unvisited node.

**Tree:** connected graph with no cycles. N nodes, N-1 edges.

**DAG (Directed Acyclic Graph):** directed, no cycles.
Used for: course scheduling, build systems, task ordering.

### Quick Identifier Table

```
See in problem:                  → Graph Type
"Can you reach from A to B?"    → Unweighted, BFS/DFS
"Minimum distance A to B?"      → BFS (unweighted) or Dijkstra (weighted)
"Prerequisites / dependencies"  → DAG, Topological Sort
"Detect cycle"                  → Cycle Detection (DFS or Union Find)
"Connected components"          → DFS/BFS + visited array
"Minimum cost to connect all"   → MST (Kruskal or Prim)
"Social network / friendship"   → Undirected + Union Find
```

---

## 1.3 Graph Representations

There are TWO main ways to store a graph in memory.

### Option 1: Adjacency List ✅ (Use this — most common)

Each node stores a LIST of its neighbors.

```
Graph:  0--1, 0--2, 1--3, 2--3

Adjacency List:
  0 → [1, 2]
  1 → [0, 3]
  2 → [0, 3]
  3 → [1, 2]
```

**Space:** O(V + E) — only stores actual edges.
**Best for:** sparse graphs (few edges), most LeetCode problems.

### Option 2: Adjacency Matrix

A 2D grid where `matrix[i][j] = 1` means there is an edge from i to j.

```
Graph:  0--1, 0--2, 1--3

Matrix (4×4):
     0  1  2  3
  0 [0, 1, 1, 0]
  1 [1, 0, 0, 1]
  2 [1, 0, 0, 1]
  3 [0, 1, 1, 0]
```

**Space:** O(V²) — stores ALL possible pairs even if no edge.
**Best for:** dense graphs, quick edge lookup O(1).
**Rarely used in LeetCode** — adjacency list is preferred.

---

## 1.4 How to Build a Graph in Java

### Method 1: List of Lists (Most Common)

```java
int n = 5; // 5 nodes: 0, 1, 2, 3, 4

// Create adjacency list
List<List<Integer>> graph = new ArrayList<>();
for (int i = 0; i < n; i++) {
    graph.add(new ArrayList<>());
}

// Add edges (undirected: add both directions)
graph.get(0).add(1);
graph.get(1).add(0);

graph.get(0).add(2);
graph.get(2).add(0);

graph.get(1).add(3);
graph.get(3).add(1);

// Access neighbors of node 0:
for (int neighbor : graph.get(0)) {
    System.out.print(neighbor + " "); // prints: 1 2
}
```

### Method 2: HashMap of Lists (When nodes aren't 0 to N-1)

```java
Map<Integer, List<Integer>> graph = new HashMap<>();

// Add edge u → v
void addEdge(int u, int v) {
    graph.computeIfAbsent(u, k -> new ArrayList<>()).add(v);
    graph.computeIfAbsent(v, k -> new ArrayList<>()).add(u); // undirected
}

// Access neighbors
List<Integer> neighbors = graph.getOrDefault(node, new ArrayList<>());
```

### Method 3: Build from Edge List Input (Very Common in LC)

```java
// Input: int[][] edges = {{0,1},{0,2},{1,3},{2,3}};  n = 4
List<List<Integer>> buildGraph(int n, int[][] edges) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
    for (int[] edge : edges) {
        int u = edge[0], v = edge[1];
        graph.get(u).add(v);
        graph.get(v).add(u); // remove this line for directed graph
    }
    return graph;
}
```

### Method 4: Build Weighted Graph

```java
// Each entry: [neighbor, weight]
List<List<int[]>> graph = new ArrayList<>();
for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

// Add weighted edge u → v with weight w
graph.get(u).add(new int[]{v, w});
graph.get(v).add(new int[]{u, w}); // for undirected
```

---

## 1.5 Java Syntax Reference — Graphs

```java
// ── BUILD GRAPH ───────────────────────────────────────────────
List<List<Integer>> graph = new ArrayList<>();
for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

// ── ADD EDGE ──────────────────────────────────────────────────
graph.get(u).add(v);            // directed: u → v
graph.get(u).add(v);
graph.get(v).add(u);            // undirected: both directions

// ── ACCESS NEIGHBORS ──────────────────────────────────────────
for (int neighbor : graph.get(node)) { }

// ── VISITED ARRAY ─────────────────────────────────────────────
boolean[] visited = new boolean[n]; // all false by default
visited[node] = true;

// ── BFS QUEUE ─────────────────────────────────────────────────
Queue<Integer> q = new LinkedList<>();
q.offer(node);
int curr = q.poll();
q.isEmpty();

// ── DFS STACK (iterative) ─────────────────────────────────────
Deque<Integer> stack = new ArrayDeque<>();
stack.push(node);
int curr = stack.pop();

// ── INDEGREE ARRAY (for Topo Sort) ───────────────────────────
int[] indegree = new int[n];
for (int[] edge : edges) indegree[edge[1]]++;

// ── DISTANCE ARRAY ────────────────────────────────────────────
int[] dist = new int[n];
Arrays.fill(dist, Integer.MAX_VALUE);
dist[src] = 0;

// ── PRIORITY QUEUE FOR DIJKSTRA ───────────────────────────────
PriorityQueue<int[]> pq =
    new PriorityQueue<>((a, b) -> a[1] - b[1]); // sort by distance
pq.offer(new int[]{node, distance});
int[] curr = pq.poll();
int node = curr[0], dist = curr[1];

// ── UNION FIND ────────────────────────────────────────────────
int[] parent = new int[n];
int[] rank = new int[n];
for (int i = 0; i < n; i++) parent[i] = i;
```

---

# 🔵 Phase 2 — Graph Traversal

---

## 2.1 Depth First Search (DFS)

### Concept

DFS explores as FAR as possible down one path before backtracking.

Think of it like exploring a maze:
- Go down one corridor as far as you can
- Hit a dead end → go BACK to last junction
- Try the next corridor
- Repeat until all corridors explored

**Uses a stack** (either recursion call stack or explicit stack).

### Visual Trace

```
Graph: 0--1--3
       |
       2--4

DFS from 0:
  Visit 0 → go to neighbor 1
  Visit 1 → go to neighbor 3
  Visit 3 → no unvisited neighbors → backtrack
  Back at 1 → no more neighbors → backtrack
  Back at 0 → go to neighbor 2
  Visit 2 → go to neighbor 4
  Visit 4 → done

Order: 0 → 1 → 3 → 2 → 4
```

### Recursive DFS (Most Common)

```java
boolean[] visited;
List<List<Integer>> graph;

void dfs(int node) {
    visited[node] = true;
    System.out.print(node + " "); // PROCESS node here

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            dfs(neighbor); // recurse deeper
        }
    }
}

// HOW TO CALL:
// visited = new boolean[n];
// dfs(0); // start from node 0
```

### Iterative DFS (Using explicit stack)

```java
void dfsIterative(int start, List<List<Integer>> graph) {
    boolean[] visited = new boolean[graph.size()];
    Deque<Integer> stack = new ArrayDeque<>();

    stack.push(start);

    while (!stack.isEmpty()) {
        int node = stack.pop();

        if (visited[node]) continue; // skip already visited
        visited[node] = true;
        System.out.print(node + " "); // PROCESS

        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                stack.push(neighbor);
            }
        }
    }
}
```

### DFS to Count Nodes in Component

```java
int countNodes(int start, boolean[] visited,
               List<List<Integer>> graph) {
    visited[start] = true;
    int count = 1; // count this node

    for (int neighbor : graph.get(start)) {
        if (!visited[neighbor]) {
            count += countNodes(neighbor, visited, graph);
        }
    }

    return count;
}
```

**Time Complexity:** O(V + E) — visits every vertex once, every edge once.
**Space Complexity:** O(V) — recursion stack depth.

---

## 2.2 Breadth First Search (BFS)

### Concept

BFS explores ALL neighbors at the current level before going deeper.

Think of it like dropping a stone in water:
- Ripples spread outward level by level
- All nodes 1 hop away first
- Then all nodes 2 hops away
- Then all nodes 3 hops away

**Uses a Queue** (FIFO — first in, first out).
**Key property: BFS always finds the SHORTEST PATH in unweighted graphs.**

### Visual Trace

```
Graph: 0--1--3
       |
       2--4

BFS from 0:
  Level 0: [0]       → visit 0
  Level 1: [1, 2]    → visit 1, 2 (neighbors of 0)
  Level 2: [3, 4]    → visit 3 (neighbor of 1), 4 (neighbor of 2)

Order: 0 → 1 → 2 → 3 → 4

Distances from 0:
  0: distance 0
  1: distance 1
  2: distance 1
  3: distance 2
  4: distance 2
```

### BFS Template

```java
void bfs(int start, List<List<Integer>> graph) {
    int n = graph.size();
    boolean[] visited = new boolean[n];
    Queue<Integer> q = new LinkedList<>();

    // STEP 1: Initialize — add start node
    q.offer(start);
    visited[start] = true;

    while (!q.isEmpty()) {
        int node = q.poll(); // take from front
        System.out.print(node + " "); // PROCESS

        // STEP 2: Add all unvisited neighbors
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true; // mark BEFORE adding to queue
                q.offer(neighbor);
            }
        }
    }
}
```

### BFS with Distance Tracking

```java
int[] bfsDistance(int start, List<List<Integer>> graph) {
    int n = graph.size();
    int[] dist = new int[n];
    Arrays.fill(dist, -1); // -1 = not visited

    Queue<Integer> q = new LinkedList<>();
    q.offer(start);
    dist[start] = 0;

    while (!q.isEmpty()) {
        int node = q.poll();

        for (int neighbor : graph.get(node)) {
            if (dist[neighbor] == -1) { // not visited
                dist[neighbor] = dist[node] + 1; // one more hop
                q.offer(neighbor);
            }
        }
    }

    return dist; // dist[i] = shortest distance from start to i
}
```

### BFS Level by Level (Important Pattern)

```java
void bfsLevelByLevel(int start, List<List<Integer>> graph) {
    boolean[] visited = new boolean[graph.size()];
    Queue<Integer> q = new LinkedList<>();
    q.offer(start);
    visited[start] = true;
    int level = 0;

    while (!q.isEmpty()) {
        int size = q.size(); // ⭐ freeze current level count
        System.out.print("Level " + level + ": ");

        for (int i = 0; i < size; i++) { // only process THIS level
            int node = q.poll();
            System.out.print(node + " ");

            for (int neighbor : graph.get(node)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.offer(neighbor);
                }
            }
        }

        System.out.println();
        level++;
    }
}
```

**Time Complexity:** O(V + E)
**Space Complexity:** O(V)

---

## 2.3 DFS vs BFS — When to Use Which

```
PROBLEM ASKS                          → USE
──────────────────────────────────────────────────────
"Does a path exist?"                 → DFS (simpler)
"Find shortest path (unweighted)"    → BFS ✅ always BFS
"Shortest path in grid"              → BFS
"Explore all paths"                  → DFS
"Count connected components"         → DFS or BFS (either)
"Detect cycle"                       → DFS
"Topological order"                  → DFS (postorder) or BFS (Kahn's)
"Level-by-level processing"          → BFS
"Closest node satisfying condition"  → BFS
"All nodes in a component"           → DFS (flood fill)
"Minimum hops/steps"                 → BFS ✅
```

### Key Rule

> BFS = SHORTEST PATH (unweighted)
> DFS = EXPLORE EVERYTHING, CYCLE DETECTION, TOPO SORT

---

## 2.4 Graph on a Grid (Matrix DFS/BFS)

### Concept

Many graph problems ARE grids (matrices). Each cell is a node, and neighbors are the 4 adjacent cells (up, down, left, right).

```
Grid:
  1  1  0  0
  1  0  0  1
  0  0  1  1

Node (0,0) neighbors: (0,1) and (1,0)
Node (1,1) neighbors: (0,1), (2,1), (1,0), (1,2)
```

### Standard Directions Setup

```java
int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}}; // up, down, left, right

// Check if valid cell
boolean inBounds(int i, int j, int rows, int cols) {
    return i >= 0 && i < rows && j >= 0 && j < cols;
}
```

### Grid DFS — Flood Fill Pattern

```java
// NUMBER OF ISLANDS (LC 200) — classic grid DFS
int numIslands(char[][] grid) {
    int rows = grid.length, cols = grid[0].length;
    int count = 0;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] == '1') {
                dfs(grid, i, j); // mark whole island
                count++;
            }
        }
    }
    return count;
}

void dfs(char[][] grid, int i, int j) {
    // Out of bounds OR water OR already visited
    if (i < 0 || i >= grid.length) return;
    if (j < 0 || j >= grid[0].length) return;
    if (grid[i][j] != '1') return;

    grid[i][j] = '0'; // mark visited (in-place)

    dfs(grid, i-1, j); // up
    dfs(grid, i+1, j); // down
    dfs(grid, i, j-1); // left
    dfs(grid, i, j+1); // right
}
```

### Grid BFS — Shortest Path in Grid

```java
// SHORTEST PATH IN BINARY MATRIX (LC 1091)
int shortestPath(int[][] grid) {
    int n = grid.length;
    if (grid[0][0] == 1 || grid[n-1][n-1] == 1) return -1;

    int[][] dirs = {{-1,-1},{-1,0},{-1,1},{0,-1},
                    {0,1},{1,-1},{1,0},{1,1}}; // 8 directions

    Queue<int[]> q = new LinkedList<>();
    q.offer(new int[]{0, 0, 1}); // {row, col, distance}
    grid[0][0] = 1; // mark visited

    while (!q.isEmpty()) {
        int[] curr = q.poll();
        int r = curr[0], c = curr[1], dist = curr[2];

        if (r == n-1 && c == n-1) return dist; // reached!

        for (int[] d : dirs) {
            int nr = r + d[0], nc = c + d[1];
            if (nr >= 0 && nr < n && nc >= 0 && nc < n
                    && grid[nr][nc] == 0) {
                grid[nr][nc] = 1; // mark visited
                q.offer(new int[]{nr, nc, dist + 1});
            }
        }
    }
    return -1;
}
```

### Multi-Source BFS — Start from Multiple Points

Used when: problem says "spread from ALL sources simultaneously."
Example: multiple rotten oranges rotting neighbors at the same time.

```java
// ROTTING ORANGES (LC 994)
int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> q = new LinkedList<>();
    int fresh = 0;

    // ADD ALL rotten oranges as starting points
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) q.offer(new int[]{i, j});
            if (grid[i][j] == 1) fresh++;
        }

    if (fresh == 0) return 0;

    int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    int minutes = 0;

    while (!q.isEmpty() && fresh > 0) {
        minutes++;
        int size = q.size(); // process ONE minute at a time

        for (int k = 0; k < size; k++) {
            int[] curr = q.poll();

            for (int[] d : dirs) {
                int nr = curr[0] + d[0];
                int nc = curr[1] + d[1];

                if (nr >= 0 && nr < m && nc >= 0 && nc < n
                        && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2; // rot it
                    fresh--;
                    q.offer(new int[]{nr, nc});
                }
            }
        }
    }

    return fresh == 0 ? minutes : -1;
}
```

---

## 2.5 Connected Components

### Concept

A connected component is a group of nodes where every node can reach every other node in the group.

```
Graph:  0--1    3--4    6
        |               
        2               

Components: {0,1,2}, {3,4}, {6}
Count = 3
```

### Count Components using DFS

```java
int countComponents(int n, int[][] edges) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

    for (int[] e : edges) {
        graph.get(e[0]).add(e[1]);
        graph.get(e[1]).add(e[0]);
    }

    boolean[] visited = new boolean[n];
    int components = 0;

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(i, visited, graph); // explore entire component
            components++;           // one more component found
        }
    }

    return components;
}

void dfs(int node, boolean[] visited, List<List<Integer>> graph) {
    visited[node] = true;
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) dfs(neighbor, visited, graph);
    }
}
```

### LeetCode Practice — Phase 2

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 733 | Flood Fill | Grid DFS | Easy |
| 200 | Number of Islands | Grid DFS | Med |
| 695 | Max Area of Island | Grid DFS + count | Med |
| 130 | Surrounded Regions | Grid DFS from border | Med |
| 994 | Rotting Oranges | Multi-source BFS | Med |
| 542 | 01 Matrix | Multi-source BFS | Med |
| 1091 | Shortest Path in Binary Matrix | Grid BFS | Med |
| 133 | Clone Graph | DFS + HashMap | Med |
| 323 | Number of Connected Components | DFS/BFS | Med |
| 547 | Number of Provinces | DFS/BFS | Med |

---

# 🟡 Phase 3 — Cycle Detection & Bipartite

---

## 3.1 Cycle in Undirected Graph

### Concept

An undirected graph has a cycle if you can start from a node, follow edges, and come back to the same node WITHOUT reusing the same edge.

```
Has cycle:          No cycle (tree):
0--1--2             0--1--2
|     |                    |
+-----+             3------+
```

### Method 1: DFS with Parent Tracking

Key idea: in an undirected graph, when doing DFS, if you visit a node that is ALREADY visited AND it is NOT the parent you came from → CYCLE exists.

```java
boolean hasCycle(int n, int[][] edges) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
    for (int[] e : edges) {
        graph.get(e[0]).add(e[1]);
        graph.get(e[1]).add(e[0]);
    }

    boolean[] visited = new boolean[n];

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            if (dfsHasCycle(i, -1, visited, graph)) return true;
        }
    }
    return false;
}

boolean dfsHasCycle(int node, int parent, boolean[] visited,
                    List<List<Integer>> graph) {
    visited[node] = true;

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            if (dfsHasCycle(neighbor, node, visited, graph)) return true;
        } else if (neighbor != parent) {
            // visited AND not the parent we came from → CYCLE!
            return true;
        }
    }
    return false;
}
```

### Method 2: BFS with Parent Tracking

```java
boolean hasCycleBFS(int start, boolean[] visited,
                    List<List<Integer>> graph) {
    Queue<int[]> q = new LinkedList<>(); // [node, parent]
    q.offer(new int[]{start, -1});
    visited[start] = true;

    while (!q.isEmpty()) {
        int[] curr = q.poll();
        int node = curr[0], parent = curr[1];

        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.offer(new int[]{neighbor, node});
            } else if (neighbor != parent) {
                return true; // CYCLE found
            }
        }
    }
    return false;
}
```

---

## 3.2 Cycle in Directed Graph

### Concept

In a DIRECTED graph, cycles are trickier. You need to track:
- Nodes fully explored (done) → `visited[]`
- Nodes in the CURRENT DFS path → `inStack[]` (also called recursion stack)

If you reach a node that is in `inStack` → CYCLE exists.

```
Directed cycle:    No cycle (DAG):
0 → 1 → 2          0 → 1 → 2
↑       |               ↓
+-------+          0 → 3 → 4
```

### DFS with Recursion Stack

```java
boolean hasCycleDirected(int n, int[][] edges) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
    for (int[] e : edges) graph.get(e[0]).add(e[1]);

    boolean[] visited = new boolean[n];
    boolean[] inStack = new boolean[n]; // current DFS path

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            if (dfsCycle(i, visited, inStack, graph)) return true;
        }
    }
    return false;
}

boolean dfsCycle(int node, boolean[] visited, boolean[] inStack,
                 List<List<Integer>> graph) {
    visited[node] = true;
    inStack[node] = true; // entering this node's path

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            if (dfsCycle(neighbor, visited, inStack, graph)) return true;
        } else if (inStack[neighbor]) {
            return true; // found node in current path → CYCLE
        }
    }

    inStack[node] = false; // leaving this node's path
    return false;
}
```

### BFS Cycle Detection (Kahn's Algorithm)

If topological sort using BFS visits fewer nodes than total → cycle exists.

```java
boolean hasCycleBFS(int n, List<List<Integer>> graph) {
    int[] indegree = new int[n];
    for (int u = 0; u < n; u++)
        for (int v : graph.get(u)) indegree[v]++;

    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; i++)
        if (indegree[i] == 0) q.offer(i);

    int visited = 0;
    while (!q.isEmpty()) {
        int node = q.poll();
        visited++;
        for (int neighbor : graph.get(node)) {
            if (--indegree[neighbor] == 0) q.offer(neighbor);
        }
    }

    return visited != n; // if not all visited → cycle exists
}
```

---

## 3.3 Bipartite Graph Check

### Concept

A graph is BIPARTITE if you can color all nodes with exactly 2 colors such that NO two adjacent nodes have the same color.

```
Bipartite:          NOT Bipartite:
0(R) - 1(B)         0 - 1 - 2
|       |           |         |
2(B) - 3(R)         +---------+
                    (triangle = odd cycle = not bipartite)
```

**Key fact:** A graph is bipartite if and only if it has NO odd-length cycles.

**Use case in interviews:** "Can we divide people into two groups with no conflicts?"

### BFS Coloring

```java
boolean isBipartite(int[][] graph) {
    int n = graph.length;
    int[] color = new int[n]; // 0=uncolored, 1=red, -1=blue
    // graph[i] = list of neighbors of i (given as adjacency list)

    for (int start = 0; start < n; start++) {
        if (color[start] != 0) continue; // already colored

        Queue<Integer> q = new LinkedList<>();
        q.offer(start);
        color[start] = 1; // color first node RED

        while (!q.isEmpty()) {
            int node = q.poll();

            for (int neighbor : graph[node]) {
                if (color[neighbor] == 0) {
                    // uncolored → give opposite color
                    color[neighbor] = -color[node];
                    q.offer(neighbor);
                } else if (color[neighbor] == color[node]) {
                    // same color as current → NOT bipartite
                    return false;
                }
            }
        }
    }
    return true;
}
```

### LeetCode Practice — Phase 3

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 207 | Course Schedule | Cycle in directed graph | Med |
| 684 | Redundant Connection | Cycle in undirected | Med |
| 785 | Is Graph Bipartite? | Bipartite BFS | Med |
| 886 | Possible Bipartition | Bipartite | Med |
| 802 | Find Eventual Safe States | Cycle detection | Med |
| 261 | Graph Valid Tree | Cycle + connected | Med |

---

# 🟠 Phase 4 — Topological Sort & Union Find

---

## 4.1 Topological Sort

### Concept

Topological sort gives an ORDER of nodes such that for every directed edge u→v, node u comes BEFORE node v.

Only works on **DAG** (Directed Acyclic Graph — no cycles).

**Real world:** If course A must be done before course B, topological sort gives a valid order to take all courses.

```
Graph: 5→0, 5→2, 4→0, 4→1, 2→3, 3→1

One valid order: 4, 5, 2, 0, 3, 1
Another valid: 5, 4, 2, 0, 3, 1
```

### Method 1: Kahn's Algorithm (BFS) ✅ Recommended

**Indegree:** number of edges pointing INTO a node.

**Algorithm:**
1. Compute indegree of every node
2. Add all nodes with indegree 0 into queue (no prerequisites)
3. Process queue: take a node, add to result, reduce neighbor indegrees
4. If a neighbor's indegree becomes 0 → add to queue
5. If result has all N nodes → valid topo sort. Else → cycle exists.

```java
List<Integer> topoSort(int n, int[][] edges) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

    int[] indegree = new int[n];
    for (int[] e : edges) {
        graph.get(e[0]).add(e[1]);
        indegree[e[1]]++;
    }

    // Start with all nodes that have no prerequisites
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) q.offer(i);
    }

    List<Integer> order = new ArrayList<>();

    while (!q.isEmpty()) {
        int node = q.poll();
        order.add(node); // add to topological order

        for (int neighbor : graph.get(node)) {
            indegree[neighbor]--; // one prerequisite done
            if (indegree[neighbor] == 0) {
                q.offer(neighbor); // ready to take this course
            }
        }
    }

    // If not all nodes visited → cycle → no valid order
    return order.size() == n ? order : new ArrayList<>();
}
```

### Kahn's for Course Schedule (LC 207)

```java
boolean canFinish(int numCourses, int[][] prerequisites) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) graph.add(new ArrayList<>());

    int[] indegree = new int[numCourses];
    for (int[] pre : prerequisites) {
        // pre[1] must be done before pre[0]
        graph.get(pre[1]).add(pre[0]);
        indegree[pre[0]]++;
    }

    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) q.offer(i);
    }

    int count = 0;
    while (!q.isEmpty()) {
        int course = q.poll();
        count++;
        for (int next : graph.get(course)) {
            if (--indegree[next] == 0) q.offer(next);
        }
    }

    return count == numCourses; // can finish all if no cycle
}
```

### Method 2: DFS Postorder (Stack)

In DFS postorder: add a node to result AFTER all its descendants are processed. Then reverse the result.

```java
List<Integer> topoSortDFS(int n, List<List<Integer>> graph) {
    boolean[] visited = new boolean[n];
    Deque<Integer> stack = new ArrayDeque<>();

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            dfsTopoSort(i, visited, stack, graph);
        }
    }

    List<Integer> result = new ArrayList<>();
    while (!stack.isEmpty()) result.add(stack.pop());
    return result;
}

void dfsTopoSort(int node, boolean[] visited, Deque<Integer> stack,
                 List<List<Integer>> graph) {
    visited[node] = true;
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            dfsTopoSort(neighbor, visited, stack, graph);
        }
    }
    stack.push(node); // push AFTER all descendants are done
}
```

---

## 4.2 Union Find (Disjoint Set Union — DSU)

### Concept

Union Find answers ONE question efficiently:
**"Are node A and node B in the same connected group?"**

Think of it like a school social group system:
- Every student starts in their own group
- When two students become friends, their groups MERGE
- To check if two students are in the same group → find their group leader (root)

**Two operations:**
- `find(x)` → who is the root/leader of x's group?
- `union(x, y)` → merge the groups of x and y

### Basic Implementation

```java
int[] parent;
int[] rank;

void init(int n) {
    parent = new int[n];
    rank = new int[n];
    for (int i = 0; i < n; i++) parent[i] = i; // each is its own leader
}

// Find root with PATH COMPRESSION
// Path compression: make every node point directly to root
int find(int x) {
    if (parent[x] != x) {
        parent[x] = find(parent[x]); // compress: point to root
    }
    return parent[x];
}

// Union by RANK
// Rank: attach smaller tree under bigger tree (keeps tree flat)
boolean union(int x, int y) {
    int rootX = find(x);
    int rootY = find(y);

    if (rootX == rootY) return false; // already same group → cycle!

    if (rank[rootX] < rank[rootY]) parent[rootX] = rootY;
    else if (rank[rootX] > rank[rootY]) parent[rootY] = rootX;
    else {
        parent[rootY] = rootX;
        rank[rootX]++;
    }
    return true; // successfully merged
}

boolean connected(int x, int y) {
    return find(x) == find(y);
}
```

### Union Find — Complete Class

```java
class UnionFind {
    int[] parent, rank;
    int components; // track number of components

    UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        components = n;
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }

    boolean union(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;

        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }

        components--;
        return true;
    }

    boolean connected(int x, int y) {
        return find(x) == find(y);
    }

    int getComponents() { return components; }
}
```

### Union Find — Number of Provinces (LC 547)

```java
int findCircleNum(int[][] isConnected) {
    int n = isConnected.length;
    UnionFind uf = new UnionFind(n);

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (isConnected[i][j] == 1) {
                uf.union(i, j);
            }
        }
    }

    return uf.getComponents();
}
```

---

## 4.3 Cycle Detection with Union Find

### Concept

When building a graph edge by edge, if you try to union two nodes that already have the SAME root → adding this edge would create a cycle.

```java
// REDUNDANT CONNECTION (LC 684)
// Find the edge that creates a cycle in undirected graph
int[] findRedundantConnection(int[][] edges) {
    int n = edges.length;
    UnionFind uf = new UnionFind(n + 1); // nodes are 1-indexed

    for (int[] edge : edges) {
        if (!uf.union(edge[0], edge[1])) {
            return edge; // this edge creates a cycle!
        }
    }
    return new int[]{};
}
```

### LeetCode Practice — Phase 4

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 207 | Course Schedule | Kahn's Topo Sort | Med |
| 210 | Course Schedule II | Kahn's + return order | Med |
| 310 | Minimum Height Trees | Topo Sort (trim leaves) | Med |
| 269 | Alien Dictionary | Topo Sort on chars | Hard |
| 684 | Redundant Connection | Union Find cycle | Med |
| 547 | Number of Provinces | Union Find | Med |
| 721 | Accounts Merge | Union Find + HashMap | Med |
| 261 | Graph Valid Tree | Union Find | Med |
| 323 | Number of Connected Components | Union Find | Med |
| 947 | Most Stones Removed | Union Find | Med |

---

# 🔴 Phase 5 — Shortest Path Algorithms

---

## 5.1 BFS Shortest Path (Unweighted)

### When to Use

When ALL edges have cost = 1 (unweighted), BFS automatically finds the shortest path.

**Why?** BFS visits nodes level by level. Level 1 = 1 hop away, Level 2 = 2 hops away. First time you reach destination = shortest path.

```java
// SHORTEST PATH — unweighted graph
int shortestPath(List<List<Integer>> graph, int src, int dst) {
    boolean[] visited = new boolean[graph.size()];
    Queue<Integer> q = new LinkedList<>();

    q.offer(src);
    visited[src] = true;
    int dist = 0;

    while (!q.isEmpty()) {
        int size = q.size(); // process level by level

        for (int i = 0; i < size; i++) {
            int node = q.poll();
            if (node == dst) return dist; // found!

            for (int neighbor : graph.get(node)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.offer(neighbor);
                }
            }
        }
        dist++;
    }
    return -1; // not reachable
}
```

### Word Ladder Pattern (LC 127)

Transform one word to another, changing one letter at a time. Each word = node. Two words connected if they differ by 1 letter.

```java
int ladderLength(String beginWord, String endWord,
                 List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    if (!wordSet.contains(endWord)) return 0;

    Queue<String> q = new LinkedList<>();
    q.offer(beginWord);
    int steps = 1;

    while (!q.isEmpty()) {
        int size = q.size();

        for (int i = 0; i < size; i++) {
            String word = q.poll();
            char[] arr = word.toCharArray();

            for (int j = 0; j < arr.length; j++) {
                char original = arr[j];

                for (char c = 'a'; c <= 'z'; c++) {
                    arr[j] = c;
                    String newWord = new String(arr);

                    if (newWord.equals(endWord)) return steps + 1;

                    if (wordSet.contains(newWord)) {
                        wordSet.remove(newWord); // mark visited
                        q.offer(newWord);
                    }
                }
                arr[j] = original; // restore
            }
        }
        steps++;
    }
    return 0;
}
```

---

## 5.2 Dijkstra's Algorithm (Weighted, Non-negative)

### Concept

Dijkstra finds the shortest path from ONE source to ALL other nodes in a WEIGHTED graph (with NON-NEGATIVE weights).

**Core idea:** always process the currently CLOSEST unvisited node.
Uses a **MinHeap (Priority Queue)** to always get the closest node.

```
Graph: 0 --(4)--> 1 --(2)--> 3
       |                     ↑
      (1)                    |
       ↓                    (5)
       2 -------(3)--------→ /

Dijkstra from 0:
Start: dist = [0, ∞, ∞, ∞]
Process 0 (dist=0):
  update 1: 0+4=4
  update 2: 0+1=1
  dist = [0, 4, 1, ∞]

Process 2 (dist=1, closest):
  update 3 via 2: 1+3=4 → wait, 1→3 via 1→3 = 4+2=6, but 2→3=1+3=4
  dist = [0, 4, 1, 4]

Process 1 (dist=4):
  update 3: 4+2=6 > 4, no update
  dist = [0, 4, 1, 4]

Final distances: 0→0, 0→1=4, 0→2=1, 0→3=4
```

### Dijkstra Template

```java
int[] dijkstra(int n, List<List<int[]>> graph, int src) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[src] = 0;

    // MinHeap: [node, distance]
    PriorityQueue<int[]> pq =
        new PriorityQueue<>((a, b) -> a[1] - b[1]);
    pq.offer(new int[]{src, 0});

    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int node = curr[0], d = curr[1];

        // ⭐ CRITICAL: skip stale entries
        if (d > dist[node]) continue;

        for (int[] edge : graph.get(node)) {
            int neighbor = edge[0], weight = edge[1];
            int newDist = dist[node] + weight;

            if (newDist < dist[neighbor]) {
                dist[neighbor] = newDist;
                pq.offer(new int[]{neighbor, newDist});
            }
        }
    }

    return dist; // dist[i] = shortest distance from src to i
}
```

### Network Delay Time (LC 743)

```java
int networkDelayTime(int[][] times, int n, int k) {
    // Build weighted graph
    List<List<int[]>> graph = new ArrayList<>();
    for (int i = 0; i <= n; i++) graph.add(new ArrayList<>());
    for (int[] t : times) {
        graph.get(t[0]).add(new int[]{t[1], t[2]});
    }

    // Dijkstra from k
    int[] dist = new int[n + 1];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[k] = 0;

    PriorityQueue<int[]> pq =
        new PriorityQueue<>((a, b) -> a[1] - b[1]);
    pq.offer(new int[]{k, 0});

    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int node = curr[0], d = curr[1];
        if (d > dist[node]) continue;

        for (int[] edge : graph.get(node)) {
            int neighbor = edge[0], w = edge[1];
            if (dist[node] + w < dist[neighbor]) {
                dist[neighbor] = dist[node] + w;
                pq.offer(new int[]{neighbor, dist[neighbor]});
            }
        }
    }

    int maxDist = 0;
    for (int i = 1; i <= n; i++) {
        if (dist[i] == Integer.MAX_VALUE) return -1;
        maxDist = Math.max(maxDist, dist[i]);
    }
    return maxDist;
}
```

### ⚠️ Dijkstra Limitations

```
✅ Works for: non-negative edge weights
❌ Fails for: NEGATIVE edge weights (use Bellman-Ford instead)
❌ Not for:   finding all pairs (use Floyd-Warshall)
```

---

## 5.3 Bellman-Ford (Negative Weights)

### Concept

Bellman-Ford finds shortest paths even when edges have NEGATIVE weights.

**Algorithm:** Relax ALL edges N-1 times.
(A path with N nodes has at most N-1 edges.)

```java
int[] bellmanFord(int n, int[][] edges, int src) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[src] = 0;

    // Relax all edges N-1 times
    for (int i = 0; i < n - 1; i++) {
        for (int[] edge : edges) {
            // edge = [u, v, weight]
            int u = edge[0], v = edge[1], w = edge[2];
            if (dist[u] != Integer.MAX_VALUE
                    && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
            }
        }
    }

    // Check for negative cycle
    for (int[] edge : edges) {
        int u = edge[0], v = edge[1], w = edge[2];
        if (dist[u] != Integer.MAX_VALUE
                && dist[u] + w < dist[v]) {
            // Still relaxing → negative cycle exists!
            System.out.println("Negative cycle detected");
            return null;
        }
    }

    return dist;
}
```

### Algorithm Comparison Table

```
Algorithm      | Weights       | Use For            | Time
───────────────────────────────────────────────────────────
BFS            | Unweighted    | Shortest hops      | O(V+E)
Dijkstra       | Non-negative  | Single source SP   | O((V+E)logV)
Bellman-Ford   | Any (neg OK)  | Single source SP   | O(VE)
Floyd-Warshall | Any           | All pairs SP       | O(V³)
```

---

## 5.4 Floyd-Warshall (All-Pairs Shortest Path)

### Concept

Finds shortest path between EVERY pair of nodes. O(V³) — only use for small graphs.

```java
int[][] floydWarshall(int n, int[][] graph) {
    // graph[i][j] = weight from i to j (INF if no edge)
    int[][] dist = new int[n][n];

    // Copy input
    for (int i = 0; i < n; i++)
        dist[i] = Arrays.copyOf(graph[i], n);

    // Try every node as intermediate
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (dist[i][k] != Integer.MAX_VALUE
                        && dist[k][j] != Integer.MAX_VALUE) {
                    dist[i][j] = Math.min(
                        dist[i][j],
                        dist[i][k] + dist[k][j]
                    );
                }
            }
        }
    }
    return dist;
}
```

---

## 5.5 Minimum Spanning Tree (MST)

### Concept

A Minimum Spanning Tree connects ALL nodes with the MINIMUM total edge weight and NO cycles.

```
Graph:              MST:
0 --(4)-- 1         0 --(4)-- 1
|         |         |
(2)       (3)       (2)
|         |         |
2 --(1)-- 3         2 --(1)-- 3

Total weight:
All edges: 4+2+1+3=10        MST: 2+1+4=7 (minimum!)
```

### Kruskal's Algorithm (Sort edges + Union Find)

**Steps:**
1. Sort ALL edges by weight (ascending)
2. For each edge, if it doesn't create a cycle (Union Find) → add it
3. Stop when you have N-1 edges (all nodes connected)

```java
int kruskalMST(int n, int[][] edges) {
    // Sort edges by weight
    Arrays.sort(edges, (a, b) -> a[2] - b[2]);

    UnionFind uf = new UnionFind(n);
    int totalCost = 0;
    int edgesUsed = 0;

    for (int[] edge : edges) {
        int u = edge[0], v = edge[1], w = edge[2];

        if (uf.union(u, v)) { // no cycle → add this edge
            totalCost += w;
            edgesUsed++;
            if (edgesUsed == n - 1) break; // MST complete
        }
    }

    return edgesUsed == n - 1 ? totalCost : -1; // -1 if disconnected
}
```

### Prim's Algorithm (Greedy with MinHeap)

**Steps:**
1. Start from any node
2. Always pick the cheapest edge that connects the MST to a NEW node
3. Repeat until all nodes are in MST

```java
int primMST(int n, List<List<int[]>> graph) {
    boolean[] inMST = new boolean[n];
    // MinHeap: [cost, node]
    PriorityQueue<int[]> pq =
        new PriorityQueue<>((a, b) -> a[0] - b[0]);
    pq.offer(new int[]{0, 0}); // start from node 0 with cost 0
    int totalCost = 0;

    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int cost = curr[0], node = curr[1];

        if (inMST[node]) continue; // already in MST
        inMST[node] = true;
        totalCost += cost;

        for (int[] edge : graph.get(node)) {
            int neighbor = edge[0], w = edge[1];
            if (!inMST[neighbor]) {
                pq.offer(new int[]{w, neighbor});
            }
        }
    }

    return totalCost;
}
```

### Min Cost to Connect All Points (LC 1584)

```java
int minCostConnectPoints(int[][] points) {
    int n = points.length;
    // Distance between any two points = Manhattan distance
    // |x1-x2| + |y1-y2|

    boolean[] inMST = new boolean[n];
    int[] minDist = new int[n]; // min cost to add each point to MST
    Arrays.fill(minDist, Integer.MAX_VALUE);
    minDist[0] = 0;

    int totalCost = 0;

    for (int i = 0; i < n; i++) {
        // Find closest point not yet in MST
        int u = -1;
        for (int j = 0; j < n; j++) {
            if (!inMST[j] && (u == -1 || minDist[j] < minDist[u])) {
                u = j;
            }
        }

        inMST[u] = true;
        totalCost += minDist[u];

        // Update distances for remaining points
        for (int v = 0; v < n; v++) {
            if (!inMST[v]) {
                int dist = Math.abs(points[u][0] - points[v][0])
                         + Math.abs(points[u][1] - points[v][1]);
                minDist[v] = Math.min(minDist[v], dist);
            }
        }
    }

    return totalCost;
}
```

### LeetCode Practice — Phase 5

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 127 | Word Ladder | BFS shortest path | Hard |
| 994 | Rotting Oranges | Multi-source BFS | Med |
| 743 | Network Delay Time | Dijkstra | Med |
| 1631 | Path with Minimum Effort | Dijkstra on grid | Med |
| 1514 | Path with Maximum Probability | Dijkstra (max) | Med |
| 787 | Cheapest Flights Within K Stops | Bellman-Ford | Med |
| 778 | Swim in Rising Water | Dijkstra | Hard |
| 1584 | Min Cost to Connect All Points | Kruskal / Prim | Med |
| 1489 | Find Critical Edges in MST | Advanced MST | Hard |

---

# 🧠 Master Decision Table

```
READ PROBLEM → MATCH SIGNAL → PICK ALGORITHM
──────────────────────────────────────────────────────────────
Signal                        Algorithm
──────────────────────────────────────────────────────────────
"Find if path exists"         → DFS or BFS (either)
"Shortest path, unweighted"   → BFS ✅
"Shortest path, weighted ≥0"  → Dijkstra ✅
"Shortest path, neg weights"  → Bellman-Ford
"All pairs shortest path"     → Floyd-Warshall
"Count islands / components"  → DFS flood fill
"Spread from multiple starts" → Multi-source BFS
"Cycle in undirected"         → DFS + parent OR Union Find
"Cycle in directed"           → DFS + inStack
"Valid ordering of tasks"     → Topological Sort (Kahn's)
"Prerequisites"               → Topological Sort
"Connect groups"              → Union Find
"Redundant edge"              → Union Find (cycle detection)
"Min cost connect all"        → MST (Kruskal or Prim)
"Bipartite / 2-color"         → BFS coloring
"Safe states / no cycle"      → Reverse DFS + cycle detect
"K stops / limited hops"      → Modified Dijkstra / BFS
```

---

# 📋 Java Syntax Master Reference — Graphs

```java
// ── BUILD GRAPH ───────────────────────────────────────────────
// Unweighted
List<List<Integer>> g = new ArrayList<>();
for (int i = 0; i < n; i++) g.add(new ArrayList<>());
g.get(u).add(v); g.get(v).add(u); // undirected
g.get(u).add(v);                   // directed

// Weighted: [neighbor, weight]
List<List<int[]>> g = new ArrayList<>();
g.get(u).add(new int[]{v, w});

// ── DFS RECURSIVE ─────────────────────────────────────────────
void dfs(int node, boolean[] visited, List<List<Integer>> g) {
    visited[node] = true;
    for (int nb : g.get(node))
        if (!visited[nb]) dfs(nb, visited, g);
}

// ── BFS ───────────────────────────────────────────────────────
Queue<Integer> q = new LinkedList<>();
q.offer(start); visited[start] = true;
while (!q.isEmpty()) {
    int node = q.poll();
    for (int nb : g.get(node))
        if (!visited[nb]) { visited[nb]=true; q.offer(nb); }
}

// ── GRID DIRECTIONS ───────────────────────────────────────────
int[][] dirs4 = {{-1,0},{1,0},{0,-1},{0,1}};
int[][] dirs8 = {{-1,-1},{-1,0},{-1,1},{0,-1},
                 {0,1},{1,-1},{1,0},{1,1}};

// ── BOUNDS CHECK ──────────────────────────────────────────────
if (r<0||r>=rows||c<0||c>=cols) continue;

// ── DIJKSTRA ──────────────────────────────────────────────────
int[] dist = new int[n]; Arrays.fill(dist, Integer.MAX_VALUE);
dist[src] = 0;
PriorityQueue<int[]> pq = new PriorityQueue<>((a,b)->a[1]-b[1]);
pq.offer(new int[]{src, 0});
while (!pq.isEmpty()) {
    int[] cur = pq.poll();
    if (cur[1] > dist[cur[0]]) continue; // skip stale
    for (int[] e : g.get(cur[0]))
        if (dist[cur[0]]+e[1] < dist[e[0]]) {
            dist[e[0]] = dist[cur[0]]+e[1];
            pq.offer(new int[]{e[0], dist[e[0]]});
        }
}

// ── KAHN'S TOPO SORT ──────────────────────────────────────────
int[] indegree = new int[n];
for (int[] e : edges) indegree[e[1]]++;
Queue<Integer> q = new LinkedList<>();
for (int i=0; i<n; i++) if (indegree[i]==0) q.offer(i);
List<Integer> order = new ArrayList<>();
while (!q.isEmpty()) {
    int node = q.poll(); order.add(node);
    for (int nb : g.get(node))
        if (--indegree[nb] == 0) q.offer(nb);
}
boolean hasCycle = order.size() != n;

// ── UNION FIND ────────────────────────────────────────────────
int[] parent = new int[n]; int[] rank = new int[n];
for (int i=0; i<n; i++) parent[i]=i;

int find(int x) {
    if (parent[x]!=x) parent[x]=find(parent[x]);
    return parent[x];
}
boolean union(int x, int y) {
    int px=find(x), py=find(y);
    if (px==py) return false;
    if (rank[px]<rank[py]) parent[px]=py;
    else if (rank[px]>rank[py]) parent[py]=px;
    else { parent[py]=px; rank[px]++; }
    return true;
}
```

---

# 🗂️ Complete Problem List by Phase

## Phase 2 — Traversal

| # | Problem | Technique |
|---|---------|-----------|
| 733 | Flood Fill | Grid DFS |
| 200 | Number of Islands | Grid DFS |
| 695 | Max Area of Island | Grid DFS |
| 130 | Surrounded Regions | DFS from border |
| 417 | Pacific Atlantic Water Flow | DFS from borders |
| 994 | Rotting Oranges | Multi-source BFS |
| 542 | 01 Matrix | Multi-source BFS |
| 1091 | Shortest Path in Binary Matrix | BFS |
| 133 | Clone Graph | DFS + HashMap |
| 547 | Number of Provinces | DFS / BFS |
| 323 | Number of Connected Components | DFS / BFS |
| 1020 | Number of Enclaves | DFS |
| 1905 | Count Sub Islands | DFS |
| 841 | Keys and Rooms | DFS |

## Phase 3 — Cycle & Bipartite

| # | Problem | Technique |
|---|---------|-----------|
| 207 | Course Schedule | Cycle in directed |
| 684 | Redundant Connection | Cycle in undirected |
| 261 | Graph Valid Tree | Cycle check |
| 785 | Is Graph Bipartite? | BFS coloring |
| 886 | Possible Bipartition | Bipartite |
| 802 | Find Eventual Safe States | Reverse DFS |

## Phase 4 — Topo Sort & Union Find

| # | Problem | Technique |
|---|---------|-----------|
| 207 | Course Schedule | Kahn's |
| 210 | Course Schedule II | Kahn's order |
| 269 | Alien Dictionary | Topo + char graph |
| 310 | Minimum Height Trees | Topo trim leaves |
| 2115 | Find All Possible Recipes | Topo |
| 684 | Redundant Connection | Union Find |
| 547 | Number of Provinces | Union Find |
| 721 | Accounts Merge | Union Find + HashMap |
| 947 | Most Stones Removed | Union Find |
| 1101 | Earliest When Everyone Friends | Union Find |

## Phase 5 — Shortest Path & MST

| # | Problem | Technique |
|---|---------|-----------|
| 127 | Word Ladder | BFS |
| 743 | Network Delay Time | Dijkstra |
| 1631 | Path with Minimum Effort | Dijkstra grid |
| 1514 | Path Maximum Probability | Dijkstra |
| 787 | Cheapest Flights K Stops | Bellman-Ford |
| 778 | Swim in Rising Water | Dijkstra |
| 1584 | Min Cost Connect All Points | Kruskal / Prim |

---

*Updated: 2026-06-06 | Java | Graph: Complete Beginner → SDE Ready*
*5 Phases: Foundations → Traversal → Cycle/Bipartite → Topo/DSU → Shortest Path*