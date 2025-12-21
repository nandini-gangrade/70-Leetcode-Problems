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
def hasPathSum(root: TreeNode, targetSum: int) -> bool
