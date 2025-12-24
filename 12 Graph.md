# Complete Graphs Guide for Beginners 🌐

## Table of Contents
1. [Introduction to Graphs](#introduction-to-graphs)
2. [Graph Fundamentals](#graph-fundamentals)
3. [Graph Representations](#graph-representations)
4. [Graph Traversals](#graph-traversals)
5. [LeetCode Problems](#leetcode-problems)
6. [Advanced Graph Algorithms](#advanced-graph-algorithms)
7. [Additional Important Concepts](#additional-important-concepts)

---

## Introduction to Graphs

### What is a Graph?

A **graph** is a collection of **vertices (nodes)** connected by **edges**. It's one of the most powerful data structures for representing relationships between entities.

**Simple Definition:** Think of a graph as dots (vertices) connected by lines (edges).

```
Simple Graph:
    A --- B
    |     |
    C --- D
    
Vertices: A, B, C, D
Edges: A-B, A-C, B-D, C-D
```

### Real-World Examples

**1. Social Networks (Facebook, LinkedIn)**
```
    You --- Friend1
     |        |
  Friend2 - Friend3

Each person is a vertex
Friendships are edges
```

**2. GPS Navigation**
```
   City A --50km-- City B
      |              |
   30km          40km
      |              |
   City C --60km-- City D

Cities are vertices
Roads are edges (with distances)
```

**3. Course Prerequisites**
```
   Math101 → Calculus
      |          |
      ↓          ↓
   Stats   → DataScience

Courses are vertices
Prerequisites are directed edges
```

**4. Web Pages (Internet)**
```
Page A → Page B
   ↓       ↓
Page C → Page D

Pages are vertices
Links are directed edges
```

### Why Are Graphs Important?

✅ **Universal:** Can represent almost any relationship  
✅ **Scalable:** Works with billions of nodes (Facebook has 3+ billion users!)  
✅ **Powerful:** Solves complex problems (shortest path, recommendations, etc.)  
✅ **Interview Favorite:** Very common in coding interviews

---

## Graph Fundamentals

### 1. Graph Components

#### Vertices (Nodes)
**Definition:** The fundamental units of a graph (the "dots").

```python
# In code, vertices are usually represented by numbers or strings
vertices = [0, 1, 2, 3, 4]
# or
vertices = ['A', 'B', 'C', 'D', 'E']
```

#### Edges
**Definition:** Connections between vertices (the "lines").

```python
# Edges are pairs of vertices
edges = [(0, 1), (0, 2), (1, 3), (2, 3)]
# Means: 0-1, 0-2, 1-3, 2-3 are connected
```

**Visual:**
```
    0 --- 1
    |     |
    2 --- 3
```

### 2. Types of Graphs

#### Directed vs Undirected

**Undirected Graph:**
- Edges have no direction
- If A connects to B, then B connects to A
- Like friendships (mutual)

```
    A --- B
    (A can reach B, B can reach A)
```

```python
# Undirected graph representation
graph = {
    'A': ['B', 'C'],  # A connects to B and C
    'B': ['A', 'D'],  # B connects to A and D
    'C': ['A'],       # C connects to A
    'D': ['B']        # D connects to B
}
```

**Directed Graph (Digraph):**
- Edges have direction (arrows)
- If A → B, doesn't mean B → A
- Like Twitter follows (one-way)

```
    A → B
    (A can reach B, but B cannot reach A)
```

```python
# Directed graph representation
graph = {
    'A': ['B', 'C'],  # A → B and A → C
    'B': ['D'],       # B → D
    'C': [],          # C has no outgoing edges
    'D': []           # D has no outgoing edges
}
```

**Visual Comparison:**
```
Undirected:        Directed:
  A --- B           A → B
  |     |           ↓   ↓
  C --- D           C → D
```

#### Connected vs Disconnected

**Connected Graph:**
- Every vertex has a path to every other vertex
- No isolated components

```
Connected:
  A --- B
  |     |
  C --- D

Can reach any vertex from any starting point!
```

**Disconnected Graph:**
- Some vertices cannot reach others
- Multiple isolated components

```
Disconnected:
  A --- B     E --- F
  |     |     
  C --- D     G

Component 1: A, B, C, D
Component 2: E, F
Component 3: G (isolated)
```

```python
# Disconnected graph
graph = {
    'A': ['B', 'C'],  # Component 1
    'B': ['A', 'D'],
    'C': ['A'],
    'D': ['B'],
    
    'E': ['F'],       # Component 2
    'F': ['E'],
    
    'G': []           # Component 3 (isolated)
}
```

#### Types of Connected Directed Graphs

**Strongly Connected:**
- Can reach ANY vertex from ANY vertex following edge directions

```
Strongly Connected:
  A → B
  ↑   ↓
  D ← C

A→B→C→D→A (can form cycle)
```

**Weakly Connected:**
- Connected only if we ignore edge directions
- Cannot reach all vertices following directions

```
Weakly Connected:
  A → B → C
      ↓
      D

If we ignore arrows: all connected
Following arrows: C and D cannot reach A or B
```

```python
def is_strongly_connected(graph):
    """Check if directed graph is strongly connected"""
    # Need to check if all vertices reachable from any vertex
    # This is complex - usually use algorithms like Kosaraju's
    pass

def is_weakly_connected(graph):
    """Check if directed graph is weakly connected"""
    # Convert to undirected and check connectivity
    pass
```

### 3. Weighted vs Unweighted Graphs

**Unweighted Graph:**
- All edges are equal
- Used for: social networks, simple connections

```
  A --- B
  |     |
  C --- D

All edges have "cost" of 1
```

**Weighted Graph:**
- Edges have values (weights/costs)
- Used for: distances, costs, capacities

```
    A --5-- B
    |       |
   3       2
    |       |
    C --4-- D

Edge A-B costs 5
Edge A-C costs 3, etc.
```

```python
# Unweighted graph (adjacency list)
unweighted = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'D'],
    'D': ['B', 'C']
}

# Weighted graph (adjacency list with weights)
weighted = {
    'A': [('B', 5), ('C', 3)],  # (neighbor, weight)
    'B': [('A', 5), ('D', 2)],
    'C': [('A', 3), ('D', 4)],
    'D': [('B', 2), ('C', 4)]
}

# Or using dictionary for weights
weighted_alt = {
    'A': {'B': 5, 'C': 3},
    'B': {'A': 5, 'D': 2},
    'C': {'A': 3, 'D': 4},
    'D': {'B': 2, 'C': 4}
}
```

### 4. Cyclic vs Acyclic Graphs

**Cyclic Graph:**
- Contains at least one cycle (circular path)

```
Cycle:
  A → B
  ↑   ↓
  D ← C

A→B→C→D→A forms a cycle
```

**Acyclic Graph:**
- No cycles exist
- **DAG:** Directed Acyclic Graph (very important!)

```
DAG (Directed Acyclic Graph):
  A → B → D
  ↓   ↓
  C → E

No cycles possible!
```

**Why DAGs Matter:**
- Course prerequisites (no circular dependencies!)
- Task scheduling
- Build systems
- Dependency resolution

```python
def has_cycle(graph):
    """Detect cycle in directed graph using DFS"""
    visited = set()
    rec_stack = set()  # Recursion stack for current path
    
    def dfs(node):
        visited.add(node)
        rec_stack.add(node)
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                if dfs(neighbor):
                    return True
            elif neighbor in rec_stack:
                # Found a back edge!
                return True
        
        rec_stack.remove(node)
        return False
    
    for node in graph:
        if node not in visited:
            if dfs(node):
                return True
    return False

# Test
graph_with_cycle = {
    'A': ['B'],
    'B': ['C'],
    'C': ['A']  # Cycle: A→B→C→A
}
print(has_cycle(graph_with_cycle))  # True

graph_without_cycle = {
    'A': ['B'],
    'B': ['C'],
    'C': []  # No cycle
}
print(has_cycle(graph_without_cycle))  # False
```

---

## Graph Representations

### 1. Adjacency List (Most Common)

**What it is:** Dictionary/HashMap where each vertex maps to its neighbors.

**Advantages:**
- ✅ Space efficient: O(V + E)
- ✅ Fast to iterate neighbors
- ✅ Easy to implement

**Disadvantages:**
- ❌ Checking if edge exists: O(V) worst case

```python
# Adjacency List Representation
graph = {
    0: [1, 2],      # Vertex 0 connects to 1, 2
    1: [0, 3],      # Vertex 1 connects to 0, 3
    2: [0, 3],      # Vertex 2 connects to 0, 3
    3: [1, 2]       # Vertex 3 connects to 1, 2
}

# Visual representation:
#   0 --- 1
#   |     |
#   2 --- 3
```

**Creating Adjacency List from Edge List:**

```python
def build_graph(n, edges):
    """
    Build adjacency list from edge list
    
    Args:
        n: number of vertices (0 to n-1)
        edges: list of [u, v] edges
    
    Returns:
        adjacency list as dictionary
    """
    graph = {i: [] for i in range(n)}  # Initialize empty lists
    
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)  # For undirected graph
        # For directed: only graph[u].append(v)
    
    return graph

# Example
edges = [[0,1], [0,2], [1,3], [2,3]]
graph = build_graph(4, edges)
print(graph)
# {0: [1, 2], 1: [0, 3], 2: [0, 3], 3: [1, 2]}
```

**Using defaultdict (Cleaner):**

```python
from collections import defaultdict

def build_graph_clean(edges):
    """Build graph without pre-initialization"""
    graph = defaultdict(list)
    
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    
    return graph

# Example
edges = [[0,1], [0,2], [1,3], [2,3]]
graph = build_graph_clean(edges)
print(dict(graph))  # Convert to regular dict for printing
```

### 2. Adjacency Matrix

**What it is:** 2D array where `matrix[i][j]` = 1 if edge exists, 0 otherwise.

**Advantages:**
- ✅ Fast edge lookup: O(1)
- ✅ Simple for dense graphs

**Disadvantages:**
- ❌ Space inefficient: O(V²)
- ❌ Slow to iterate neighbors

```python
# Adjacency Matrix Representation
# Same graph as above:
#   0 --- 1
#   |     |
#   2 --- 3

matrix = [
    [0, 1, 1, 0],  # Vertex 0 connects to 1, 2
    [1, 0, 0, 1],  # Vertex 1 connects to 0, 3
    [1, 0, 0, 1],  # Vertex 2 connects to 0, 3
    [0, 1, 1, 0]   # Vertex 3 connects to 1, 2
]

# Check if edge exists: O(1)
has_edge = matrix[0][1] == 1  # True (0 connects to 1)

# Get all neighbors: O(V) - must check entire row
neighbors_of_0 = [j for j in range(4) if matrix[0][j] == 1]
print(neighbors_of_0)  # [1, 2]
```

**Creating Adjacency Matrix from Edge List:**

```python
def build_matrix(n, edges):
    """Build adjacency matrix from edge list"""
    matrix = [[0] * n for _ in range(n)]
    
    for u, v in edges:
        matrix[u][v] = 1
        matrix[v][u] = 1  # For undirected
        # For directed: only matrix[u][v] = 1
    
    return matrix

# Example
edges = [[0,1], [0,2], [1,3], [2,3]]
matrix = build_matrix(4, edges)
for row in matrix:
    print(row)
# [0, 1, 1, 0]
# [1, 0, 0, 1]
# [1, 0, 0, 1]
# [0, 1, 1, 0]
```

**Weighted Graph Matrix:**

```python
# For weighted graphs, store weight instead of 1
# Use infinity for no edge

INF = float('inf')

weighted_matrix = [
    [0,   5,   3,   INF],  # 0 to 1 costs 5, 0 to 2 costs 3
    [5,   0,   INF, 2  ],  # 1 to 0 costs 5, 1 to 3 costs 2
    [3,   INF, 0,   4  ],  # 2 to 0 costs 3, 2 to 3 costs 4
    [INF, 2,   4,   0  ]   # 3 to 1 costs 2, 3 to 2 costs 4
]

# Check cost from 0 to 1
cost = weighted_matrix[0][1]  # 5
```

### 3. Edge List

**What it is:** Simple list of all edges.

**Advantages:**
- ✅ Very simple
- ✅ Easy to store/transmit

**Disadvantages:**
- ❌ Slow to find neighbors: O(E)

```python
# Edge List Representation
edges = [
    (0, 1),
    (0, 2),
    (1, 3),
    (2, 3)
]

# To find neighbors of vertex 0:
neighbors = [v for u, v in edges if u == 0]
print(neighbors)  # [1, 2]

# Weighted edge list
weighted_edges = [
    (0, 1, 5),   # (from, to, weight)
    (0, 2, 3),
    (1, 3, 2),
    (2, 3, 4)
]
```

### Comparison Table

| Representation | Space | Check Edge | Get Neighbors | Best For |
|----------------|-------|------------|---------------|----------|
| Adjacency List | O(V+E) | O(V) | O(1) avg | **Interviews** |
| Adjacency Matrix | O(V²) | O(1) | O(V) | Dense graphs |
| Edge List | O(E) | O(E) | O(E) | Simple storage |

---

## Graph Traversals

### 1. Breadth-First Search (BFS)

**What it is:** Explore graph level by level (like ripples in water).

**Key characteristics:**
- Uses **queue** (FIFO)
- Explores nearest neighbors first
- Finds **shortest path** in unweighted graphs

**Visual Example:**
```
Starting from 0:

    0
   / \
  1   2
  |   |
  3   4

Level 0: [0]
Level 1: [1, 2]  (neighbors of 0)
Level 2: [3, 4]  (neighbors of 1, 2)

Visit order: 0 → 1 → 2 → 3 → 4
```

**Template Code:**

```python
from collections import deque

def bfs(graph, start):
    """
    BFS traversal of graph
    
    Time: O(V + E) - visit each vertex once, check each edge once
    Space: O(V) - queue and visited set
    """
    visited = set()
    queue = deque([start])
    visited.add(start)
    result = []
    
    while queue:
        node = queue.popleft()  # Remove from front
        result.append(node)
        
        # Visit all neighbors
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result

# Test
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0, 4],
    3: [1],
    4: [2]
}

print(bfs(graph, 0))  # [0, 1, 2, 3, 4]
```

**Step-by-Step Trace:**

```python
# Detailed trace
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0, 4],
    3: [1],
    4: [2]
}

# Initial state
visited = {0}
queue = [0]
result = []

# Iteration 1: Process 0
node = 0
result = [0]
# Check neighbors: 1, 2
# Add 1: visited = {0, 1}, queue = [1]
# Add 2: visited = {0, 1, 2}, queue = [1, 2]

# Iteration 2: Process 1
node = 1
result = [0, 1]
# Check neighbors: 0 (visited), 3
# Add 3: visited = {0, 1, 2, 3}, queue = [2, 3]

# Iteration 3: Process 2
node = 2
result = [0, 1, 2]
# Check neighbors: 0 (visited), 4
# Add 4: visited = {0, 1, 2, 3, 4}, queue = [3, 4]

# Iteration 4: Process 3
node = 3
result = [0, 1, 2, 3]
# Check neighbors: 1 (visited)
# queue = [4]

# Iteration 5: Process 4
node = 4
result = [0, 1, 2, 3, 4]
# Check neighbors: 2 (visited)
# queue = []

# Done! result = [0, 1, 2, 3, 4]
```

**BFS for Shortest Path:**

```python
def bfs_shortest_path(graph, start, target):
    """
    Find shortest path using BFS
    
    Returns: list of nodes in path, or None if no path exists
    """
    if start == target:
        return [start]
    
    visited = {start}
    queue = deque([(start, [start])])  # (node, path_to_node)
    
    while queue:
        node, path = queue.popleft()
        
        for neighbor in graph.get(node, []):
            if neighbor == target:
                return path + [neighbor]
            
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    
    return None  # No path found

# Test
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0, 4],
    3: [1, 5],
    4: [2, 5],
    5: [3, 4]
}

path = bfs_shortest_path(graph, 0, 5)
print(path)  # [0, 1, 3, 5] or [0, 2, 4, 5]
```

### 2. Depth-First Search (DFS)

**What it is:** Explore as deep as possible before backtracking.

**Key characteristics:**
- Uses **stack** (or recursion)
- Explores one path completely before trying others
- Good for **detecting cycles**, **topological sort**

**Visual Example:**
```
Starting from 0:

    0
   / \
  1   2
  |   |
  3   4

DFS path: 0 → 1 → 3 (dead end, backtrack) → 2 → 4

Visit order: 0 → 1 → 3 → 2 → 4
(goes deep into left branch first)
```

**Template Code (Recursive):**

```python
def dfs_recursive(graph, start, visited=None):
    """
    DFS traversal (recursive)
    
    Time: O(V + E)
    Space: O(V) for recursion stack
    """
    if visited is None:
        visited = set()
    
    visited.add(start)
    result = [start]
    
    for neighbor in graph.get(start, []):
        if neighbor not in visited:
            result.extend(dfs_recursive(graph, neighbor, visited))
    
    return result

# Test
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0, 4],
    3: [1],
    4: [2]
}

print(dfs_recursive(graph, 0))  # [0, 1, 3, 2, 4]
```

**Template Code (Iterative):**

```python
def dfs_iterative(graph, start):
    """
    DFS traversal (iterative with stack)
    
    Time: O(V + E)
    Space: O(V) for stack
    """
    visited = set()
    stack = [start]
    result = []
    
    while stack:
        node = stack.pop()  # Remove from end (LIFO)
        
        if node not in visited:
            visited.add(node)
            result.append(node)
            
            # Add neighbors in reverse for consistent ordering
            for neighbor in reversed(graph.get(node, [])):
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return result

# Test
print(dfs_iterative(graph, 0))  # [0, 1, 3, 2, 4]
```

**Step-by-Step Trace:**

```python
# Detailed trace (iterative)
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0, 4],
    3: [1],
    4: [2]
}

# Initial state
visited = set()
stack = [0]
result = []

# Iteration 1: Process 0
node = 0
visited = {0}
result = [0]
# Push neighbors: stack = [2, 1] (reversed)

# Iteration 2: Process 1 (top of stack)
node = 1
visited = {0, 1}
result = [0, 1]
# Push neighbors: 0 (visited), 3
# stack = [2, 3]

# Iteration 3: Process 3
node = 3
visited = {0, 1, 3}
result = [0, 1, 3]
# Push neighbors: 1 (visited)
# stack = [2]

# Iteration 4: Process 2
node = 2
visited = {0, 1, 2, 3}
result = [0, 1, 3, 2]
# Push neighbors: 0 (visited), 4
# stack = [4]

# Iteration 5: Process 4
node = 4
visited = {0, 1, 2, 3, 4}
result = [0, 1, 3, 2, 4]
# Push neighbors: 2 (visited)
# stack = []

# Done! result = [0, 1, 3, 2, 4]
```

### BFS vs DFS Comparison

| Aspect | BFS | DFS |
|--------|-----|-----|
| Data Structure | Queue | Stack/Recursion |
| Order | Level by level | Deep first |
| Shortest Path | ✅ Yes (unweighted) | ❌ No |
| Memory | More (stores level) | Less (path only) |
| Cycle Detection | Possible | ✅ **Better** |
| Use Cases | Shortest path, level-order | Topological sort, cycles |

**When to Use BFS:**
- ✅ Finding shortest path
- ✅ Level-order traversal
- ✅ Finding all nodes at distance k

**When to Use DFS:**
- ✅ Detecting cycles
- ✅ Topological sorting
- ✅ Path existence
- ✅ Connected components

---

## LeetCode Problems

### Problem 1: Clone Graph

**🔗 LeetCode Link:** [133. Clone Graph](https://leetcode.com/problems/clone-graph/)

**Difficulty:** Medium

#### Problem Statement

Given a reference of a node in a **connected undirected graph**, return a **deep copy** (clone) of the graph.

Each node in the graph contains:
- A value (`val`)
- A list of its neighbors (`neighbors`)

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
```

**Example:**
```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]

Graph visualization:
   1 --- 2
   |     |
   4 --- 3

Output: A deep copy of the graph
```

#### Understanding Deep Copy vs Shallow Copy

**Shallow Copy:**
- Copies references only
- Changes in copy affect original

```python
# Shallow copy example
original = [1, 2, 3]
shallow = original  # Just copies reference!

shallow.append(4)
print(original)  # [1, 2, 3, 4] - Original changed!
```

**Deep Copy:**
- Creates completely new objects
- Changes in copy DON'T affect original

```python
import copy

original = [1, 2, 3]
deep = copy.deepcopy(original)  # Creates new list!

deep.append(4)
print(original)  # [1, 2, 3] - Original unchanged!
print(deep)      # [1, 2, 3, 4]
```

**For graphs:**
```
Original Graph:
Node1 (id: 0x1234) → Node2 (id: 0x5678)

Shallow Copy (WRONG):
CloneNode1 → Node2 (id: 0x5678)  # Still points to original!

Deep Copy (CORRECT):
CloneNode1 (id: 0xABCD) → CloneNode2 (id: 0xEF01)  # New objects!
```

#### Understanding Adjacency List Input

**Input format:** `adjList = [[2,4],[1,3],[2,4],[1,3]]`

**What this means:**
- Index represents node value (1-indexed)
- List at each index = that node's neighbors

```python
adjList = [[2,4], [1,3], [2,4], [1,3]]
#          node1   node2   node3   node4

# Node 1's neighbors: [2, 4]
# Node 2's neighbors: [1, 3]
# Node 3's neighbors: [2, 4]
# Node 4's neighbors: [1, 3]

# Visual:
#   1 --- 2
#   |     |
#   4 --- 3
```

#### Approach: BFS with HashMap

**💡 Key Insights:**
1. **Cannot clone during regular traversal** - neighbors might not exist yet!
2. **Use HashMap** to track original → clone mapping
3. **Two responsibilities:**
   - Create clone when first visiting
   - Update neighbors after clone exists

**Strategy:**
1. Use BFS to traverse graph
2. Create clone for each node on first visit
3. Update clone's neighbors using HashMap

**Why HashMap?**
```python
# Without HashMap (WRONG):
clone1 = Node(1)
clone2 = Node(2)
clone1.neighbors.append(clone2)
clone2.neighbors.append(clone1)  # Where is clone1?

# With HashMap (CORRECT):
clones = {}
clones[node1] = Node(1)
clones[node2] = Node(2)
clones[node1].neighbors.append(clones[node2])  # Easy lookup!
```

**Solution:**

```python
from collections import deque

class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def cloneGraph(node):
    """
    Clone graph using BFS
    
    Time: O(V + E) - visit each node once, check each edge
    Space: O(V) - queue and clones dictionary
    """
    if not node:
        return None
    
    # Step 1: Initialize BFS
    queue = deque([node])
    clones = {node: Node(node.val)}  # Map original → clone
    
    # Step 2: BFS traversal
    while queue:
        current = queue.popleft()
        current_clone = clones[current]
        
        # Step 3: Process each neighbor
        for neighbor in current.neighbors:
            # Create clone if not exists
            if neighbor not in clones:
                clones[neighbor] = Node(neighbor.val)
                queue.append(neighbor)
            
            # Add clone neighbor to current clone
            current_clone.neighbors.append(clones[neighbor])
    
    return clones[node]
```

**Detailed Walkthrough:**

```python
# Example graph:
#   1 --- 2
#   |     |
#   4 --- 3

# adjList = [[2,4], [1,3], [2,4], [1,3]]

# Initial state
queue = [Node1]
clones = {Node1: CloneNode1}

# ========== Iteration 1 ==========
# Process: Node1 (neighbors: [Node2, Node4])
current = Node1
current_clone = CloneNode1

# Process neighbor: Node2
# Not in clones → create clone
clones[Node2] = CloneNode2
queue.append(Node2)
# queue = [Node2]
CloneNode1.neighbors.append(CloneNode2)
# CloneNode1.neighbors = [CloneNode2]

# Process neighbor: Node4
# Not in clones → create clone
clones[Node4] = CloneNode4
queue.append(Node4)
# queue = [Node2, Node4]
CloneNode1.neighbors.append(CloneNode4)
# CloneNode1.neighbors = [CloneNode2, CloneNode4]

# ========== Iteration 2 ==========
# Process: Node2 (neighbors: [Node1, Node3])
current = Node2
current_clone = CloneNode2

# Process neighbor: Node1
# Already in clones!
CloneNode2.neighbors.append(CloneNode1)
# CloneNode2.neighbors = [CloneNode1]

# Process neighbor: Node3
# Not in clones → create clone
clones[Node3] = CloneNode3
queue.append(Node3)
# queue = [Node4, Node3]
CloneNode2.neighbors.append(CloneNode3)
# CloneNode2.neighbors = [CloneNode1, CloneNode3]

# ========== Iteration 3 ==========
# Process: Node4 (neighbors: [Node1, Node3])
current = Node4
current_clone = CloneNode4

# Process neighbor: Node1
# Already in clones!
CloneNode4.neighbors.append(CloneNode1)

# Process neighbor: Node3
# Already in clones!
CloneNode4.neighbors.append(CloneNode3)
# CloneNode4.neighbors = [CloneNode1, CloneNode3]

# ========== Iteration 4 ==========
# Process: Node3 (neighbors: [Node2, Node4])
current = Node3
current_clone = CloneNode3

# Both neighbors already in clones
CloneNode3.neighbors = [CloneNode2, CloneNode4]

# Done! Return clones[Node1]
```

**Visual Trace:**#### Alternative: DFS Solution

```python
def cloneGraph_DFS(node):
    """
    Clone graph using DFS (recursive)
    
    Time: O(V + E)
    Space: O(V) for recursion stack + clones dict
    """
    if not node:
        return None
    
    clones = {}
    
    def dfs(node):
        # If already cloned, return clone
        if node in clones:
            return clones[node]
        
        # Create clone
        clone = Node(node.val)
        clones[node] = clone
        
        # Recursively clone neighbors
        for neighbor in node.neighbors:
            clone.neighbors.append(dfs(neighbor))
        
        return clone
    
    return dfs(node)
```

#### Common Mistakes

```python
# ❌ MISTAKE 1: Not using HashMap
def cloneGraph_WRONG(node):
    queue = deque([node])
    
    while queue:
        current = queue.popleft()
        clone = Node(current.val)  # Creates new clone every time!
        # No way to find existing clones!

# ❌ MISTAKE 2: Updating neighbors too early
def cloneGraph_WRONG2(node):
    clones = {node: Node(node.val)}
    queue = deque([node])
    
    while queue:
        current = queue.popleft()
        for neighbor in current.neighbors:
            # Trying to add neighbor before it's cloned!
            clones[current].neighbors.append(clones[neighbor])  # KeyError!

# ✅ CORRECT: Check if neighbor cloned first
def cloneGraph_CORRECT(node):
    clones = {node: Node(node.val)}
    queue = deque([node])
    
    while queue:
        current = queue.popleft()
        for neighbor in current.neighbors:
            if neighbor not in clones:
                clones[neighbor] = Node(neighbor.val)
                queue.append(neighbor)
            # Now safe to add!
            clones[current].neighbors.append(clones[neighbor])
```

---

### Core Graph Operations

Before we move to more problems, let's cover essential graph operations.

#### Operation 1: Find Largest/Smallest Node

```python
def find_largest_node(graph, start):
    """
    Find node with largest value using BFS
    
    Time: O(V + E)
    Space: O(V)
    """
    if not graph or start not in graph:
        return None
    
    visited = {start}
    queue = deque([start])
    largest = start
    
    while queue:
        node = queue.popleft()
        
        # Update largest
        if node > largest:
            largest = node
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return largest

# Test
graph = {
    1: [2, 3],
    2: [1, 4],
    3: [1, 5],
    4: [2],
    5: [3]
}

print(find_largest_node(graph, 1))  # 5

# For smallest: change > to 
```

#### Operation 2: Detect Cycle

```python
def has_cycle_undirected(graph, start):
    """
    Detect cycle in UNDIRECTED graph using DFS
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set()
    
    def dfs(node, parent):
        visited.add(node)
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:
                # Visited neighbor that's not parent = cycle!
                return True
        
        return False
    
    return dfs(start, None)

# Test
graph_with_cycle = {
    1: [2, 3],
    2: [1, 3],
    3: [1, 2]  # Triangle = cycle
}
print(has_cycle_undirected(graph_with_cycle, 1))  # True

graph_no_cycle = {
    1: [2, 3],
    2: [1],
    3: [1]  # Tree = no cycle
}
print(has_cycle_undirected(graph_no_cycle, 1))  # False
```

```python
def has_cycle_directed(graph):
    """
    Detect cycle in DIRECTED graph
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set()
    rec_stack = set()  # Current DFS path
    
    def dfs(node):
        visited.add(node)
        rec_stack.add(node)
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                if dfs(neighbor):
                    return True
            elif neighbor in rec_stack:
                # Back edge = cycle!
                return True
        
        rec_stack.remove(node)
        return False
    
    for node in graph:
        if node not in visited:
            if dfs(node):
                return True
    
    return False

# Test
directed_cycle = {
    'A': ['B'],
    'B': ['C'],
    'C': ['A']  # A→B→C→A = cycle
}
print(has_cycle_directed(directed_cycle))  # True

directed_no_cycle = {
    'A': ['B'],
    'B': ['C'],
    'C': []  # No cycle
}
print(has_cycle_directed(directed_no_cycle))  # False
```

#### Operation 3: Count Edges

```python
def count_edges_from_adjacency_list(graph):
    """
    Count edges from adjacency list
    
    For undirected: divide by 2 (each edge counted twice)
    For directed: count directly
    
    Time: O(V)
    Space: O(1)
    """
    total = sum(len(neighbors) for neighbors in graph.values())
    
    # For undirected graph, each edge counted twice
    return total // 2  # Undirected
    # return total  # For directed

# Test
undirected_graph = {
    1: [2, 3],
    2: [1, 3],
    3: [1, 2]
}
print(count_edges_from_adjacency_list(undirected_graph))  # 3

# Edges: 1-2, 1-3, 2-3
# In adjacency list: counted as 1→2, 2→1, 1→3, 3→1, 2→3, 3→2 = 6
# Divide by 2 = 3 actual edges
```

#### Operation 4: Find All Paths

```python
def find_all_paths(graph, start, end, path=[]):
    """
    Find all paths from start to end
    
    Time: O(V!) worst case (exponential)
    Space: O(V) for recursion
    """
    path = path + [start]
    
    if start == end:
        return [path]
    
    if start not in graph:
        return []
    
    paths = []
    for neighbor in graph[start]:
        if neighbor not in path:  # Avoid cycles
            new_paths = find_all_paths(graph, neighbor, end, path)
            paths.extend(new_paths)
    
    return paths

# Test
graph = {
    'A': ['B', 'C'],
    'B': ['D'],
    'C': ['D'],
    'D': []
}

all_paths = find_all_paths(graph, 'A', 'D')
print(all_paths)
# [['A', 'B', 'D'], ['A', 'C', 'D']]
```

---

### Problem 2: Cheapest Flights Within K Stops

**🔗 LeetCode Link:** [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

**Difficulty:** Medium

#### Problem Statement

There are `n` cities connected by some number of flights. You are given:
- `flights[i] = [from, to, price]` where there's a flight from city `from` to city `to` with cost `price`
- Three integers: `src` (source city), `dst` (destination city), and `k` (maximum stops)

Return the **cheapest price** from `src` to `dst` with at most `k` stops. Return `-1` if no such route exists.

**Example:**
```
Input: 
n = 4
flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]
src = 0, dst = 3, k = 1

Visualization:
   0 --100--> 1
   ^          |
  100       100,600
   |          ↓
   2 <--200-- 3

Output: 700
Explanation: 0 → 1 → 3 costs 700 (1 stop)
Alternative: 0 → 1 → 2 → 3 costs 400 (2 stops, exceeds k=1)
```

#### Understanding the Problem

**Key Points:**
1. **K stops** = K intermediate cities (not including src and dst)
2. **Weighted directed graph** - prices differ by direction
3. **Not asking for shortest path** - asking for cheapest within constraint

**Examples of K stops:**
```
k = 0: Direct flight only
  src → dst

k = 1: At most 1 intermediate city
  src → city1 → dst

k = 2: At most 2 intermediate cities
  src → city1 → city2 → dst
```

#### Approach 1: Bellman-Ford Algorithm (Optimal)

**What is Bellman-Ford?**
- Algorithm for finding shortest paths in weighted graphs
- Works with **negative weights** (unlike Dijkstra)
- Based on **relaxation** principle

**What is Relaxation?**
Updating shortest path estimate when we find a better path.

```python
# Example of relaxation
prices = [inf, inf, inf]  # Cost to reach each city
prices[0] = 0  # Start city costs 0

# Check edge: 0 → 1, cost 100
if prices[0] + 100 < prices[1]:  # 0 + 100 < inf
    prices[1] = 100  # Relax! Update to better price

# Check edge: 0 → 2, cost 500
if prices[0] + 500 < prices[2]:  # 0 + 500 < inf
    prices[2] = 500  # Relax!

# Later, check edge: 1 → 2, cost 100
if prices[1] + 100 < prices[2]:  # 100 + 100 < 500
    prices[2] = 200  # Relax again! Found cheaper path
```

**Why K+1 iterations?**
```
k = 0 (direct flight):
  Need 1 iteration to check direct edges

k = 1 (1 stop):
  Need 2 iterations
  - Iteration 1: Find cities reachable in 1 hop
  - Iteration 2: Find cities reachable in 2 hops (with 1 stop)

k = 2 (2 stops):
  Need 3 iterations
  - Iteration 1: 1 hop
  - Iteration 2: 2 hops
  - Iteration 3: 3 hops (with 2 stops)

Pattern: k stops = k+1 hops = k+1 iterations
```

**Why temporary prices table?**

```python
# WITHOUT temp table (WRONG):
prices = [0, inf, inf]

# Iteration 1:
# Edge 0→1: prices[1] = 100
# Edge 1→2: prices[2] = prices[1] + cost = 100 + cost
# Problem: Used updated value in same iteration!
# This gives us 2 hops in 1 iteration!

# WITH temp table (CORRECT):
prices = [0, inf, inf]
temp = prices.copy()

# Iteration 1:
# Edge 0→1: temp[1] = prices[0] + cost  # Uses old prices
# Edge 1→2: temp[2] = prices[1] + cost  # Uses old prices
# After iteration: prices = temp
# Now we correctly process 1 hop per iteration!
```

**Solution:**

```python
def findCheapestPrice(n, flights, src, dst, k):
    """
    Bellman-Ford algorithm for cheapest flight
    
    Time: O(k * E) where E = number of flights
    Space: O(n) for prices array
    """
    # Step 1: Initialize prices
    prices = [float('inf')] * n
    prices[src] = 0  # Cost to reach source is 0
    
    # Step 2: Relax edges k+1 times
    for i in range(k + 1):
        # Create temporary copy
        temp_prices = prices.copy()
        
        # Check all flights
        for from_city, to_city, price in flights:
            # Skip if source city not reachable yet
            if prices[from_city] == float('inf'):
                continue
            
            # Relax edge if found cheaper path
            new_price = prices[from_city] + price
            if new_price < temp_prices[to_city]:
                temp_prices[to_city] = new_price
        
        # Update prices
        prices = temp_prices
    
    # Step 3: Return result
    return prices[dst] if prices[dst] != float('inf') else -1

# Test
n = 4
flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]
src, dst, k = 0, 3, 1
print(findCheapestPrice(n, flights, src, dst, k))  # 700
```

**Detailed Walkthrough:**

```python
# Example: n=4, flights=[[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]
# src=0, dst=3, k=1

# Initial:
prices = [0, inf, inf, inf]
#         0   1    2    3

# ========== Iteration 1 (i=0) ==========
temp_prices = [0, inf, inf, inf]

# Check flight [0,1,100]:
# prices[0]=0, 0+100=100 < inf
temp_prices[1] = 100
# temp = [0, 100, inf, inf]

# Check flight [1,2,100]:
# prices[1]=inf, skip

# Check flight [2,0,100]:
# prices[2]=inf, skip

# Check flight [1,3,600]:
# prices[1]=inf, skip

# Check flight [2,3,200]:
# prices[2]=inf, skip

# Update prices
prices = [0, 100, inf, inf]

# ========== Iteration 2 (i=1) ==========
temp_prices = [0, 100, inf, inf]

# Check flight [0,1,100]:
# 0+100=100, not less than temp[1]=100
# temp = [0, 100, inf, inf]

# Check flight [1,2,100]:
# prices[1]=100, 100+100=200 < inf
temp_prices[2] = 200
# temp = [0, 100, 200, inf]

# Check flight [2,0,100]:
# prices[2]=inf, skip

# Check flight [1,3,600]:
# prices[1]=100, 100+600=700 < inf
temp_prices[3] = 700
# temp = [0, 100, 200, 700]

# Check flight [2,3,200]:
# prices[2]=inf, skip

# Update prices
prices = [0, 100, 200, 700]

# Done! (k=1, so k+1=2 iterations)
# Answer: prices[3] = 700
```

**Visual Trace:**#### Alternative: Modified Dijkstra's Algorithm

```python
import heapq

def findCheapestPrice_Dijkstra(n, flights, src, dst, k):
    """
    Modified Dijkstra's with stop constraint
    
    Time: O(E * k * log(E * k))
    Space: O(n + E)
    """
    # Build graph
    graph = {i: [] for i in range(n)}
    for from_city, to_city, price in flights:
        graph[from_city].append((to_city, price))
    
    # Min-heap: (cost, city, stops_used)
    heap = [(0, src, 0)]
    visited = {}  # (city, stops) -> min_cost
    
    while heap:
        cost, city, stops = heapq.heappop(heap)
        
        # Reached destination
        if city == dst:
            return cost
        
        # Exceeded stops
        if stops > k:
            continue
        
        # Already visited with fewer stops and less cost
        if (city, stops) in visited:
            continue
        visited[(city, stops)] = cost
        
        # Explore neighbors
        for next_city, price in graph[city]:
            new_cost = cost + price
            heapq.heappush(heap, (new_cost, next_city, stops + 1))
    
    return -1
```

---

### Problem 3: Course Schedule

**🔗 LeetCode Link:** [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

**Difficulty:** Medium

#### Problem Statement

There are `numCourses` courses labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates you must take course `bi` first before course `ai`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: Take course 0, then course 1
```

**Example 2:**
```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: Circular dependency! 
To take course 1, need course 0
To take course 0, need course 1
Impossible!
```

#### Understanding the Problem

**Key Insight:** This is a **cycle detection** problem in a directed graph!

**Why?**
- Courses = vertices
- Prerequisites = directed edges
- Circular dependency = cycle
- If cycle exists → impossible to complete courses

**Visual Examples:**

```
Example 1: No Cycle (Possible)
prerequisites = [[1,0]]

  0 → 1

Can complete: 0 then 1 ✅

Example 2: Cycle (Impossible)
prerequisites = [[1,0], [0,1]]

  0 → 1
  ↑   ↓
  └───┘

Cannot complete: circular dependency! ❌

Example 3: Complex No Cycle
prerequisites = [[1,0], [2,0], [3,1], [3,2]]

    0
   / \
  1   2
   \ /
    3

Can complete: 0 → 1 → 2 → 3 ✅
Or: 0 → 2 → 1 → 3 ✅
```

#### Approach 1: DFS with Cycle Detection (Iterative)

**Strategy:**
1. Build adjacency list from prerequisites
2. For each course, do DFS to check for cycles
3. Track visited nodes to detect back edges
4. If back edge found = cycle = impossible

**What's a back edge?**
An edge that points to an ancestor in the current DFS path.

```
DFS Path: A → B → C
Back edge: C → A (points back to ancestor)

Result: Cycle detected!
```

**Solution:**

```python
from collections import defaultdict

def canFinish(numCourses, prerequisites):
    """
    Check if can complete all courses using DFS cycle detection
    
    Time: O(V + E) where V = courses, E = prerequisites
    Space: O(V + E) for graph + visited set
    """
    # Step 1: Build adjacency list
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[course].append(prereq)
    
    # Step 2: Check each course for cycles
    for course in range(numCourses):
        # Use iterative DFS with stack
        stack = [(course, set())]  # (course, visited_in_path)
        
        while stack:
            current, visited = stack.pop()
            
            # If already visited in this path = cycle!
            if current in visited:
                return False
            
            # Add to visited for this path
            visited = visited.copy()
            visited.add(current)
            
            # Explore prerequisites
            for prereq in graph[current]:
                stack.append((prereq, visited))
    
    return True

# Test
print(canFinish(2, [[1,0]]))  # True
print(canFinish(2, [[1,0],[0,1]]))  # False
```

**Detailed Walkthrough:**

```python
# Example: numCourses = 5, prerequisites = [[1,0], [2,1], [3,2], [1,3]]
#
# Graph:
#   0 → 1 → 2 → 3
#       ↑       ↓
#       └───────┘
# Cycle: 1 → 2 → 3 → 1

# Build graph
graph = {
    1: [0, 3],  # To take 1, need 0 and 3
    2: [1],     # To take 2, need 1
    3: [2]      # To take 3, need 2
}

# Check course 0:
# No prerequisites, no cycle

# Check course 1:
stack = [(1, {})]

# Iteration 1:
current = 1, visited = {}
visited = {1}
# Push prerequisites: 0 and 3
stack = [(0, {1}), (3, {1})]

# Iteration 2:
current = 3, visited = {1}
visited = {1, 3}
# Push prerequisite: 2
stack = [(0, {1}), (2, {1, 3})]

# Iteration 3:
current = 2, visited = {1, 3}
visited = {1, 2, 3}
# Push prerequisite: 1
stack = [(0, {1}), (1, {1, 2, 3})]

# Iteration 4:
current = 1, visited = {1, 2, 3}
# 1 already in visited! CYCLE DETECTED!
return False
```

#### Approach 2: DFS with Optimizations (Recommended)

**Better approach:**
- Use global visited set (don't revisit processed courses)
- Use recursion stack for current path

```python
def canFinish_optimized(numCourses, prerequisites):
    """
    Optimized DFS with global visited tracking
    
    Time: O(V + E)
    Space: O(V + E)
    """
    # Build graph
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[course].append(prereq)
    
    visited = set()  # Globally visited (safe courses)
    rec_stack = set()  # Current DFS path
    
    def has_cycle(course):
        # Already processed this course
        if course in visited:
            return False
        
        # Found in current path = cycle!
        if course in rec_stack:
            return True
        
        # Add to current path
        rec_stack.add(course)
        
        # Check all prerequisites
        for prereq in graph[course]:
            if has_cycle(prereq):
                return True
        
        # Remove from current path
        rec_stack.remove(course)
        
        # Mark as safe
        visited.add(course)
        return False
    
    # Check all courses
    for course in range(numCourses):
        if has_cycle(course):
            return False
    
    return True

# Test
print(canFinish_optimized(2, [[1,0]]))  # True
print(canFinish_optimized(2, [[1,0],[0,1]]))  # False
```

**Trace with Colors:**Due to length constraints, I'll continue with the remaining content in a structured summary format:

---

## Additional Important Graph Concepts

### 1. Topological Sort

**What is it?** Ordering of vertices in a DAG such that for every edge u→v, u comes before v.

**Use cases:**
- Task scheduling with dependencies
- Build systems (compile order)
- Course prerequisites

**🔗 LeetCode:** [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

```python
def topologicalSort(numCourses, prerequisites):
    """
    Time: O(V + E), Space: O(V + E)
    """
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[prereq].append(course)  # Reverse direction
    
    visited = set()
    result = []
    
    def dfs(course):
        if course in visited:
            return
        visited.add(course)
        for next_course in graph[course]:
            dfs(next_course)
        result.append(course)
    
    for course in range(numCourses):
        dfs(course)
    
    return result[::-1]
```

### 2. Union-Find (Disjoint Set)

**🔗 LeetCode:** 
- [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

### 3. Shortest Path Algorithms

- **Dijkstra's:** Single source, non-negative weights
- **Bellman-Ford:** Single source, can handle negative weights
- **Floyd-Warshall:** All pairs shortest path

---

## More Practice Problems

### Easy
1. **[997. Find the Town Judge](https://leetcode.com/problems/find-the-town-judge/)** - Graph properties
2. **[1971. Find if Path Exists](https://leetcode.com/problems/find-if-path-exists-in-graph/)** - Basic BFS/DFS

### Medium
3. **[200. Number of Islands](https://leetcode.com/problems/number-of-islands/)** - DFS/BFS on grid
4. **[207. Course Schedule](https://leetcode.com/problems/course-schedule/)** - Cycle detection
5. **[210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)** - Topological sort
6. **[323. Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)** - Union-Find
7. **[417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)** - Multi-source BFS
8. **[787. Cheapest Flights](https://leetcode.com/problems/cheapest-flights-within-k-stops/)** - Bellman-Ford

### Hard
9. **[127. Word Ladder](https://leetcode.com/problems/word-ladder/)** - BFS transformation
10. **[269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)** - Topological sort
11. **[329. Longest Increasing Path](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)** - DFS + memoization
