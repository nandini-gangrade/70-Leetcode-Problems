# Complete Binary Trees Guide for Interview Preparation

## Table of Contents
1. [What is a Tree?](#what-is-a-tree)
2. [Binary Trees Fundamentals](#binary-trees-fundamentals)
3. [Tree Types](#tree-types)
4. [Tree Traversal Methods](#tree-traversal-methods)
5. [Problems](#problems)

---

## What is a Tree?

A **tree** is a hierarchical data structure consisting of nodes connected by edges. Unlike linear data structures (arrays, linked lists, stacks, queues), trees have a hierarchical relationship.

### Real-World Analogies

**1. Family Tree:**
```
        Grandparent
       /           \
    Parent1      Parent2
    /    \         /
Child1 Child2  Child3
```

**2. Organization Chart:**
```
        CEO
       /   \
    CTO    CFO
    /  \
Dev1  Dev2
```

**3. File System:**
```
        Root/
       /     \
   Users/   Programs/
    /  \
John/ Mary/
```

### Tree Terminology

**Visual Example:**
```
         1        ← Root (no parent)
        / \
       2   3      ← Internal nodes (have children)
      / \   \
     4   5   6    ← Leaf nodes (no children)
```

**Key Terms:**

| Term | Definition | Example |
|------|------------|---------|
| **Root** | Top node with no parent | Node 1 |
| **Parent** | Node with children | Node 2 is parent of 4, 5 |
| **Child** | Node with a parent | Node 4 is child of 2 |
| **Leaf** | Node with no children | Nodes 4, 5, 6 |
| **Internal Node** | Node with at least one child | Nodes 1, 2, 3 |
| **Edge** | Connection between nodes | Link from 1 to 2 |
| **Path** | Sequence of nodes | 1 → 2 → 4 |
| **Height** | Longest path from node to leaf | Height of tree = 2 |
| **Depth/Level** | Distance from root to node | Depth of node 4 = 2 |
| **Subtree** | Tree formed by a node and descendants | Subtree rooted at 2 |

**Detailed Explanation:**

```
         1        Level 0 (Root)
        / \
       2   3      Level 1
      / \   \
     4   5   6    Level 2 (All leaves)

Height of tree: 2 (longest path: 1→2→4)
Depth of node 4: 2 (distance from root)
Degree of node 2: 2 (has 2 children)

Subtree rooted at 2:
    2
   / \
  4   5
```

---

## Binary Trees Fundamentals

A **binary tree** is a tree where each node has **at most 2 children**, typically called **left** and **right**.

### Binary Tree Node Structure

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val        # Value stored in node
        self.left = left      # Left child (TreeNode or None)
        self.right = right    # Right child (TreeNode or None)
```

**Visual:**
```
    TreeNode
    ┌─────────┐
    │  val: 5 │
    ├─────────┤
    │ left: • │ ────→ Left subtree
    │ right:• │ ────→ Right subtree
    └─────────┘
```

### Creating a Binary Tree

```python
# Creating this tree:
#      1
#     / \
#    2   3
#   /
#  4

# Method 1: Bottom-up
node4 = TreeNode(4)
node2 = TreeNode(2, node4, None)
node3 = TreeNode(3)
root = TreeNode(1, node2, node3)

# Method 2: Top-down
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
```

### Why Binary Trees?

**Advantages:**
1. **Efficient searching** - O(log n) in balanced trees
2. **Hierarchical structure** - Natural for many problems
3. **Fast insertion/deletion** - O(log n) in balanced trees

**Applications:**
- **File systems** - Directory structure
- **Databases** - B-trees for indexing
- **Compilers** - Abstract syntax trees
- **AI** - Decision trees
- **Graphics** - Scene graphs

---

## Tree Types

Understanding different tree types is crucial for choosing the right approach!

### 1. Full Binary Tree

**Definition:** Every node has either 0 or 2 children (no node has only 1 child).

```
Full Binary Tree ✓:
       1
      / \
     2   3
    / \
   4   5

NOT Full ✗:
       1
      / \
     2   3
    /         ← Node 2 has only 1 child!
   4
```

**Properties:**
- Every internal node has exactly 2 children
- All leaves can be at different levels
- If tree has n nodes: n = 2h + 1 (where h = height)

**Code to check:**
```python
def isFullBinaryTree(root):
    if not root:
        return True
    
    # If leaf node
    if not root.left and not root.right:
        return True
    
    # If both children exist
    if root.left and root.right:
        return (isFullBinaryTree(root.left) and 
                isFullBinaryTree(root.right))
    
    # If only one child exists
    return False
```

### 2. Complete Binary Tree

**Definition:** All levels are completely filled except possibly the last level, which is filled from left to right.

```
Complete Binary Tree ✓:
       1
      / \
     2   3
    / \  /
   4  5 6

NOT Complete ✗:
       1
      / \
     2   3
      \  /      ← Gap in left side!
       5 6
```

**Properties:**
- All levels filled except last
- Last level filled from left to right
- Used in **heap** data structure
- Height = floor(log₂(n))

**Why important:**
- Can be efficiently stored in array!
- No wasted space

**Array representation:**
```
Tree:
       1
      / \
     2   3
    / \
   4   5

Array: [_, 1, 2, 3, 4, 5]
Index:  0  1  2  3  4  5

For node at index i:
- Left child: 2*i
- Right child: 2*i + 1
- Parent: i//2
```

**Code to check:**
```python
def isCompleteBinaryTree(root):
    if not root:
        return True
    
    from collections import deque
    queue = deque([root])
    flag = False  # Flag for null node
    
    while queue:
        node = queue.popleft()
        
        if node:
            if flag:  # Found non-null after null
                return False
            queue.append(node.left)
            queue.append(node.right)
        else:
            flag = True  # Found null
    
    return True
```

### 3. Perfect Binary Tree

**Definition:** All internal nodes have 2 children AND all leaves are at the same level.

```
Perfect Binary Tree ✓:
       1
      / \
     2   3
    / \ / \
   4  5 6  7

NOT Perfect ✗:
       1
      / \
     2   3
    / \        ← Leaves at different levels!
   4   5
```

**Properties:**
- Most restrictive type
- Number of nodes = 2^(h+1) - 1
- Number of leaves = 2^h
- Extremely balanced

**Example calculations:**
```
Height 0: 1 node  (2^1 - 1 = 1)
Height 1: 3 nodes (2^2 - 1 = 3)
Height 2: 7 nodes (2^3 - 1 = 7)
Height 3: 15 nodes (2^4 - 1 = 15)
```

**Code to check:**
```python
def isPerfectBinaryTree(root):
    def depth(node):
        d = 0
        while node:
            d += 1
            node = node.left
        return d
    
    def isPerfect(node, d, level=0):
        if not node:
            return True
        
        # If leaf, check if at expected depth
        if not node.left and not node.right:
            return d == level + 1
        
        # If only one child, not perfect
        if not node.left or not node.right:
            return False
        
        return (isPerfect(node.left, d, level + 1) and
                isPerfect(node.right, d, level + 1))
    
    d = depth(root)
    return isPerfect(root, d)
```

### 4. Balanced Binary Tree

**Definition:** Height of left and right subtrees differ by at most 1 for every node.

```
Balanced ✓:
       1
      / \
     2   3
    / \
   4   5

Height difference at every node ≤ 1


NOT Balanced ✗:
       1
      /
     2
    /
   3
  /
 4

Left subtree height = 3
Right subtree height = 0
Difference = 3 (> 1)
```

**Why important:**
- Ensures O(log n) operations
- Prevents degeneration to linked list
- AVL and Red-Black trees maintain balance

**Visual comparison:**
```
Balanced (Height 2):          Unbalanced (Height 4):
       1                             1
      / \                           /
     2   3                         2
    / \                           /
   4   5                         3
                                /
                               4

Operations: O(log n)          Operations: O(n)
```

**Code to check:**
```python
def isBalanced(root):
    def height(node):
        if not node:
            return 0
        
        left_height = height(node.left)
        if left_height == -1:
            return -1
        
        right_height = height(node.right)
        if right_height == -1:
            return -1
        
        # Check balance
        if abs(left_height - right_height) > 1:
            return -1
        
        return max(left_height, right_height) + 1
    
    return height(root) != -1
```

### 5. Degenerate (Skewed) Tree

**Definition:** Every internal node has only one child. Essentially a linked list!

```
Left-Skewed:         Right-Skewed:
    1                    1
   /                      \
  2                        2
 /                          \
3                            3

This is basically a linked list!
```

**Properties:**
- Height = n - 1 (worst case)
- Operations: O(n)
- Loses benefits of tree structure
- Should be avoided!

**When does this happen?**
```
Inserting sorted data into BST:

Insert: 1, 2, 3, 4, 5

Result:
1
 \
  2
   \
    3
     \
      4
       \
        5

Solution: Use self-balancing trees (AVL, Red-Black)
```

### Comparison Table

| Type | All Levels Full? | Leaves Same Level? | Max 2 Children? | Example Use |
|------|------------------|-------------------|-----------------|-------------|
| Full | No | No | Yes | Expression trees |
| Complete | Almost | No | Yes | Heaps |
| Perfect | Yes | Yes | Yes | Theoretical analysis |
| Balanced | No | No | Yes | Search trees |
| Degenerate | No | No | No (effectively 1) | Avoid! |

---

## Tree Traversal Methods

**Traversal** = Visiting every node in the tree exactly once in a specific order.

### Why Multiple Traversal Methods?

Different traversals serve different purposes:

```
         1
        / \
       2   3
      / \
     4   5

BFS (Level Order): [1, 2, 3, 4, 5]
Use: Level-by-level processing

DFS Preorder: [1, 2, 4, 5, 3]
Use: Copy tree, serialize tree

DFS Inorder: [4, 2, 5, 1, 3]
Use: Get sorted sequence from BST

DFS Postorder: [4, 5, 2, 3, 1]
Use: Delete tree, calculate directory size
```

### 1. BFS (Breadth-First Search)

**Also called:** Level Order Traversal

**Concept:** Visit nodes level by level, left to right.

**Uses:** Queue (FIFO)

**Visual:**
```
         1          Level 0
        / \
       2   3        Level 1
      / \   \
     4   5   6      Level 2

Visit order: 1, 2, 3, 4, 5, 6
```

**How it works:**
```
1. Start with root in queue: [1]
2. Process level by level:
   
   Queue: [1]
   Dequeue 1, add children: [2, 3]
   
   Queue: [2, 3]
   Dequeue 2, add children: [3, 4, 5]
   
   Queue: [3, 4, 5]
   Dequeue 3, add children: [4, 5, 6]
   
   Queue: [4, 5, 6]
   Process remaining leaves
```

**Implementation:**
```python
from collections import deque

def bfs(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        node = queue.popleft()  # Visit node
        result.append(node.val)
        
        # Add children to queue
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    
    return result
```

**Detailed trace:**
```python
Tree:
         1
        / \
       2   3
      / \
     4   5

Step-by-step:

Initial:
queue = [1]
result = []

Iteration 1:
Dequeue 1
result = [1]
Add 2, 3
queue = [2, 3]

Iteration 2:
Dequeue 2
result = [1, 2]
Add 4, 5
queue = [3, 4, 5]

Iteration 3:
Dequeue 3
result = [1, 2, 3]
No children
queue = [4, 5]

Iteration 4:
Dequeue 4
result = [1, 2, 3, 4]
No children
queue = [5]

Iteration 5:
Dequeue 5
result = [1, 2, 3, 4, 5]
queue = []

Return [1, 2, 3, 4, 5]
```

**When to use BFS:**
- Find shortest path
- Level-order operations
- Find minimum depth
- Check if tree is complete

### 2. DFS (Depth-First Search)

**Concept:** Go deep before going wide - explore one branch completely before backtracking.

**Uses:** Stack (explicit or recursion call stack)

**Three types:**
1. **Preorder** (Root → Left → Right)
2. **Inorder** (Left → Root → Right)
3. **Postorder** (Left → Right → Root)

#### 2.1 Preorder Traversal

**Order:** Root → Left → Right

**Visual:**
```
         1          ← Visit root first
        / \
       2   3        ← Then left subtree
      / \
     4   5

Result: [1, 2, 4, 5, 3]

Process:
1. Visit 1
2. Go left to 2
3. Go left to 4 (leaf)
4. Backtrack to 2, go right to 5 (leaf)
5. Backtrack to 1, go right to 3 (leaf)
```

**Recursive implementation:**
```python
def preorder(root):
    if not root:
        return []
    
    result = []
    result.append(root.val)              # Root
    result.extend(preorder(root.left))   # Left
    result.extend(preorder(root.right))  # Right
    
    return result
```

**Iterative implementation:**
```python
def preorder_iterative(root):
    if not root:
        return []
    
    result = []
    stack = [root]
    
    while stack:
        node = stack.pop()
        result.append(node.val)
        
        # Push right first (so left is processed first)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    
    return result
```

**Detailed trace (iterative):**
```python
Tree:
         1
        / \
       2   3
      /
     4

Initial:
stack = [1]
result = []

Iteration 1:
Pop 1
result = [1]
Push 3 (right)
Push 2 (left)
stack = [3, 2]
        ↑
      Top

Iteration 2:
Pop 2
result = [1, 2]
Push 4 (left)
stack = [3, 4]

Iteration 3:
Pop 4
result = [1, 2, 4]
No children
stack = [3]

Iteration 4:
Pop 3
result = [1, 2, 4, 3]
No children
stack = []

Return [1, 2, 4, 3]
```

**Use cases:**
- Create copy of tree
- Serialize tree
- Prefix expression evaluation

#### 2.2 Inorder Traversal

**Order:** Left → Root → Right

**Visual:**
```
         1
        / \
       2   3
      / \
     4   5

Result: [4, 2, 5, 1, 3]

Process:
1. Go left to 2
2. Go left to 4 (leaf) - visit 4
3. Backtrack to 2 - visit 2
4. Go right to 5 (leaf) - visit 5
5. Backtrack to 1 - visit 1
6. Go right to 3 (leaf) - visit 3
```

**Recursive implementation:**
```python
def inorder(root):
    if not root:
        return []
    
    result = []
    result.extend(inorder(root.left))    # Left
    result.append(root.val)              # Root
    result.extend(inorder(root.right))   # Right
    
    return result
```

**Iterative implementation:**
```python
def inorder_iterative(root):
    result = []
    stack = []
    current = root
    
    while current or stack:
        # Go to leftmost node
        while current:
            stack.append(current)
            current = current.left
        
        # Process node
        current = stack.pop()
        result.append(current.val)
        
        # Go to right subtree
        current = current.right
    
    return result
```

**Detailed trace (iterative):**
```python
Tree:
         1
        / \
       2   3
      / \
     4   5

Initial:
stack = []
current = 1
result = []

Step 1: Go to leftmost
current = 1 → stack = [1], current = 2
current = 2 → stack = [1, 2], current = 4
current = 4 → stack = [1, 2, 4], current = None

Step 2: Process leftmost
Pop 4: result = [4]
current = None (no right child)

Step 3: Process parent
Pop 2: result = [4, 2]
current = 5 (right child of 2)

Step 4: Go left (none), process
current = 5 → stack = [1, 5], current = None
Pop 5: result = [4, 2, 5]
current = None

Step 5: Process root
Pop 1: result = [4, 2, 5, 1]
current = 3 (right child of 1)

Step 6: Process rightmost
current = 3 → stack = [3], current = None
Pop 3: result = [4, 2, 5, 1, 3]

Return [4, 2, 5, 1, 3]
```

**Use cases:**
- **Get sorted sequence from BST** (most important!)
- Validate BST
- Find kth smallest element

**Why inorder gives sorted sequence in BST:**
```
BST:
         4
        / \
       2   6
      / \ / \
     1  3 5  7

Inorder: [1, 2, 3, 4, 5, 6, 7] ← Sorted!

Because we visit:
1. All smaller values (left subtree)
2. Root
3. All larger values (right subtree)
```

#### 2.3 Postorder Traversal

**Order:** Left → Right → Root

**Visual:**
```
         1
        / \
       2   3
      / \
     4   5

Result: [4, 5, 2, 3, 1]

Process:
1. Go left to 2
2. Go left to 4 (leaf) - visit 4
3. Backtrack to 2, go right to 5 (leaf) - visit 5
4. Backtrack to 2 - visit 2
5. Backtrack to 1, go right to 3 (leaf) - visit 3
6. Finally visit 1
```

**Recursive implementation:**
```python
def postorder(root):
    if not root:
        return []
    
    result = []
    result.extend(postorder(root.left))   # Left
    result.extend(postorder(root.right))  # Right
    result.append(root.val)               # Root
    
    return result
```

**Iterative implementation:**
```python
def postorder_iterative(root):
    if not root:
        return []
    
    result = []
    stack = [root]
    
    while stack:
        node = stack.pop()
        result.append(node.val)
        
        # Push left first (so right is processed first)
        if node.left:
            stack.append(node.left)
        if node.right:
            stack.append(node.right)
    
    return result[::-1]  # Reverse result
```

**Why reverse?**
```
We're actually doing reverse postorder:
Root → Right → Left

Then reverse to get:
Left → Right → Root

Example:
         1
        / \
       2   3

Process: 1, 3, 2
Reverse: 2, 3, 1 ✓ (correct postorder)
```

**Use cases:**
- Delete tree (delete children before parent)
- Calculate directory size
- Expression tree evaluation

### Traversal Comparison

```
Tree:
         1
        / \
       2   3
      / \
     4   5

BFS:       [1, 2, 3, 4, 5]
Preorder:  [1, 2, 4, 5, 3]
Inorder:   [4, 2, 5, 1, 3]
Postorder: [4, 5, 2, 3, 1]
```

**Memory usage:**
```
BFS: O(w) where w = max width
     Worst: O(n) for complete tree

DFS: O(h) where h = height
     Best: O(log n) for balanced tree
     Worst: O(n) for skewed tree
```

**Which to use?**
```
Need shortest path? → BFS
Need to explore all paths? → DFS
BST and need sorted? → Inorder DFS
Need to delete tree? → Postorder DFS
Need to copy tree? → Preorder DFS
```

---

# Binary Tree Problems

## Problems

### Problem 1: Average of Levels in Binary Tree

**[LeetCode 637 - Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)**

#### Problem Statement

Given the `root` of a binary tree, return the average value of the nodes on each level in the form of an array.

**Example 1:**
```
Input: root = [3,9,20,null,null,15,7]

Tree:
      3
     / \
    9  20
      /  \
     15   7

Output: [3.00000, 14.50000, 11.00000]

Explanation:
Level 0: Average of [3] = 3
Level 1: Average of [9, 20] = (9 + 20) / 2 = 14.5
Level 2: Average of [15, 7] = (15 + 7) / 2 = 11
```

**Example 2:**
```
Input: root = [3,9,20,15,7]

Tree:
      3
     / \
    9  20
   / \
  15  7

Output: [3.00000, 14.50000, 11.00000]
```

#### Understanding the Problem

**What we need:**
- Process tree level by level
- Calculate average for each level
- Return list of averages

**Key observations:**
```
Level-by-level processing → BFS!

Why BFS?
- Naturally processes by levels
- Queue keeps track of level boundaries
- Can count nodes per level
```

#### Naive Approach (Works but Inefficient)

**Attempt 1: Multiple passes through tree**

```python
def averageOfLevels(root):
    # Find max depth
    def maxDepth(node):
        if not node:
            return 0
        return 1 + max(maxDepth(node.left), maxDepth(node.right))
    
    depth = maxDepth(root)
    result = []
    
    # For each level, sum values
    for level in range(depth):
        level_sum = 0
        level_count = 0
        
        def sumLevel(node, curr_level):
            nonlocal level_sum, level_count
            if not node:
                return
            
            if curr_level == level:
                level_sum += node.val
                level_count += 1
                return
            
            sumLevel(node.left, curr_level + 1)
            sumLevel(node.right, curr_level + 1)
        
        sumLevel(root, 0)
        result.append(level_sum / level_count)
    
    return result
```

**Problems:**
- Time: O(n × h) where h = height
- Multiple traversals of tree
- Inefficient!

#### Optimal Approach: BFS with Level Tracking

**Key insight:** Process one level at a time using queue!

**Algorithm:**
1. Use queue to track nodes at current level
2. For each level:
   - Count nodes in queue
   - Process exactly that many nodes
   - Calculate average
3. Add children for next level

**Why this works:**
```
Queue naturally separates levels!

Initial: Queue = [root]
         This is level 0

Process level 0:
- Get size = 1
- Process 1 node
- Add children → Next level!

Queue = [left, right]
This is level 1
```

#### Visual Step-by-Step

```
Tree:
      3
     / \
    9  20
      /  \
     15   7


INITIAL:
queue = [3]
result = []


LEVEL 0:
Level size = 1
Level sum = 0

Process node 3:
  sum = 0 + 3 = 3
  Add children: 9, 20
  queue = [9, 20]

Average = 3 / 1 = 3.0
result = [3.0]


LEVEL 1:
Level size = 2
Level sum = 0

Process node 9:
  sum = 0 + 9 = 9
  No children
  queue = [20]

Process node 20:
  sum = 9 + 20 = 29
  Add children: 15, 7
  queue = [15, 7]

Average = 29 / 2 = 14.5
result = [3.0, 14.5]


LEVEL 2:
Level size = 2
Level sum = 0

Process node 15:
  sum = 0 + 15 = 15
  No children
  queue = [7]

Process node 7:
  sum = 15 + 7 = 22
  No children
  queue = []

Average = 22 / 2 = 11.0
result = [3.0, 14.5, 11.0]


Queue empty → Done!
Return [3.0, 14.5, 11.0] ✓
```

#### Solution

```python
from collections import deque

def averageOfLevels(root: TreeNode) -> list[float]:
    """
    Calculate average value at each level using BFS
    
    Time: O(n) - visit each node once
    Space: O(w) - w is max width of tree
    """
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)  # Nodes at current level
        level_sum = 0
        
        # Process all nodes at current level
        for _ in range(level_size):
            node = queue.popleft()
            level_sum += node.val
            
            # Add children for next level
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        # Calculate and store average
        level_avg = level_sum / level_size
        result.append(level_avg)
    
    return result
```

#### Detailed Trace

```python
Tree:
      1
     / \
    2   3
   / \   \
  4   5   6

Step-by-step execution:

Initial:
queue = [1]
result = []

Level 0:
level_size = 1
level_sum = 0

  Iteration 1 (i=0):
  node = 1
  level_sum = 0 + 1 = 1
  Add 2, 3
  queue = [2, 3]

level_avg = 1 / 1 = 1.0
result = [1.0]

Level 1:
level_size = 2
level_sum = 0

  Iteration 1 (i=0):
  node = 2
  level_sum = 0 + 2 = 2
  Add 4, 5
  queue = [3, 4, 5]

  Iteration 2 (i=1):
  node = 3
  level_sum = 2 + 3 = 5
  Add 6
  queue = [4, 5, 6]

level_avg = 5 / 2 = 2.5
result = [1.0, 2.5]

Level 2:
level_size = 3
level_sum = 0

  Iteration 1 (i=0):
  node = 4
  level_sum = 0 + 4 = 4
  No children
  queue = [5, 6]

  Iteration 2 (i=1):
  node = 5
  level_sum = 4 + 5 = 9
  No children
  queue = [6]

  Iteration 3 (i=2):
  node = 6
  level_sum = 9 + 6 = 15
  No children
  queue = []

level_avg = 15 / 3 = 5.0
result = [1.0, 2.5, 5.0]

Queue empty
Return [1.0, 2.5, 5.0] ✓
```

#### Why Level Size Matters

**The trick:** `level_size = len(queue)` before processing!

```
Why this works:

Before processing level 1:
queue = [2, 3]
level_size = 2

Process node 2, add its children:
queue = [3, 4, 5]

But we only process 2 nodes!
- Node 3 (from level 1)
- NOT nodes 4, 5 (they're level 2)

Because we saved level_size = 2 before adding children!
```

**Common mistake without level_size:**
```python
# WRONG
while queue:
    node = queue.popleft()
    # This mixes levels!
    # Can't calculate per-level average!
```

#### Common Mistakes

**Mistake 1: Not tracking level size**
```python
# WRONG
while queue:
    sum = 0
    node = queue.popleft()
    sum += node.val
    # Can't know when level ends!

# RIGHT
while queue:
    level_size = len(queue)  # Know level boundary
    sum = 0
    for _ in range(level_size):
        node = queue.popleft()
        sum += node.val
```

**Mistake 2: Integer division**
```python
# WRONG (in some languages)
average = sum / count  # Might truncate in Java/C++

# RIGHT (Python 3 is fine, but be explicit)
average = float(sum) / count
```

**Mistake 3: Not handling empty tree**
```python
# WRONG
queue = deque([root])  # Errors if root is None!

# RIGHT
if not root:
    return []
queue = deque([root])
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Visit each node exactly once
- Each node: O(1) operations
- Total: O(n)

**Space Complexity: O(w)**
- w = maximum width of tree
- Queue holds at most one level
- Worst case: Complete tree, bottom level
  - Width = n/2 nodes
  - O(n/2) = O(n)

**Best case for space:**
```
Skewed tree:
    1
   /
  2
 /
3

Max width = 1
Space: O(1)
```

**Worst case for space:**
```
Complete tree:
       1
      / \
     2   3
    / \ / \
   4  5 6  7

Bottom level has n/2 nodes
Space: O(n/2) = O(n)
```

---

### Problem 2: Minimum Depth of Binary Tree

**[LeetCode 111 - Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)**

#### Problem Statement

Given a binary tree, find its minimum depth. The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example 1:**
```
Input: root = [3,9,20,null,null,15,7]

Tree:
      3
     / \
    9  20
      /  \
     15   7

Output: 2
Explanation: Shortest path is 3 → 9 (2 nodes)
```

**Example 2:**
```
Input: root = [2,null,3,null,4,null,5,null,6]

Tree:
    2
     \
      3
       \
        4
         \
          5
           \
            6

Output: 5
Explanation: Only one path, all 5 nodes
```

#### Understanding the Problem

**What is minimum depth?**
```
Minimum depth = shortest path from root to any leaf

Key: Must reach a LEAF (node with no children)
```

**Visual examples:**
```
Tree 1:
      1
     / \
    2   3
       / \
      4   5

Paths to leaves:
1 → 2 (depth 2) ← Shortest!
1 → 3 → 4 (depth 3)
1 → 3 → 5 (depth 3)

Minimum depth: 2


Tree 2:
      1
     /
    2
   /
  3

Paths to leaves:
1 → 2 → 3 (depth 3)

Minimum depth: 3
```

**Critical case to understand:**
```
Tree:
    1
   /
  2

Is minimum depth 1? NO!
Node 1 is NOT a leaf (has child 2)
Minimum depth: 2 (must reach leaf 2)
```

#### Approach 1: DFS (Not Optimal for Min Depth)

**Why DFS is suboptimal:**
```
Tree:
      1
     / \
    2   3
   /
  4

DFS explores entire tree:
Path 1: 1 → 2 → 4 (depth 3)
Path 2: 1 → 3 (depth 2) ← Found last!

Must explore all paths!
Time: O(n)
```

**DFS solution (for comparison):**
```python
def minDepth_DFS(root):
    if not root:
        return 0
    
    # If leaf node
    if not root.left and not root.right:
        return 1
    
    # If only one child exists
    if not root.left:
        return 1 + minDepth_DFS(root.right)
    if not root.right:
        return 1 + minDepth_DFS(root.left)
    
    # Both children exist
    return 1 + min(minDepth_DFS(root.left), 
                   minDepth_DFS(root.right))
```

**Why the special cases?**
```
Case 1: Only right child
    1
     \
      2

Can't just take min(left, right)!
min(0, 1) = 0 but answer is 2!

Must go down the existing child.


Case 2: Both children
      1
     / \
    2   3

Now we can take minimum of both paths.
```

#### Approach 2: BFS (Optimal!)

**Key insight:** BFS finds shortest path naturally!

**Why BFS is better:**
```
Tree:
      1
     / \
    2   3
   /
  4

BFS level by level:
Level 1: Check 1 (not leaf)
Level 2: Check 2, 3 → 3 is leaf! Stop!

Found minimum without exploring deeper!
Early termination possible!
```

**Algorithm:**
1. Use BFS to explore level by level
2. Track depth/level
3. Return depth when first leaf is found
4. First leaf = shortest path!

#### Visual Step-by-Step

```
Tree:
      1
     / \
    2   3
   / \
  4   5


LEVEL 1 (depth = 1):
queue = [(1, 1)]
       node depth

Process (1, 1):
  Is 1 a leaf? No (has children)
  Add children with depth + 1
  queue = [(2, 2), (3, 2)]


LEVEL 2 (depth = 2):
queue = [(2, 2), (3, 2)]

Process (2, 2):
  Is 2 a leaf? No (has children)
  Add children with depth + 1
  queue = [(3, 2), (4, 3), (5, 3)]

Process (3, 2):
  Is 3 a leaf? YES! ✓
  Return depth = 2

Answer: 2 (didn't need to check 4 or 5!)
```

#### Solution

```python
from collections import deque

def minDepth(root: TreeNode) -> int:
    """
    Find minimum depth using BFS
    Returns depth when first leaf is found
    
    Time: O(n) worst case, but can stop early
    Space: O(w) where w is max width
    """
    if not root:
        return 0
    
    queue = deque([(root, 1)])  # (node, depth)
    
    while queue:
        node, depth = queue.popleft()
        
        # Check if leaf node
        if not node.left and not node.right:
            return depth  # First leaf found!
        
        # Add children with incremented depth
        if node.left:
            queue.append((node.left, depth + 1))
        if node.right:
            queue.append((node.right, depth + 1))
    
    return 0  # Should never reach (empty tree handled)
```

#### Alternative: Level-by-Level BFS

```python
def minDepth_alternative(root: TreeNode) -> int:
    """
    Track depth explicitly, level by level
    """
    if not root:
        return 0
    
    queue = deque([root])
    depth = 1
    
    while queue:
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            
            # Check if leaf
            if not node.left and not node.right:
                return depth
            
            # Add children
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        depth += 1  # Move to next level
    
    return depth
```

#### Detailed Trace

```python
Tree:
      1
     /
    2
   / \
  3   4
     /
    5

Using first solution:

Initial:
queue = [(1, 1)]

Iteration 1:
(node, depth) = (1, 1)
Is 1 a leaf? No
Add (2, 2)
queue = [(2, 2)]

Iteration 2:
(node, depth) = (2, 2)
Is 2 a leaf? No
Add (3, 3), (4, 3)
queue = [(3, 3), (4, 3)]

Iteration 3:
(node, depth) = (3, 3)
Is 3 a leaf? YES! ✓
Return 3

(Never checked node 4 or 5!)
```

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None
Output: 0
```

**Edge Case 2: Single node**
```python
root = [1]
Tree:
  1

Is 1 a leaf? Yes
Output: 1
```

**Edge Case 3: Only left path**
```python
root = [1,2]
Tree:
    1
   /
  2

Node 1 is NOT a leaf
Must go to node 2
Output: 2
```

**Edge Case 4: Only right path**
```python
root = [1,null,2]
Tree:
    1
     \
      2

Node 1 is NOT a leaf
Must go to node 2
Output: 2
```

#### Common Mistakes

**Mistake 1: Treating node with one child as leaf**
```python
# WRONG
if not root.left or not root.right:
    return 1  # Wrong! Might have one child

# RIGHT
if not root.left and not root.right:
    return 1  # Both must be None
```

**Mistake 2: Using DFS min() without special cases**
```python
# WRONG
def minDepth(root):
    if not root:
        return 0
    return 1 + min(minDepth(root.left), minDepth(root.right))

# For tree:    1
#             /
#            2
# Returns: 1 + min(1, 0) = 1 ❌ (should be 2)

# RIGHT: Check if only one child exists
```

**Mistake 3: Not storing depth with node in BFS**
```python
# WRONG
queue = deque([root])
depth = 0
while queue:
    node = queue.popleft()
    depth += 1  # Depth gets messed up!

# RIGHT
queue = deque([(root, 1)])
# OR track level by level
```

#### BFS vs DFS for Min Depth

```
BFS Advantages:
✓ Can stop early when first leaf found
✓ Optimal for minimum depth
✓ Clear level tracking

DFS Advantages:
✗ Must explore all paths
✗ No early termination
✓ But simpler code (recursive)

For minimum depth: BFS is better!


Example where BFS wins:

Tree:
      1
     / \
    2   3
   /
  4

BFS: Checks levels 1, 2 → Finds leaf 3 at level 2, returns 2
DFS: Must check entire tree to compare all paths

BFS is more efficient!
```

#### Time & Space Complexity

**Time Complexity:**
- Best case: O(w) where w = width
  - Early leaf at shallow level
- Average case: O(n/2)
  - Leaf in middle level
- Worst case: O(n)
  - All nodes on one side (skewed tree)

**Space Complexity: O(w)**
- Queue holds at most one level
- w = maximum width

**Comparison:**
```
Approach    Time        Space       Early Stop?
BFS         O(n)        O(w)        Yes ✓
DFS         O(n)        O(h)        No ✗

For min depth: BFS preferred!
```

---

### Problem 3: Maximum Depth of Binary Tree

**[LeetCode 104 - Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)**

#### Problem Statement

Given the `root` of a binary tree, return its maximum depth. The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**
```
Input: root = [3,9,20,null,null,15,7]

Tree:
      3
     / \
    9  20
      /  \
     15   7

Output: 3
Explanation: Longest path 3 → 20 → 15 (or 7)
```

**Example 2:**
```
Input: root = [1,null,2]

Tree:
    1
     \
      2

Output: 2
```

#### Understanding the Problem

**Maximum depth = longest path to any leaf**

```
Tree:
      1
     / \
    2   3
   /
  4

Paths:
1 → 2 → 4 (depth 3) ← Longest!
1 → 3 (depth 2)

Maximum depth: 3
```

#### Approach 1: DFS Recursive (Optimal!)

**Key insight:** Maximum depth = 1 + max(left depth, right depth)

**Why DFS is perfect here:**
```
For maximum depth, need to explore ALL paths
DFS naturally explores all paths!

Unlike min depth, no early termination possible
Must check all leaves to find deepest
```

**Recursive formula:**
```
maxDepth(node) = 1 + max(maxDepth(left), maxDepth(right))

Base case: empty node has depth 0
```

**Visual:**
```
      1
     / \
    2   3
   /
  4

maxDepth(1) = 1 + max(maxDepth(2), maxDepth(3))
            = 1 + max(2, 1)
            = 3

maxDepth(2) = 1 + max(maxDepth(4), maxDepth(None))
            = 1 + max(1, 0)
            = 2

maxDepth(4) = 1 + max(0, 0) = 1
```

#### Solution - DFS Recursive

```python
def maxDepth(root: TreeNode) -> int:
    """
    Calculate maximum depth using DFS recursion
    
    Time: O(n) - visit all nodes
    Space: O(h) - recursion stack height
    """
    # Base case: empty tree
    if not root:
        return 0
    
    # Recursive case
    left_depth = maxDepth(root.left)
    right_depth = maxDepth(root.right)
    
    return 1 + max(left_depth, right_depth)
```

**Concise version:**
```python
def maxDepth(root: TreeNode) -> int:
    if not root:
        return 0
    return 1 + max(maxDepth(root.left), maxDepth(root.right))
```

#### Detailed Recursive Trace

```python
Tree:
      1
     / \
    2   3
   / \
  4   5

Call stack visualization:

maxDepth(1)
├─ maxDepth(2)
│  ├─ maxDepth(4)
│  │  ├─ maxDepth(None) → 0
│  │  └─ maxDepth(None) → 0
│  │  returns 1 + max(0,0) = 1
│  └─ maxDepth(5)
│     ├─ maxDepth(None) → 0
│     └─ maxDepth(None) → 0
│     returns 1 + max(0,0) = 1
│  returns 1 + max(1,1) = 2
└─ maxDepth(3)
   ├─ maxDepth(None) → 0
   └─ maxDepth(None) → 0
   returns 1 + max(0,0) = 1
returns 1 + max(2,1) = 3 ✓
```

**Step-by-step execution:**
```
Call: maxDepth(1)

  Call: maxDepth(2)
  
    Call: maxDepth(4)
    
      Call: maxDepth(None)
      Return: 0
      
      Call: maxDepth(None)
      Return: 0
      
    Return: 1 + max(0,0) = 1
    
    Call: maxDepth(5)
    
      Call: maxDepth(None)
      Return: 0
      
      Call: maxDepth(None)
      Return: 0
      
    Return: 1 + max(0,0) = 1
    
  Return: 1 + max(1,1) = 2
  
  Call: maxDepth(3)
  
    Call: maxDepth(None)
    Return: 0
    
    Call: maxDepth(None)
    Return: 0
    
  Return: 1 + max(0,0) = 1
  
Return: 1 + max(2,1) = 3
```

#### Approach 2: DFS Iterative

**Using explicit stack:**

```python
def maxDepth_iterative(root: TreeNode) -> int:
    """
    Iterative DFS using stack
    Track depth with each node
    """
    if not root:
        return 0
    
    stack = [(root, 1)]  # (node, depth)
    max_depth = 0
    
    while stack:
        node, depth = stack.pop()
        
        # Update max depth
        max_depth = max(max_depth, depth)
        
        # Add children with incremented depth
        if node.left:
            stack.append((node.left, depth + 1))
        if node.right:
            stack.append((node.right, depth + 1))
    
    return max_depth
```

**Trace:**
```python
Tree:
      1
     / \
    2   3

Initial:
stack = [(1, 1)]
max_depth = 0

Iteration 1:
Pop (1, 1)
max_depth = max(0, 1) = 1
Push (2, 2), (3, 2)
stack = [(2, 2), (3, 2)]

Iteration 2:
Pop (3, 2)
max_depth = max(1, 2) = 2
No children
stack = [(2, 2)]

Iteration 3:
Pop (2, 2)
max_depth = max(2, 2) = 2
No children
stack = []

Return 2 ✓
```

#### Approach 3: BFS

**Level order traversal, count levels:**

```python
from collections import deque

def maxDepth_BFS(root: TreeNode) -> int:
    """
    BFS to count levels
    """
    if not root:
        return 0
    
    queue = deque([root])
    depth = 0
    
    while queue:
        depth += 1
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return depth
```

**Trace:**
```python
Tree:
      1
     / \
    2   3
   /
  4

Initial:
queue = [1]
depth = 0

Level 1:
depth = 1
level_size = 1
Process 1, add 2, 3
queue = [2, 3]

Level 2:
depth = 2
level_size = 2
Process 2, add 4
Process 3
queue = [4]

Level 3:
depth = 3
level_size = 1
Process 4
queue = []

Return 3 ✓
```

#### Comparison of Approaches

```
For MAXIMUM depth:

DFS Recursive:
✓ Most elegant
✓ Easy to understand
✓ Least code
✓ Natural recursion
Space: O(h)

DFS Iterative:
✓ No recursion (stack overflow safe)
✓ Similar logic
Space: O(h)

BFS:
✓ Level-by-level clear
✗ More code
✗ No advantage over DFS here
Space: O(w)

Best choice: DFS Recursive!
```

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None
Output: 0
```

**Edge Case 2: Single node**
```python
root = [1]
Output: 1
```

**Edge Case 3: Skewed tree**
```python
root = [1,2,null,3,null,4]

Tree:
    1
   /
  2
 /
3
/
4

Output: 4
```

#### Common Mistakes

**Mistake 1: Off-by-one error**
```python
# WRONG
def maxDepth(root):
    if not root:
        return 0
    return max(maxDepth(root.left), maxDepth(root.right))
    # Forgot the +1!

# RIGHT
return 1 + max(maxDepth(root.left), maxDepth(root.right))
```

**Mistake 2: Counting edges instead of nodes**
```python
# Question asks for number of NODES
# Not number of EDGES

Tree:
  1
 /
2

Nodes: 2
Edges: 1

Answer: 2 (nodes), not 1 (edges)
```

**Mistake 3: Treating None as depth 1**
```python
# WRONG
if not root:
    return 1  # Should be 0!

# RIGHT
if not root:
    return 0
```

#### Time & Space Complexity

**DFS Recursive:**
- Time: O(n) - visit each node once
- Space: O(h) - recursion stack
  - Best: O(log n) for balanced tree
  - Worst: O(n) for skewed tree

**DFS Iterative:**
- Time: O(n)
- Space: O(h) - explicit stack

**BFS:**
- Time: O(n)
- Space: O(w) - queue width
  - Worst: O(n/2) = O(n) for complete tree

---

### Problem 4: Binary Tree Level Order Traversal

**[LeetCode 102 - Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)**

#### Problem Statement

Given the `root` of a binary tree, return the level order traversal of its nodes' values (i.e., from left to right, level by level).

**Example 1:**
```
Input: root = [3,9,20,null,null,15,7]

Tree:
      3
     / \
    9  20
      /  \
     15   7

Output: [[3], [9,20], [15,7]]
```

**Example 2:**
```
Input: root = [1]
Output: [[1]]
```

**Example 3:**
```
Input: root = []
Output: []
```

#### Understanding the Problem

**What's different from previous problems:**
- Not just one value per level
- Need to return **array of arrays**
- Each inner array = one level

```
Tree:
      1
     / \
    2   3
   / \   \
  4   5   6

Level 0: [1]
Level 1: [2, 3]
Level 2: [4, 5, 6]

Output: [[1], [2, 3], [4, 5, 6]]
```

#### Why This is a Medium Problem

**Added complexity:**
1. Need to group nodes by level
2. Need to track level boundaries
3. Return 2D array structure

**Key insight:** Use BFS with level tracking!

```
Previous problems:
- Average of levels: One number per level
- Min/Max depth: One number total

This problem:
- All values per level: Array per level
```

#### Approach: BFS with Level Processing

**Algorithm:**
1. Use queue for BFS
2. For each level:
   - Store level size
   - Process exactly that many nodes
   - Collect values in array
   - Add array to result
3. Add children for next level

**Why level size is crucial:**
```
Without level size:
queue = [2, 3, 4, 5, 6]
         ↑_↑  ↑_____↑
       Level 1  Level 2

Can't tell where level 1 ends!

With level size:
Before processing level 1:
level_size = 2
Process exactly 2 nodes
Now we know children are level 2!
```

#### Visual Step-by-Step

```
Tree:
      1
     / \
    2   3
   / \
  4   5


INITIAL:
queue = [1]
result = []


LEVEL 0:
level_size = 1
level_values = []

  Process node 1:
  level_values = [1]
  Add children: 2, 3
  queue = [2, 3]

result = [[1]]


LEVEL 1:
level_size = 2  ← Important! Captured before adding children
level_values = []

  Process node 2:
  level_values = [2]
  Add children: 4, 5
  queue = [3, 4, 5]
  
  Process node 3:
  level_values = [2, 3]
  No children
  queue = [4, 5]

result = [[1], [2, 3]]


LEVEL 2:
level_size = 2
level_values = []

  Process node 4:
  level_values = [4]
  No children
  queue = [5]
  
  Process node 5:
  level_values = [4, 5]
  No children
  queue = []

result = [[1], [2, 3], [4, 5]]


Queue empty → Done!
Return [[1], [2, 3], [4, 5]] ✓
```

#### Solution

```python
from collections import deque

def levelOrder(root: TreeNode) -> list[list[int]]:
    """
    Level order traversal using BFS
    Returns 2D array where each subarray is one level
    
    Time: O(n) - visit each node once
    Space: O(w) - w is max width
    """
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)  # Nodes at current level
        level_values = []         # Values for this level
        
        # Process all nodes at current level
        for _ in range(level_size):
            node = queue.popleft()
            level_values.append(node.val)
            
            # Add children for next level
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        # Add this level to result
        result.append(level_values)
    
    return result
```

#### Detailed Trace

```python
Tree:
      1
     / \
    2   3
   / \   \
  4   5   6

Execution:

Initial:
queue = [1]
result = []

While loop iteration 1:
-------------------
level_size = 1
level_values = []

  For loop iteration 1 (i=0):
  node = 1
  level_values = [1]
  Add 2, 3
  queue = [2, 3]

result = [[1]]

While loop iteration 2:
-------------------
level_size = 2  ← Queue has [2, 3]
level_values = []

  For loop iteration 1 (i=0):
  node = 2
  level_values = [2]
  Add 4, 5
  queue = [3, 4, 5]
  
  For loop iteration 2 (i=1):
  node = 3
  level_values = [2, 3]
  Add 6
  queue = [4, 5, 6]

result = [[1], [2, 3]]

While loop iteration 3:
-------------------
level_size = 3  ← Queue has [4, 5, 6]
level_values = []

  For loop iteration 1 (i=0):
  node = 4
  level_values = [4]
  No children
  queue = [5, 6]
  
  For loop iteration 2 (i=1):
  node = 5
  level_values = [4, 5]
  No children
  queue = [6]
  
  For loop iteration 3 (i=2):
  node = 6
  level_values = [4, 5, 6]
  No children
  queue = []

result = [[1], [2, 3], [4, 5, 6]]

Queue empty
Return [[1], [2, 3], [4, 5, 6]] ✓
```

#### Why Level Size Must Be Captured First

**Critical concept:**

```python
# Inside while loop:
level_size = len(queue)  # ← Must be BEFORE for loop

for _ in range(level_size):
    node = queue.popleft()
    # Add children here ← Queue size changes!
```

**What happens:**
```
Before for loop:
queue = [2, 3]
level_size = 2  ← Saved!

During for loop:
Process 2 → Add its children
queue = [3, 4, 5]  ← Size changed!

But we only process 2 nodes
Because level_size = 2 was saved!
```

**Without saving level_size:**
```python
# WRONG
while queue:
    for _ in range(len(queue)):  # ← len(queue) changes during loop!
        node = queue.popleft()
        # Add children...
        # Now len(queue) is different!
```

**Example of what goes wrong:**
```
Initial queue = [2, 3]

Iteration 1:
range(len(queue)) = range(2)
Pop 2, add children 4, 5
queue = [3, 4, 5]

len(queue) is NOW 3, but range is still 2
Iterations: 2

But if we keep checking len(queue):
- Would process 3, 4 in same level!
- Wrong grouping!
```

#### Alternative: Using Null Markers

**Different approach (less common):**

```python
def levelOrder_null_marker(root: TreeNode) -> list[list[int]]:
    """
    Use None as level separator
    """
    if not root:
        return []
    
    result = []
    queue = deque([root, None])  # None marks end of level
    level = []
    
    while queue:
        node = queue.popleft()
        
        if node is None:
            # End of level
            result.append(level)
            level = []
            
            # Add marker for next level
            if queue:  # If more nodes exist
                queue.append(None)
        else:
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return result
```

**How null markers work:**
```
Initial: [1, None]

Process 1:
Add children: [None, 2, 3]
Hit None: End level 0

Process 2, 3:
Add children: [None, 4, 5, 6]
Hit None: End level 1

Process 4, 5, 6:
Queue: [None]
Hit None: End level 2
queue is empty, stop
```

**Why level_size method is better:**
- Clearer logic
- No special markers
- More intuitive
- Less error-prone

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None
Output: []
```

**Edge Case 2: Single node**
```python
root = [1]

Tree:
  1

Output: [[1]]
```

**Edge Case 3: Complete tree**
```python
root = [1,2,3,4,5,6,7]

Tree:
       1
      / \
     2   3
    / \ / \
   4  5 6  7

Output: [[1], [2,3], [4,5,6,7]]
```

**Edge Case 4: Skewed tree**
```python
root = [1,2,null,3]

Tree:
    1
   /
  2
 /
3

Output: [[1], [2], [3]]
```

#### Common Mistakes

**Mistake 1: Not using level_size**
```python
# WRONG
while queue:
    level = []
    node = queue.popleft()
    level.append(node.val)
    # Can't group by levels!

# RIGHT
while queue:
    level_size = len(queue)
    level = []
    for _ in range(level_size):
        node = queue.popleft()
        level.append(node.val)
```

**Mistake 2: Reusing level array**
```python
# WRONG
level = []
while queue:
    level_size = len(queue)
    for _ in range(level_size):
        node = queue.popleft()
        level.append(node.val)
    result.append(level)  # All point to same list!
    # level.clear() or level = [] needed

# RIGHT
while queue:
    level_size = len(queue)
    level = []  # New list for each level
    for _ in range(level_size):
        ...
```

**Mistake 3: Appending node instead of value**
```python
# WRONG
level.append(node)  # Appends TreeNode object!

# RIGHT
level.append(node.val)  # Appends integer value
```

#### Variations of This Problem

**Variation 1: Right to left per level**
```python
# Same code, but reverse each level
result.append(level[::-1])
```

**Variation 2: Zigzag level order**
**[LeetCode 103 - Binary Tree Zigzag Level Order](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)**

```
Tree:
      1
     / \
    2   3
   / \ / \
  4  5 6  7

Zigzag: [[1], [3,2], [4,5,6,7]]
         L→R  R→L   L→R

Level 0: Left to right
Level 1: Right to left
Level 2: Left to right
...
```

```python
def zigzagLevelOrder(root: TreeNode) -> list[list[int]]:
    if not root:
        return []
    
    result = []
    queue = deque([root])
    left_to_right = True
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        # Reverse if right to left
        if not left_to_right:
            level.reverse()
        
        result.append(level)
        left_to_right = not left_to_right  # Toggle direction
    
    return result
```

**Variation 3: Level order bottom up**
**[LeetCode 107 - Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)**

```
Tree:
      1
     / \
    2   3
   / \
  4   5

Normal:    [[1], [2,3], [4,5]]
Bottom-up: [[4,5], [2,3], [1]]
```

```python
def levelOrderBottom(root: TreeNode) -> list[list[int]]:
    # Same code as level order
    # Just reverse result at end!
    result = levelOrder(root)
    return result[::-1]
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Visit each node exactly once
- Each node: O(1) operations (append to list)
- Total: O(n)

**Space Complexity: O(w)**
- w = maximum width of tree
- Queue holds at most one complete level

**Detailed space analysis:**
```
Input size: n nodes

Queue space:
- Best case (skewed): O(1)
- Average case: O(n/2) = O(n)
- Worst case (complete): O(n/2) = O(n)

Output space: O(n)
- Not counted in space complexity
- But total memory = O(n)

So: O(w) for algorithm, O(n) total
```

---

### Problem 5: Same Tree

**[LeetCode 100 - Same Tree](https://leetcode.com/problems/same-tree/)**

#### Problem Statement

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not. Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

**Example 1:**
```
Input: p = [1,2,3], q = [1,2,3]

Tree p:        Tree q:
    1              1
   / \            / \
  2   3          2   3

Output: true
```

**Example 2:**
```
Input: p = [1,2], q = [1,null,2]

Tree p:        Tree q:
    1              1
   /                \
  2                  2

Output: false
```

**Example 3:**
```
Input: p = [1,2,1], q = [1,1,2]

Tree p:        Tree q:
    1              1
   / \            / \
  2   1          1   2

Output: false
```

#### Understanding the Problem

**What does "same" mean?**

Two trees are same if:
1. **Structure is identical** - same shape
2. **Values are identical** - same values at each position

**Examples:**

```
Same ✓:
    1           1
   / \         / \
  2   3       2   3

Different structures ✗:
    1           1
   /             \
  2               2

Same structure, different values ✗:
    1           1
   / \         / \
  2   3       2   4
              ↑
         Different!
```

#### Naive Approach (Wrong)

**Attempt 1: Compare serializations**

```python
# WRONG - Too complicated
def isSameTree(p, q):
    def serialize(root):
        if not root:
            return "null,"
        return str(root.val) + "," + serialize(root.left) + serialize(root.right)
    
    return serialize(p) == serialize(q)
```

**Problems:**
- Unnecessary string operations
- Space: O(n) for strings
- More complex than needed

#### Optimal Approach: Simultaneous DFS

**Key insight:** Traverse both trees simultaneously!

**Algorithm:**
1. Compare current nodes
2. If different, return False
3. Recursively compare left subtrees
4. Recursively compare right subtrees
5. Both must match!

**Base cases:**
```
Case 1: Both None → Same ✓
Case 2: One None, one not → Different ✗
Case 3: Both exist but values differ → Different ✗
Case 4: Values same → Check children
```

#### Visual Step-by-Step

```
Tree p:        Tree q:
    1              1
   / \            / \
  2   3          2   3

Compare(1, 1):
  Values equal? Yes ✓
  Compare left: Compare(2, 2)
    Values equal? Yes ✓
    Compare left: Compare(None, None)
      Both None → True ✓
    Compare right: Compare(None, None)
      Both None → True ✓
    Return True
  Compare right: Compare(3, 3)
    Values equal? Yes ✓
    Compare left: Compare(None, None)
      Both None → True ✓
    Compare right: Compare(None, None)
      Both None → True ✓
    Return True
  Return True

Final: True ✓
```

#### Solution

```python
def isSameTree(p: TreeNode, q: TreeNode) -> bool:
    """
    Check if two trees are identical
    
    Time: O(min(n, m)) - stop at first difference
    Space: O(min(h1, h2)) - recursion depth
    """
    # Base case 1: Both None
    if not p and not q:
        return True
    
    # Base case 2: One None, one not
    if not p or not q:
        return False
    
    # Base case 3: Values differ
    if p.val != q.val:
        return False
    
    # Recursive case: Check both subtrees
    return (isSameTree(p.left, q.left) and 
            isSameTree(p.right, q.right))
```

**Concise version:**
```python
def isSameTree(p: TreeNode, q: TreeNode) -> bool:
    # All conditions in one
    if not p and not q:
        return True
    if not p or not q or p.val != q.val:
        return False
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

#### Detailed Trace

```python
Tree p:        Tree q:
    1              1
   / \            / \
  2   3          2   4
                    ↑
               Different!

Call tree:

isSameTree(1, 1)
│ p.val=1, q.val=1 → Continue
│
├─ isSameTree(2, 2)
│  │ p.val=2, q.val=2 → Continue
│  │
│  ├─ isSameTree(None, None)
│  │  │ Both None → True ✓
│  │
│  └─ isSameTree(None, None)
│     │ Both None → True ✓
│  │
│  Return True
│
└─ isSameTree(3, 4)
   │ p.val=3, q.val=4 → False! ✗
   │
   Return False

Return False (because right subtree returned False)
```

**Another example:**
```python
Tree p:        Tree q:
    1              1
   /                \
  2                  2

Call tree:

isSameTree(1, 1)
│ p.val=1, q.val=1 → Continue
│
├─ isSameTree(2, None)
│  │ p exists, q is None → False! ✗
│  │
│  Return False
│
(Never checks right because left returned False)

Return False
```

#### Understanding the Logic

**Why this order of checks?**

```python
# Check 1: Both None
if not p and not q:
    return True

# Check 2: One None
if not p or not q:
    return False  # We know ONE is None (not both)

# Check 3: Values
if p.val != q.val:
    return False  # We know both exist

# All checks passed, recurse
return isSameTree(left) and isSameTree(right)
```

**Why check both None first?**
```python
if not p or not q:  # This would catch both None too!
    return False     # Wrong for both None!

# So check both None first:
if not p and not q:
    return True

# Now we can check one None:
if not p or not q:
    return False
```

**Short-circuit evaluation:**
```python
return isSameTree(left) and isSameTree(right)

# If left returns False:
# - Right is NOT evaluated
# - Returns False immediately
# - Optimization!
```

#### Iterative Solution (Alternative)

**Using BFS with simultaneous queues:**

```python
from collections import deque

def isSameTree_iterative(p: TreeNode, q: TreeNode) -> bool:
    """
    Iterative BFS approach
    """
    queue_p = deque([p])
    queue_q = deque([q])
    
    while queue_p and queue_q:
        node_p = queue_p.popleft()
        node_q = queue_q.popleft()
        
        # Both None - continue
        if not node_p and not node_q:
            continue
        
        # One None or values differ
        if not node_p or not node_q or node_p.val != node_q.val:
            return False
        
        # Add children
        queue_p.append(node_p.left)
        queue_p.append(node_p.right)
        queue_q.append(node_q.left)
        queue_q.append(node_q.right)
    
    return True
```

**Trace:**
```python
Tree p:        Tree q:
    1              1
   / \            / \
  2   3          2   3

Initial:
queue_p = [1]
queue_q = [1]

Iteration 1:
node_p = 1, node_q = 1
Values equal ✓
Add children
queue_p = [2, 3]
queue_q = [2, 3]

Iteration 2:
node_p = 2, node_q = 2
Values equal ✓
Add children (all None)
queue_p = [3, None, None]
queue_q = [3, None, None]

Iteration 3:
node_p = 3, node_q = 3
Values equal ✓
Add children (all None)
queue_p = [None, None, None, None]
queue_q = [None, None, None, None]

Iterations 4-7:
All None nodes, continue

Both queues empty
Return True ✓
```

#### Edge Cases

**Edge Case 1: Both empty**
```python
p = None, q = None
Output: True
```

**Edge Case 2: One empty**
```python
p = [1], q = None
Output: False
```

**Edge Case 3: Single node same**
```python
p = [1], q = [1]
Output: True
```

**Edge Case 4: Single node different**
```python
p = [1], q = [2]
Output: False
```

**Edge Case 5: Same structure, one different value**
```python
p = [1,2,3], q = [1,2,4]
Output: False
```

#### Common Mistakes

**Mistake 1: Wrong order of checks**
```python
# WRONG
if not p or not q:
    return False  # Wrong for both None!

if not p and not q:
    return True  # Never reached!

# RIGHT
if not p and not q:
    return True  # Check both None first

if not p or not q:
    return False  # Then check one None
```

**Mistake 2: Forgetting to check values**
```python
# WRONG
def isSameTree(p, q):
    if not p and not q:
        return True
    if not p or not q:
        return False
    # Forgot to check p.val != q.val!
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)

# For trees:
#   1       2
# Would return True! Wrong!

# RIGHT
if p.val != q.val:
    return False
```

**Mistake 3: Using OR instead of AND**
```python
# WRONG
return isSameTree(p.left, q.left) or isSameTree(p.right, q.right)
# Only ONE subtree needs to match? Wrong!

# RIGHT
return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
# BOTH subtrees must match!
```

#### Time & Space Complexity

**Time Complexity: O(min(n, m))**
- n = nodes in tree p
- m = nodes in tree q
- Stop at first difference
- Best case: O(1) if roots differ
- Worst case: O(n) if trees identical

**Space Complexity: O(min(h1, h2))**
- h1 = height of tree p
- h2 = height of tree q
- Recursion stack depth
- Best: O(log n) for balanced
- Worst: O(n) for skewed

**Iterative space: O(w)**
- w = max width
- Queue size

---

### Problem 6: Path Sum

**[LeetCode 112 - Path Sum](https://leetcode.com/problems/path-sum/)**

#### Problem Statement

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

**Example 1:**
```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22

Tree:
         5
        / \
       4   8
      /   / \
     11  13  4
    / \       \
   7   2       1

Output: true

Explanation: Path 5→4→11→2 sums to 22
```

**Example 2:**
```
Input: root = [1,2,3], targetSum = 5

Tree:
    1
   / \
  2   3

Output: false

Explanation:
Path 1→2 = 3
Path 1→3 = 4
No path sums to 5
```

**Example 3:**
```
Input: root = [], targetSum = 0
Output: false
```

#### Understanding the Problem

**Key requirements:**
1. Must be **root-to-leaf** path (can't stop mid-tree)
2. Must reach a **leaf** (no children)
3. Sum must **exactly** equal targetSum

**Visual:**
```
Tree:
      5
     / \
    4   8
   /   / \
  11  13  4
 / \       \
7   2       1

Paths:
5→4→11→7 = 27
5→4→11→2 = 22 ✓ Found!
5→8→13 = 26
5→8→4→1 = 18

Target: 22
Answer: True (path 5→4→11→2)
```

**Critical: Must reach leaf!**
```
Tree:
    5
   /
  4
 /
11

Target: 9 (5 + 4)

Node path 5→4 sums to 9
But 4 is NOT a leaf!
Must continue to 11
Path 5→4→11 = 20
Answer: False
```

#### Naive Approach (Inefficient)

**Attempt 1: Find all paths, then check**

```python
# WRONG - Too much work
def hasPathSum(root, targetSum):
    def findAllPaths(node, path, all_paths):
        if not node:
            return
        
        path.append(node.val)
        
        if not node.left and not node.right:
            all_paths.append(path[:])
        
        findAllPaths(node.left, path, all_paths)
        findAllPaths(node.right, path, all_paths)
        path.pop()
    
    all_paths = []
    findAllPaths(root, [], all_paths)
    
    for path in all_paths:
        if sum(path) == targetSum:
            return True
    
    return False
```

**Problems:**
- Stores all paths: O(n²) space worst case
- Computes all sums even after finding answer
- Inefficient!

#### Optimal Approach: DFS with Running Sum

**Key insight:** Track remaining sum as we go down!

**Algorithm:**
1. Subtract current value from targetSum
2. Recurse with new targetSum
3. At leaf, check if targetSum == 0
4. Return True if any path works

**Why this works:**
```
Original: targetSum = 22

At root (5): remaining = 22 - 5 = 17
At node (4): remaining = 17 - 4 = 13
At node (11): remaining = 13 - 11 = 2
At leaf (2): remaining = 2 - 2 = 0 ✓

Found path!
```

#### Visual Step-by-Step

```
Tree:
      5
     / \
    4   8
   /
  11
 / \
7   2

Target: 22


DFS(5, 22):
  Not leaf, continue
  remaining = 22 - 5 = 17
  
  DFS(4, 17):
    Not leaf, continue
    remaining = 17 - 4 = 13
    
    DFS(11, 13):
      Not leaf, continue
      remaining = 13 - 11 = 2
      
      DFS(7, 2):
        Is leaf!
        remaining = 2 - 7 = -5
        -5 == 0? No
        Return False
      
      DFS(2, 2):
        Is leaf!
        remaining = 2 - 2 = 0
        0 == 0? Yes! ✓
        Return True
      
      Return True (found path)
    
    Return True
  
  DFS(8, 17):
    (Not explored - already found answer)
  
  Return True

Answer: True ✓
```

#### Solution

```python
def hasPathSum(root: TreeNode, targetSum: int) -> bool:
    """
    Check if root-to-leaf path sums to targetSum
    
    Time: O(n) - may visit all nodes
    Space: O(h) - recursion depth
    """
    # Base case: empty tree
    if not root:
        return False
    
    # Check if leaf node
    if not root.left and not root.right:
        return root.val == targetSum
    
    # Recurse with reduced targetSum
    remaining = targetSum - root.val
    
    return (hasPathSum(root.left, remaining) or 
            hasPathSum(root.right, remaining))
```

**Concise version:**
```python
def hasPathSum(root: TreeNode, targetSum: int) -> bool:
    if not root:
        return False
    
    if not root.left and not root.right:
        return root.val == targetSum
    
    return (hasPathSum(root.left, targetSum - root.val) or
            hasPathSum(root.right, targetSum - root.val))
```

#### Detailed Trace

```python
Tree:
      1
     / \
    2   3

Target: 3

Call tree:

hasPathSum(1, 3)
│ root=1, not leaf
│ remaining = 3 - 1 = 2
│
├─ hasPathSum(2, 2)
│  │ root=2, IS LEAF
│  │ 2 == 2? Yes! ✓
│  │
│  Return True
│
(Right subtree not evaluated - OR short-circuits)

Return True ✓


Another example:

Tree:
      1
     / \
    2   3

Target: 5

Call tree:

hasPathSum(1, 5)
│ root=1, not leaf
│ remaining = 5 - 1 = 4
│
├─ hasPathSum(2, 4)
│  │ root=2, IS LEAF
│  │ 2 == 4? No
│  │
│  Return False
│
└─ hasPathSum(3, 4)
   │ root=3, IS LEAF
   │ 3 == 4? No
   │
   Return False

Return False ✗
```

#### Why OR instead of AND?

**Critical understanding:**

```python
return hasPathSum(left, remaining) or hasPathSum(right, remaining)
```

**Why OR?**
- We need **at least ONE** path to work
- If left path works → return True
- If right path works → return True
- Both fail → return False

**Why not AND?**
```python
# WRONG
return hasPathSum(left, remaining) and hasPathSum(right, remaining)

# This requires BOTH paths to sum to target!
# But we only need ONE path!

Example:
      5
     / \
    4   8

Target: 9

Left path: 5→4 = 9 ✓
Right path: 5→8 = 13 ✗

OR: True (one path works) ✓
AND: False (both don't work) ✗
```

#### Alternative: Iterative Solution

**Using stack with (node, running_sum):**

```python
def hasPathSum_iterative(root: TreeNode, targetSum: int) -> bool:
    """
    Iterative DFS using stack
    """
    if not root:
        return False
    
    stack = [(root, root.val)]
    
    while stack:
        node, current_sum = stack.pop()
        
        # Check if leaf
        if not node.left and not node.right:
            if current_sum == targetSum:
                return True
        
        # Add children with updated sum
        if node.left:
            stack.append((node.left, current_sum + node.left.val))
        if node.right:
            stack.append((node.right, current_sum + node.right.val))
    
    return False
```

**Trace:**
```python
Tree:
      5
     / \
    4   8
   /
  11

Target: 20

Initial:
stack = [(5, 5)]

Iteration 1:
Pop (5, 5)
Not leaf
Add (4, 9), (8, 13)
stack = [(4, 9), (8, 13)]

Iteration 2:
Pop (8, 13)
Not leaf (actually is in this tree, let's adjust)

Let me fix:

Tree:
      5
     / \
    4   8

Target: 13

Initial:
stack = [(5, 5)]

Iteration 1:
Pop (5, 5)
Not leaf
Add (4, 9), (8, 13)
stack = [(4, 9), (8, 13)]

Iteration 2:
Pop (8, 13)
IS LEAF
13 == 13? Yes! ✓
Return True
```

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None, targetSum = 0
Output: False
(No path exists)
```

**Edge Case 2: Single node matching**
```python
root = [1], targetSum = 1
Output: True
(Node 1 is a leaf, 1 == 1)
```

**Edge Case 3: Single node not matching**
```python
root = [1], targetSum = 2
Output: False
```

**Edge Case 4: Path to non-leaf**
```python
root = [1,2], targetSum = 1

Tree:
    1
   /
  2

Path 1 alone sums to 1
But node 1 is NOT a leaf!
Output: False
```

**Edge Case 5: Negative values**
```python
root = [1,-2,3], targetSum = 2

Tree:
    1
   / \
 -2   3

Path 1→3 = 4
Path 1→-2 = -1
Output: False
```

#### Common Mistakes

**Mistake 1: Not checking if leaf**
```python
# WRONG
def hasPathSum(root, targetSum):
    if not root:
        return False
    
    if root.val == targetSum:
        return True  # Wrong! Might not be leaf!
    
    return hasPathSum(root.left, targetSum - root.val) or \
           hasPathSum(root.right, targetSum - root.val)

# RIGHT
if not root.left and not root.right:  # Check if leaf!
    return root.val == targetSum
```

**Mistake 2: Using AND instead of OR**
```python
# WRONG
return hasPathSum(left, remaining) and hasPathSum(right, remaining)
# Need BOTH paths? No!

# RIGHT
return hasPathSum(left, remaining) or hasPathSum(right, remaining)
# Need at least ONE path!
```

**Mistake 3: Not handling None children**
```python
# WRONG - if one child is None
return hasPathSum(root.left, remaining) or \
       hasPathSum(root.right, remaining)

# If root.left is None:
# hasPathSum(None, remaining) returns False ✓
# This is correct!

# If root.right is None:
# hasPathSum(None, remaining) returns False ✓
# This is correct!

# Actually not a mistake if base case handles None!
```

#### Variation: Path Sum II

**[LeetCode 113 - Path Sum II](https://leetcode.com/problems/path-sum-ii/)**

**Problem:** Return ALL paths that sum to targetSum

```python
def pathSum(root: TreeNode, targetSum: int) -> list[list[int]]:
    """
    Find all root-to-leaf paths that sum to targetSum
    """
    result = []
    
    def dfs(node, remaining, path):
        if not node:
            return
        
        # Add current node to path
        path.append(node.val)
        
        # Check if leaf and sum matches
        if not node.left and not node.right and remaining == node.val:
            result.append(path[:])  # Add copy!
        
        # Recurse
        dfs(node.left, remaining - node.val, path)
        dfs(node.right, remaining - node.val, path)
        
        # Backtrack
        path.pop()
    
    dfs(root, targetSum, [])
    return result
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Must check all nodes in worst case
- Early termination possible
- Best: O(log n) if path found early in balanced tree
- Worst: O(n) if no path exists

**Space Complexity: O(h)**
- h = height of tree
- Recursion stack depth
- Best: O(log n) for balanced tree
- Worst: O(n) for skewed tree

---

### Problem 7: Diameter of Binary Tree

**[LeetCode 543 - Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)**

#### Problem Statement

Given the `root` of a binary tree, return the length of the **diameter** of the tree.

The **diameter** of a binary tree is the **length of the longest path** between any two nodes in a tree. This path may or may not pass through the root.

The **length of a path** between two nodes is represented by the number of edges between them.

**Example 1:**
```
Input: root = [1,2,3,4,5]

Tree:
      1
     / \
    2   3
   / \
  4   5

Output: 3

Explanation: The longest path is [4,2,1,3] or [5,2,1,3]
Length = 3 edges
```

**Example 2:**
```
Input: root = [1,2]

Tree:
    1
   /
  2

Output: 1
```

#### Understanding the Problem

**What is diameter?**
- Longest path between ANY two nodes
- Path can be anywhere in tree
- Doesn't have to pass through root!

**Critical:** Count EDGES, not NODES

```
Path with 4 nodes:
[4] - [2] - [1] - [3]
    ↑     ↑     ↑
  3 edges total

Length = 3
```

**Visual examples:**

```
Example 1:
      1
     / \
    2   3
   / \
  4   5

Possible paths:
4→2→1→3 (3 edges) ← Longest!
5→2→1→3 (3 edges) ← Also longest!
4→2→5 (2 edges)
2→1→3 (2 edges)

Diameter = 3
```

```
Example 2:
      1
     / \
    2   3
   /     \
  4       5
 /         \
6           7

Longest path: 6→4→2→1→3→5→7 (6 edges)
Diameter = 6

Note: Path DOESN'T have to go through root!
Could be 6→4→2 (2 edges) in left subtree only
```

#### Key Insight

**Diameter at any node = left height + right height**

```
At node N:
Longest path through N = 
  longest path going left + longest path going right

      N
     / \
    L   R
   
Diameter through N = height(L) + height(R)
```

**Visual:**
```
      1
     / \
    2   3
   / \
  4   5

At node 1:
Left height = 2 (path: 1→2→4)
Right height = 1 (path: 1→3)
Diameter = 2 + 1 = 3 ✓

At node 2:
Left height = 1 (path: 2→4)
Right height = 1 (path: 2→5)
Diameter = 1 + 1 = 2
```

**Why check every node?**

```
Tree:
        1
       /
      2
     / \
    3   4
   /
  5

Diameter through root (1):
Left height = 3, Right height = 0
Diameter = 3 + 0 = 3

Diameter through node 2:
Left height = 2, Right height = 1
Diameter = 2 + 1 = 3

Same! But if we only checked root, we'd miss:

Tree:
        1
       /
      2
     / \
    3   4
   /     \
  5       6
 /
7

Diameter through node 2:
Left = 3, Right = 2
Diameter = 3 + 2 = 5

Diameter through root 1:
Left = 4, Right = 0
Diameter = 4

We'd miss the diameter of 5!
```

#### Naive Approach (Inefficient)

**Attempt 1: Calculate height repeatedly**

```python
def diameterOfBinaryTree(root):
    def height(node):
        if not node:
            return 0
        return 1 + max(height(node.left), height(node.right))
    
    def diameter(node):
        if not node:
            return 0
        
        # Diameter through this node
        left_height = height(node.left)
        right_height = height(node.right)
        through_node = left_height + right_height
        
        # Diameter in subtrees
        left_diameter = diameter(node.left)
        right_diameter = diameter(node.right)
        
        return max(through_node, left_diameter, right_diameter)
    
    return diameter(root)
```

**Problem:**
```
Time: O(n²)

For each node (n nodes):
- Calculate height: O(n)
- Total: O(n²)

Tree:
    1
   / \
  2   3
 / \
4   5

At node 1: Calculate height(2) - visits 4, 5
At node 2: Calculate height(4), height(5) again!

Recalculating heights!
```

#### Optimal Approach: Single DFS with Global Variable

**Key optimization:** Calculate height once, track diameter globally!

**Algorithm:**
1. Use DFS to calculate height
2. At each node, calculate diameter through it
3. Update global maximum
4. Return height for parent

**Why this works:**
- Single traversal: O(n)
- No recalculation
- Track max diameter as we go

#### Visual Step-by-Step

```
Tree:
      1
     / \
    2   3
   / \
  4   5

Global: diameter = 0


DFS(4):
  Leaf node
  diameter_here = 0 + 0 = 0
  diameter = max(0, 0) = 0
  Return height = 1

DFS(5):
  Leaf node
  diameter_here = 0 + 0 = 0
  diameter = max(0, 0) = 0
  Return height = 1

DFS(2):
  left_height = 1 (from node 4)
  right_height = 1 (from node 5)
  diameter_here = 1 + 1 = 2
  diameter = max(0, 2) = 2 ← Updated!
  Return height = 1 + max(1, 1) = 2

DFS(3):
  Leaf node
  diameter_here = 0 + 0 = 0
  diameter = max(2, 0) = 2
  Return height = 1

DFS(1):
  left_height = 2 (from node 2)
  right_height = 1 (from node 3)
  diameter_here = 2 + 1 = 3
  diameter = max(2, 3) = 3 ← Updated!
  Return height = 1 + max(2, 1) = 3

Return diameter = 3 ✓
```

#### Solution

```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        """
        Find diameter using single DFS
        
        Time: O(n) - visit each node once
        Space: O(h) - recursion depth
        """
        self.diameter = 0  # Global variable to track max
        
        def height(node):
            if not node:
                return 0
            
            # Get heights of subtrees
            left_height = height(node.left)
            right_height = height(node.right)
            
            # Update diameter if path through this node is longer
            self.diameter = max(self.diameter, left_height + right_height)
            
            # Return height for parent
            return 1 + max(left_height, right_height)
        
        height(root)
        return self.diameter
```

**Alternative without class variable:**

```python
def diameterOfBinaryTree(root: TreeNode) -> int:
    """
    Using list to store diameter (mutable in closure)
    """
    diameter = [0]  # Use list so it's mutable in nested function
    
    def height(node):
        if not node:
            return 0
        
        left = height(node.left)
        right = height(node.right)
        
        diameter[0] = max(diameter[0], left + right)
        
        return 1 + max(left, right)
    
    height(root)
    return diameter[0]
```

**Why use list or class variable?**

```python
# WRONG - doesn't work
def diameterOfBinaryTree(root):
    diameter = 0  # Local variable
    
    def height(node):
        diameter = max(diameter, left + right)  # Creates new local!
        # Doesn't update outer diameter!
    
    # diameter is still 0

# RIGHT - use nonlocal
def diameterOfBinaryTree(root):
    diameter = 0
    
    def height(node):
        nonlocal diameter  # Modify outer variable
        diameter = max(diameter, left + right)

# OR use mutable object
def diameterOfBinaryTree(root):
    diameter = [0]  # List is mutable
    
    def height(node):
        diameter[0] = max(diameter[0], left + right)  # Works!
```

#### Detailed Trace

```python
Tree:
        1
       / \
      2   3
     /
    4

diameter = 0

height(1):
│
├─ height(2):
│  │
│  ├─ height(4):
│  │  │
│  │  ├─ height(None) → 0
│  │  │
│  │  └─ height(None) → 0
│  │  │
│  │  diameter = max(0, 0+0) = 0
│  │  Return 1
│  │
│  ├─ height(None) → 0
│  │
│  diameter = max(0, 1+0) = 1
│  Return 1 + max(1, 0) = 2
│
└─ height(3):
   │
   ├─ height(None) → 0
   │
   └─ height(None) → 0
   │
   diameter = max(1, 0+0) = 1
   Return 1
│
diameter = max(1, 2+1) = 3
Return 1 + max(2, 1) = 3

Final diameter = 3
```

**Another example:**

```python
Tree:
      1
     /
    2
   / \
  3   4

diameter = 0

height(1):
│
├─ height(2):
│  │
│  ├─ height(3):
│  │  │ Both children None
│  │  │ diameter = max(0, 0+0) = 0
│  │  │ Return 1
│  │
│  └─ height(4):
│     │ Both children None
│     │ diameter = max(0, 0+0) = 0
│     │ Return 1
│  │
│  diameter = max(0, 1+1) = 2 ← Diameter found here!
│  Return 1 + max(1, 1) = 2
│
└─ height(None) → 0
│
diameter = max(2, 2+0) = 2
Return 1 + max(2, 0) = 3

Final diameter = 2 ✓
```

#### Why This Algorithm Works

**Key insight breakdown:**

```
1. Height calculation:
   height(node) = 1 + max(height(left), height(right))
   Standard recursive height

2. Diameter calculation:
   At each node, diameter = left_height + right_height
   This is the longest path THROUGH this node

3. Global maximum:
   Track max diameter seen across all nodes
   Answer is the maximum of all node diameters

4. Single traversal:
   Calculate height bottom-up
   Update diameter at each node
   O(n) time!
```

**Visual proof:**

```
      A
     / \
    B   C
   / \
  D   E

Diameter at A = height(B) + height(C)
              = 2 + 1 = 3

Path represented:
- height(B) = 2 means we can go 2 edges down left (A→B→D)
- height(C) = 1 means we can go 1 edge down right (A→C)
- Total path: D→B→A→C (3 edges)

This is the diameter through A!
```

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None
diameter = 0
Output: 0
```

**Edge Case 2: Single node**
```python
root = [1]

height(1):
  left = 0, right = 0
  diameter = max(0, 0+0) = 0
  Return 1

Output: 0 (no edges)
```

**Edge Case 3: Two nodes**
```python
root = [1,2]

Tree:
  1
 /
2

diameter through 1 = 1 + 0 = 1
Output: 1 ✓
```

**Edge Case 4: Skewed tree**
```python
root = [1,2,null,3,null,4]

Tree:
1
 \
  2
   \
    3
     \
      4

All diameters = 0 + height(right)
Max at root: 0 + 3 = 3
Output: 3
```

#### Common Mistakes

**Mistake 1: Counting nodes instead of edges**
```python
# WRONG
diameter = left_height + right_height + 1
# +1 counts current node
# But we count EDGES not NODES!

# RIGHT
diameter = left_height + right_height
```

**Mistake 2: Not checking all nodes**
```python
# WRONG
def diameterOfBinaryTree(root):
    if not root:
        return 0
    
    left = height(root.left)
    right = height(root.right)
    return left + right  # Only checks root!

# RIGHT
# Must check diameter at EVERY node
def height(node):
    ...
    diameter = max(diameter, left + right)  # Check at each node
```

**Mistake 3: Forgetting nonlocal**
```python
# WRONG
def diameterOfBinaryTree(root):
    diameter = 0
    
    def height(node):
        diameter = max(diameter, left + right)
        # Creates new local variable!
    
    return diameter  # Still 0!

# RIGHT
def diameterOfBinaryTree(root):
    diameter = 0
    
    def height(node):
        nonlocal diameter  # Modify outer variable
        diameter = max(diameter, left + right)
```

**Mistake 4: Returning diameter instead of height**
```python
# WRONG
def height(node):
    ...
    diameter = max(diameter, left + right)
    return diameter  # Wrong! Should return height!

# RIGHT
def height(node):
    ...
    diameter = max(diameter, left + right)
    return 1 + max(left, right)  # Return height for parent
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Visit each node exactly once
- At each node: O(1) operations
- Total: O(n)

**Space Complexity: O(h)**
- h = height of tree
- Recursion stack depth
- Best: O(log n) for balanced tree
- Worst: O(n) for skewed tree

**Comparison with naive approach:**

```
Naive:  Time O(n²), Space O(h)
Optimal: Time O(n), Space O(h)

Huge improvement for large trees!
```

---

### Problem 8: Invert Binary Tree

**[LeetCode 226 - Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)**

#### Problem Statement

Given the `root` of a binary tree, invert the tree, and return its root.

**Example 1:**
```
Input: root = [4,2,7,1,3,6,9]

Before:
      4
     / \
    2   7
   / \ / \
  1  3 6  9

After:
      4
     / \
    7   2
   / \ / \
  9  6 3  1

Output: [4,7,2,9,6,3,1]
```

**Example 2:**
```
Input: root = [2,1,3]

Before:
    2
   / \
  1   3

After:
    2
   / \
  3   1

Output: [2,3,1]
```

**Example 3:**
```
Input: root = []
Output: []
```

#### Understanding the Problem

**What is "invert"?**
- Swap left and right children at EVERY node
- Mirror image of the tree
- Also called "mirror tree"

**Visual:**

```
Original:
      1
     / \
    2   3
   / \
  4   5

Inverted:
      1
     / \
    3   2
       / \
      5   4

Every node's children are swapped!
```

**Step-by-step swap:**

```
Start:
      1
     / \
    2   3
   / \
  4   5

Swap at node 1:
      1
     / \
    3   2    ← Swapped!
   / \
  4   5

Swap at node 2:
      1
     / \
    3   2
       / \
      5   4  ← Swapped!

Done!
```

#### Naive Approach (Actually Works!)

**Sometimes the obvious solution is the best!**

**Algorithm:**
1. Swap left and right children
2. Recursively invert left subtree
3. Recursively invert right subtree

```python
def invertTree(root: TreeNode) -> TreeNode:
    if not root:
        return None
    
    # Swap children
    root.left, root.right = root.right, root.left
    
    # Recursively invert subtrees
    invertTree(root.left)
    invertTree(root.right)
    
    return root
```

**This is actually optimal! No need for "better" approach.**

#### Why This Works

**Key insight:** Swap, then recurse

```
      1
     / \
    2   3
   /
  4

Step 1: Swap at root
      1
     / \
    3   2
       /
      4

Step 2: Recurse on left (3)
No children, return

Step 3: Recurse on right (2)
      1
     / \
    3   2
         \
          4  ← Swapped!

Done!
```

**Why swap first?**

```
Option 1: Swap first, then recurse
root.left, root.right = root.right, root.left
invertTree(root.left)
invertTree(root.right)

Option 2: Recurse first, then swap
invertTree(root.left)
invertTree(root.right)
root.left, root.right = root.right, root.left

Both work! Order doesn't matter.
But swap-first is clearer.
```

#### Visual Step-by-Step

```
Tree:
      1
     / \
    2   3
   / \
  4   5

Call: invertTree(1)
│
├─ Swap: 1's children
│  Tree now:
│       1
│      / \
│     3   2
│        / \
│       4   5
│
├─ invertTree(3)
│  │ 3 is leaf
│  │ Return 3
│
└─ invertTree(2)
   │
   ├─ Swap: 2's children
   │  Tree now:
   │       1
   │      / \
   │     3   2
   │        / \
   │       5   4
   │
   ├─ invertTree(5)
   │  │ 5 is leaf
   │  │ Return 5
   │
   └─ invertTree(4)
      │ 4 is leaf
      │ Return 4

Final tree:
      1
     / \
    3   2
       / \
      5   4
```

#### Solution - Recursive

```python
def invertTree(root: TreeNode) -> TreeNode:
    """
    Invert binary tree recursively
    
    Time: O(n) - visit each node once
    Space: O(h) - recursion depth
    """
    # Base case: empty tree
    if not root:
        return None
    
    # Swap children
    root.left, root.right = root.right, root.left
    
    # Recursively invert subtrees
    invertTree(root.left)
    invertTree(root.right)
    
    return root
```

**Alternative: Swap after recursion**

```python
def invertTree(root: TreeNode) -> TreeNode:
    if not root:
        return None
    
    # Invert subtrees first
    left = invertTree(root.left)
    right = invertTree(root.right)
    
    # Then swap
    root.left = right
    root.right = left
    
    return root
```

**Both work equally well!**

#### Solution - Iterative (BFS)

```python
from collections import deque

def invertTree_BFS(root: TreeNode) -> TreeNode:
    """
    Invert using BFS (level order)
    """
    if not root:
        return None
    
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        
        # Swap children
        node.left, node.right = node.right, node.left
        
        # Add children to queue
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    
    return root
```

**Trace:**

```python
Tree:
      1
     / \
    2   3
   /
  4

Initial:
queue = [1]

Iteration 1:
node = 1
Swap children: [3, 2]
Add 3, 2 to queue
queue = [3, 2]

Tree now:
      1
     / \
    3   2
       /
      4

Iteration 2:
node = 3
Swap children: None
queue = [2]

Iteration 3:
node = 2
Swap children: [None, 4] → [4, None]
Add 4
queue = [4]

Tree now:
      1
     / \
    3   2
         \
          4

Iteration 4:
node = 4
Swap children: None
queue = []

Done!
```

#### Solution - Iterative (DFS)

```python
def invertTree_DFS(root: TreeNode) -> TreeNode:
    """
    Invert using DFS with stack
    """
    if not root:
        return None
    
    stack = [root]
    
    while stack:
        node = stack.pop()
        
        # Swap children
        node.left, node.right = node.right, node.left
        
        # Add children to stack
        if node.left:
            stack.append(node.left)
        if node.right:
            stack.append(node.right)
    
    return root
```

#### Detailed Trace

```python
Tree:
      1
     / \
    2   3
   / \   \
  4   5   6

invertTree(1):
│ Swap: left=2, right=3 → left=3, right=2
│
├─ invertTree(3):
│  │ Swap: left=None, right=6 → left=6, right=None
│  │
│  ├─ invertTree(6):
│  │  │ 6 is leaf
│  │  │ Return 6
│  │
│  └─ invertTree(None):
│     │ Return None
│  │
│  Return 3
│
└─ invertTree(2):
   │ Swap: left=4, right=5 → left=5, right=4
   │
   ├─ invertTree(5):
   │  │ 5 is leaf
   │  │ Return 5
   │
   └─ invertTree(4):
      │ 4 is leaf
      │ Return 4
   │
   Return 2

Return 1

Final tree:
      1
     / \
    3   2
   /   / \
  6   5   4
```

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None
Output: None
```

**Edge Case 2: Single node**
```python
root = [1]

No children to swap
Output: [1]
```

**Edge Case 3: Only left child**
```python
root = [1,2]

Before:
  1
 /
2

After:
  1
   \
    2

Swapped!
```

**Edge Case 4: Only right child**
```python
root = [1,null,2]

Before:
  1
   \
    2

After:
  1
 /
2

Swapped!
```

**Edge Case 5: Complete tree**
```python
root = [1,2,3,4,5,6,7]

Before:
       1
      / \
     2   3
    / \ / \
   4  5 6  7

After:
       1
      / \
     3   2
    / \ / \
   7  6 5  4
```

#### Common Mistakes

**Mistake 1: Only swapping once**
```python
# WRONG
def invertTree(root):
    if not root:
        return None
    
    root.left, root.right = root.right, root.left
    # Forgot to recurse!
    return root

# Only swaps root's children
# Doesn't invert subtrees!

# RIGHT
root.left, root.right = root.right, root.left
invertTree(root.left)
invertTree(root.right)
```

**Mistake 2: Incorrect swap**
```python
# WRONG
temp = root.left
root.left = root.right
root.right = root.left  # This is root.right, not temp!

# RIGHT
temp = root.left
root.left = root.right
root.right = temp

# OR use Python's tuple swap
root.left, root.right = root.right, root.left
```

**Mistake 3: Modifying during iteration (iterative)**
```python
# CAREFUL
if node.left:
    stack.append(node.left)
if node.right:
    stack.append(node.right)

node.left, node.right = node.right, node.left

# After swap, node.left and node.right are different!
# But we already added them to stack
# This is actually OK! We added the correct nodes.

# Just make sure you swap AFTER or BEFORE adding
# Not in between!
```

#### Comparison of Approaches

```
Recursive DFS:
✓ Cleanest code
✓ Easy to understand
✓ Natural recursion
Space: O(h)

Iterative BFS:
✓ No recursion (stack overflow safe)
✓ Level by level
Space: O(w)

Iterative DFS:
✓ No recursion
✓ Similar to recursive
Space: O(h)

All have same time: O(n)
Choose based on preference!
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Visit each node exactly once
- O(1) swap at each node
- Total: O(n)

**Space Complexity:**
- Recursive: O(h) for call stack
  - Best: O(log n) balanced
  - Worst: O(n) skewed
- Iterative: O(w) for queue/stack
  - Best: O(1) skewed
  - Worst: O(n/2) = O(n) complete tree

---

### Problem 9: Lowest Common Ancestor of a Binary Tree

**[LeetCode 236 - Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)**

#### Problem Statement

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA: "The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in the tree that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**)."

**Example 1:**
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1

Tree:
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4

Output: 3

Explanation: LCA of nodes 5 and 1 is 3
```

**Example 2:**
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4

Tree:
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4

Output: 5

Explanation: LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself
```

**Example 3:**
```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

#### Understanding the Problem

**What is Lowest Common Ancestor (LCA)?**

The LCA is the deepest node that is an ancestor of both nodes.

**Visual:**
```
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4

LCA(6, 4) = ?

Ancestors of 6: 6, 5, 3
Ancestors of 4: 4, 2, 5, 3

Common ancestors: 5, 3
Lowest (deepest) common: 5 ✓
```

**Important:** Node can be ancestor of itself!

```
         3
       /   \
      5     1
     / \
    6   2

LCA(5, 6) = ?

Ancestors of 5: 5, 3
Ancestors of 6: 6, 5, 3

Common: 5, 3
Lowest: 5 ✓

5 is ancestor of itself!
```

#### Key Observations

**Observation 1:** LCA is where paths to p and q split

```
         3
       /   \
      5     1
     / \
    6   2

Path to 6: 3 → 5 → 6
Path to 2: 3 → 5 → 2

Paths split at 5!
LCA(6, 2) = 5
```

**Observation 2:** If one node is ancestor of other, it's the LCA

```
         3
       /   \
      5     1
     / \
    6   2

5 is ancestor of 6
LCA(5, 6) = 5
```

**Observation 3:** Three cases at any node

```
At node N:

Case 1: p in left, q in right
        → N is LCA!

Case 2: Both p and q in left
        → LCA is in left subtree

Case 3: Both p and q in right
        → LCA is in right subtree
```

#### Naive Approach (Inefficient)

**Attempt 1: Find paths to both nodes, then compare**

```python
def lowestCommonAncestor(root, p, q):
    # Find path from root to p
    def findPath(node, target, path):
        if not node:
            return False
        
        path.append(node)
        
        if node == target:
            return True
        
        if findPath(node.left, target, path) or \
           findPath(node.right, target, path):
            return True
        
        path.pop()
        return False
    
    path_p = []
    path_q = []
    findPath(root, p, path_p)
    findPath(root, q, path_q)
    
    # Find last common node
    lca = None
    for i in range(min(len(path_p), len(path_q))):
        if path_p[i] == path_q[i]:
            lca = path_p[i]
        else:
            break
    
    return lca
```

**Problems:**
- Two separate traversals
- Store entire paths: O(n) space
- More complex than needed

**Example:**
```
         3
       /   \
      5     1
     / \
    6   2

path_p (to 6): [3, 5, 6]
path_q (to 2): [3, 5, 2]

Compare:
i=0: 3 == 3 ✓ lca = 3
i=1: 5 == 5 ✓ lca = 5
i=2: 6 != 2 ✗ stop

Return 5 ✓
```

#### Optimal Approach: Recursive DFS

**Key insight:** Recursively search in both subtrees!

**Algorithm:**
1. If current node is p or q, return it
2. Recursively search in left and right subtrees
3. If p and q found in different subtrees → current node is LCA
4. If both in same subtree → LCA is in that subtree

**Return values:**
- `None`: Neither p nor q found
- `p or q`: Found one of them (or their LCA)
- When both subtrees return non-None → current is LCA!

#### Visual Step-by-Step

```
Tree:
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4

Find LCA(6, 4)


LCA(3):
│
├─ LCA(5):
│  │
│  ├─ LCA(6):
│  │  │ node == p, return 6
│  │
│  └─ LCA(2):
│     │
│     ├─ LCA(7):
│     │  │ Not p or q, check children
│     │  │ Both None
│     │  │ Return None
│     │
│     └─ LCA(4):
│        │ node == q, return 4
│     │
│     left = None, right = 4
│     Return 4
│  │
│  left = 6, right = 4
│  Both non-None! → 5 is LCA ✓
│  Return 5
│
└─ LCA(1):
   │ (not explored, already found LCA)
│
Return 5 ✓
```

**Another example:**

```
Tree:
         3
       /   \
      5     1
     / \
    6   2

Find LCA(5, 6)


LCA(3):
│
├─ LCA(5):
│  │ node == p, return 5 immediately!
│
└─ (right not needed)
│
left = 5, right = None
Return 5 ✓

(5 is ancestor of itself and 6)
```

#### Solution

```python
def lowestCommonAncestor(root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
    """
    Find LCA using recursive DFS
    
    Time: O(n) - visit all nodes in worst case
    Space: O(h) - recursion depth
    """
    # Base case: reached null or found p or q
    if not root or root == p or root == q:
        return root
    
    # Search in left and right subtrees
    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)
    
    # If both sides found something, current node is LCA
    if left and right:
        return root
    
    # Otherwise, return whichever side found something
    return left if left else right
```

**Understanding the return logic:**

```python
# Case 1: Both left and right are non-None
if left and right:
    return root
# This means p in one subtree, q in other
# Current node is LCA!

# Case 2: Only left is non-None
return left
# Either:
# - Only p or q is in left subtree
# - Both p and q are in left subtree (LCA already found)

# Case 3: Only right is non-None
return right
# Similar to case 2

# Case 4: Both None
return None
# Neither p nor q found here
```

#### Detailed Trace

```python
Tree:
         3
       /   \
      5     1
     / \   / \
    6   2 0   8

Find LCA(6, 8)

Call tree with return values:

LCA(3, 6, 8):
│
├─ LCA(5, 6, 8):
│  │
│  ├─ LCA(6, 6, 8):
│  │  │ root == p
│  │  │ Return 6 ✓
│  │
│  └─ LCA(2, 6, 8):
│     │
│     ├─ LCA(7, 6, 8):
│     │  │ Neither p nor q
│     │  │ left = None, right = None
│     │  │ Return None
│     │
│     └─ LCA(4, 6, 8):
│        │ Neither p nor q
│        │ left = None, right = None
│        │ Return None
│     │
│     left = None, right = None
│     Return None
│  │
│  left = 6, right = None
│  Return 6
│
└─ LCA(1, 6, 8):
   │
   ├─ LCA(0, 6, 8):
   │  │ Neither p nor q
   │  │ Return None
   │
   └─ LCA(8, 6, 8):
      │ root == q
      │ Return 8 ✓
   │
   left = None, right = 8
   Return 8
│
left = 6, right = 8
Both non-None! ← p and q in different subtrees
Return 3 ✓

Answer: 3
```

**Example where node is ancestor of itself:**

```python
Tree:
         3
       /   \
      5     1
     / \
    6   2
       / \
      7   4

Find LCA(5, 4)

LCA(3, 5, 4):
│
├─ LCA(5, 5, 4):
│  │ root == p
│  │ Return 5 immediately! ✓
│  │ (Don't even check children)
│
└─ (right not evaluated - left already returned)
│
left = 5, right = None
Return 5 ✓

Answer: 5 (5 is ancestor of itself and 4)
```

#### Why This Algorithm Works

**Key insight: Early return when finding p or q**

```python
if not root or root == p or root == q:
    return root
```

**Why return immediately when we find p or q?**

```
Case 1: p is ancestor of q
         p
        /
       ...
        \
         q

When we reach p, we return immediately
We don't go down to find q
But that's OK! Because:
- If q exists, it MUST be in p's subtree
- p is the LCA!

Case 2: p and q in different subtrees
         LCA
        /   \
       p     q

We return p from left subtree
We return q from right subtree
LCA sees both → returns itself
```

**Why "left if left else right" works:**

```python
return left if left else right

This handles:

1. left = p, right = None
   → Return p (might be LCA, or p is deeper)

2. left = None, right = q
   → Return q (similar to above)

3. left = None, right = None
   → Return None (neither found)

4. left = something, right = something
   → Already handled by "if left and right"
```

#### Iterative Solution (More Complex)

**Using parent pointers:**

```python
def lowestCommonAncestor_iterative(root, p, q):
    """
    Store parent pointers, then find common ancestor
    """
    # Store parent of each node
    parent = {root: None}
    stack = [root]
    
    # Build parent map until we find both p and q
    while p not in parent or q not in parent:
        node = stack.pop()
        
        if node.left:
            parent[node.left] = node
            stack.append(node.left)
        if node.right:
            parent[node.right] = node
            stack.append(node.right)
    
    # Get ancestors of p
    ancestors = set()
    while p:
        ancestors.add(p)
        p = parent[p]
    
    # Find first common ancestor
    while q not in ancestors:
        q = parent[q]
    
    return q
```

**Trace:**
```python
Tree:
         3
       /   \
      5     1
     / \
    6   2

Find LCA(6, 2)

Build parent map:
parent = {
    3: None,
    5: 3,
    1: 3,
    6: 5,
    2: 5
}

Ancestors of 6:
Start at 6
ancestors = {6}
Go to parent: 5
ancestors = {6, 5}
Go to parent: 3
ancestors = {6, 5, 3}
Go to parent: None, stop

Find first common ancestor of 2:
Start at 2
2 in ancestors? No
Go to parent: 5
5 in ancestors? Yes! ✓

Return 5
```

#### Edge Cases

**Edge Case 1: One node is root**
```python
root = [1,2], p = 1, q = 2

Tree:
  1
 /
2

LCA(1, 2) = 1 (root is ancestor of 2)
```

**Edge Case 2: Both nodes are children of root**
```python
root = [1,2,3], p = 2, q = 3

Tree:
    1
   / \
  2   3

LCA(2, 3) = 1
```

**Edge Case 3: Nodes at different levels**
```python
Tree:
      1
     /
    2
   /
  3

LCA(2, 3) = 2
```

**Edge Case 4: Node is LCA of itself**
```python
Tree:
      1
     /
    2
   /
  3

LCA(2, 2) = 2
```

#### Common Mistakes

**Mistake 1: Not returning when finding p or q**
```python
# WRONG
def lowestCommonAncestor(root, p, q):
    if not root:
        return None
    
    # Forgot to check if root == p or root == q!
    
    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)
    ...

# RIGHT
if not root or root == p or root == q:
    return root
```

**Mistake 2: Wrong return logic**
```python
# WRONG
if left and right:
    return root
else:
    return None  # Lost the found node!

# RIGHT
if left and right:
    return root
return left if left else right  # Propagate found node up
```

**Mistake 3: Checking values instead of nodes**
```python
# WRONG
if root.val == p or root.val == q:
    return root

# RIGHT
if root == p or root == q:
    return root

# p and q are TreeNode objects, not values!
```

**Mistake 4: Assuming p and q always exist**
```python
# The problem guarantees p and q exist in tree
# But good practice to handle:

if not root:
    return None

# Already handles non-existent nodes
```

#### Variation: Binary Search Tree (BST)

**[LeetCode 235 - Lowest Common Ancestor of BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)**

For BST, we can use the ordering property!

```python
def lowestCommonAncestor_BST(root, p, q):
    """
    Use BST property: left < root < right
    """
    while root:
        # Both in left subtree
        if p.val < root.val and q.val < root.val:
            root = root.left
        # Both in right subtree
        elif p.val > root.val and q.val > root.val:
            root = root.right
        # Split point found!
        else:
            return root
```

**Why this works:**

```
BST:
      6
     / \
    2   8
   / \ / \
  0  4 7  9

LCA(2, 8):

At 6: 2 < 6 < 8
      Split point! Return 6 ✓

LCA(2, 4):

At 6: both 2 and 4 < 6
      Go left
At 2: 2 ≤ 2 ≤ 4
      Split point! Return 2 ✓
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Worst case: Visit all nodes
- Example: Skewed tree, nodes at opposite ends
- Best case: O(h) if nodes close to root

**Space Complexity: O(h)**
- Recursion stack depth
- Best: O(log n) for balanced tree
- Worst: O(n) for skewed tree

**Comparison:**

```
Naive (path storage):
Time: O(n) - two traversals
Space: O(n) - store paths

Optimal (recursive):
Time: O(n) - single traversal
Space: O(h) - recursion only

Iterative (parent pointers):
Time: O(n)
Space: O(n) - parent map

Best: Recursive approach!
```

---

## Binary Trees - Complete Summary

### Key Concepts Covered

**1. Tree Types**
- Full Binary Tree
- Complete Binary Tree
- Perfect Binary Tree
- Balanced Binary Tree
- Degenerate Tree

**2. Traversal Methods**
- BFS (Level Order)
- DFS Preorder
- DFS Inorder
- DFS Postorder

**3. Common Patterns**

```python
# Pattern 1: Level-by-level processing
while queue:
    level_size = len(queue)
    for _ in range(level_size):
        node = queue.popleft()
        # Process node

# Pattern 2: Height calculation
def height(node):
    if not node:
        return 0
    return 1 + max(height(node.left), height(node.right))

# Pattern 3: Path problems
def hasPath(node, target):
    if not node:
        return False
    if is_leaf(node):
        return check_condition
    return hasPath(node.left, ...) or hasPath(node.right, ...)

# Pattern 4: Global variable tracking
self.result = initial
def helper(node):
    # Update self.result
    helper(node.left)
    helper(node.right)
```

### Problem Categories

**Level Order Problems:**
- Average of Levels
- Level Order Traversal
- Zigzag Level Order
- Right Side View

**Height/Depth Problems:**
- Min Depth
- Max Depth
- Balanced Tree
- Diameter

**Path Problems:**
- Path Sum
- Binary Tree Paths
- Sum Root to Leaf

**Tree Modification:**
- Invert Tree
- Flatten Tree
- Merge Trees

**Comparison/Search:**
- Same Tree
- Symmetric Tree
- Lowest Common Ancestor

### When to Use What

```
Need level-by-level? → BFS
Need all paths? → DFS
BST in sorted order? → Inorder DFS
Copy/serialize tree? → Preorder DFS
Delete tree? → Postorder DFS

Need shortest path? → BFS
Need to explore all? → DFS

Tracking max/min across tree? → Global variable
Need path information? → Pass down parameters
```

### Time/Space Complexity Patterns

```
Single traversal: O(n) time
Height calculation: O(n) time, O(h) space

BFS space: O(w) where w = max width
DFS space: O(h) where h = height

Balanced tree: h = O(log n)
Skewed tree: h = O(n)

Complete tree: w = O(n/2) = O(n)
```
