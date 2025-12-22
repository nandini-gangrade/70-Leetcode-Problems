# Complete Binary Search Trees (BST) Guide for Interview Preparation

## Table of Contents
1. [What is a Binary Search Tree?](#what-is-a-binary-search-tree)
2. [BST Properties & Operations](#bst-properties--operations)
3. [BST vs Binary Tree](#bst-vs-binary-tree)
4. [Problems](#problems)

---

## What is a Binary Search Tree?

A **Binary Search Tree (BST)** is a binary tree with a special ordering property that makes searching efficient.

### The BST Property

**Core Rule:** For every node:
- **All values in left subtree < node value**
- **All values in right subtree > node value**

**Visual:**
```
Valid BST:
       8
      / \
     3   10
    / \    \
   1   6    14
      / \   /
     4   7 13

For node 8:
- Left subtree: 3, 1, 6, 4, 7 (all < 8) ✓
- Right subtree: 10, 14, 13 (all > 8) ✓

For node 3:
- Left subtree: 1 (< 3) ✓
- Right subtree: 6, 4, 7 (all > 3) ✓
```

**Invalid BST:**
```
NOT a BST:
       8
      / \
     3   10
    / \    \
   1   6    14
      / \   /
     9   7 13
     ↑
   9 > 8 but in left subtree! ✗

For node 3:
- Right subtree has 9
- 9 > 8 (violates BST property at root) ✗
```

### Why BST is Important

**1. Efficient Searching: O(log n) in balanced BST**
```
Search for 7 in BST:

       8
      / \
     3   10
    / \    \
   1   6    14
      / \
     4   7

Steps:
1. Start at 8: 7 < 8, go left
2. At 3: 7 > 3, go right
3. At 6: 7 > 6, go right
4. At 7: Found! ✓

Only 4 comparisons for 7 nodes!
Binary search in a tree!
```

**Compare to unsorted tree: O(n)**
```
Must check every node!
```

**2. Sorted order via Inorder traversal**
```
BST:
    5
   / \
  3   7
 / \   \
1   4   9

Inorder: [1, 3, 4, 5, 7, 9] ← Sorted! ✓
```

**3. Real-world applications:**
- Databases (indexing)
- File systems
- Auto-complete
- Range queries

---

## BST Properties & Operations

### Property 1: Inorder Gives Sorted Sequence

**Key property:** Inorder traversal of BST gives sorted array!

```
BST:
      4
     / \
    2   6
   / \ / \
  1  3 5  7

Inorder: [1, 2, 3, 4, 5, 6, 7] ← Ascending order!
```

**Why?**
```
Inorder: Left → Root → Right

For each node:
1. Visit all smaller values (left)
2. Visit node
3. Visit all larger values (right)

Result: Sorted sequence!
```

### Property 2: Efficient Search

**Binary search property:**

```python
def search(root, target):
    if not root:
        return None
    
    if target < root.val:
        return search(root.left, target)  # Go left
    elif target > root.val:
        return search(root.right, target)  # Go right
    else:
        return root  # Found!
```

**Time complexity:**
- Balanced BST: O(log n)
- Skewed BST: O(n)

```
Balanced:           Skewed:
      4                 1
     / \                 \
    2   6                 2
   / \ / \                 \
  1  3 5  7                 3
                             \
Height: log n               Height: n
Search: O(log n)           Search: O(n)
```

### Property 3: No Duplicates (Usually)

Most BST implementations don't allow duplicates.

```
If duplicates needed:
- Option 1: Allow equal values on one side (left or right)
- Option 2: Store count in node
- Option 3: Use different data structure
```

### Property 4: Min and Max

```
Minimum: Leftmost node
Maximum: Rightmost node

BST:
      8
     / \
    3   10
   /      \
  1        14
  ↑         ↑
 Min       Max

Min = keep going left
Max = keep going right
```

---

## BST vs Binary Tree

| Feature | Binary Tree | BST |
|---------|-------------|-----|
| Ordering | None | Left < Root < Right |
| Search | O(n) | O(log n) balanced, O(n) skewed |
| Inorder | Random order | Sorted order |
| Insert | O(1) at known position | O(log n) to find position |
| Use case | Hierarchical data | Searchable data |

**Example:**

```
Binary Tree (no order):
      5
     / \
    8   3
   / \
  2   9

Inorder: [2, 8, 5, 9, 3] ← Not sorted


BST (ordered):
      5
     / \
    3   8
   /   / \
  2   7   9

Inorder: [2, 3, 5, 7, 8, 9] ← Sorted!
```

---

## Problems

### Problem 1: Search in a Binary Search Tree

**[LeetCode 700 - Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)**

#### Problem Statement

You are given the `root` of a binary search tree (BST) and an integer `val`. Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

**Example 1:**
```
Input: root = [4,2,7,1,3], val = 2

Tree:
      4
     / \
    2   7
   / \
  1   3

Output: [2,1,3]
(The subtree rooted at node 2)
```

**Example 2:**
```
Input: root = [4,2,7,1,3], val = 5
Output: null
```

#### Understanding the Problem

**What we're doing:**
- Search for a specific value in BST
- Return the node (and its subtree) if found
- Return null if not found

**Key insight: Use BST property!**

```
Don't need to check every node
Use binary search logic!

If target < current:
    Go left (all values smaller)

If target > current:
    Go right (all values larger)

If target == current:
    Found!
```

#### Naive Approach (Ignores BST Property)

```python
# WRONG - Treats BST like regular tree
def searchBST(root, val):
    if not root:
        return None
    
    if root.val == val:
        return root
    
    # Check both left AND right (unnecessary!)
    left = searchBST(root.left, val)
    if left:
        return left
    
    return searchBST(root.right, val)
```

**Problems:**
- Checks both subtrees
- Time: O(n) - same as regular tree!
- Doesn't use BST property!

#### Optimal Approach: Binary Search

**Algorithm:**
1. Compare target with current node
2. If target < current: go left
3. If target > current: go right
4. If target == current: found!
5. If null: not found

**Visual:**
```
Search for 3:
      4
     / \
    2   7
   / \
  1   3

Step 1: At 4
3 < 4 → Go left

Step 2: At 2
3 > 2 → Go right

Step 3: At 3
3 == 3 → Found! ✓
Return node 3
```

```
Search for 6:
      4
     / \
    2   7
   / \
  1   3

Step 1: At 4
6 > 4 → Go right

Step 2: At 7
6 < 7 → Go left

Step 3: At null
Not found → Return null
```

#### Solution - Recursive

```python
def searchBST(root: TreeNode, val: int) -> TreeNode:
    """
    Search in BST using recursion
    
    Time: O(h) where h = height
          O(log n) balanced, O(n) skewed
    Space: O(h) - recursion stack
    """
    # Base case: not found or found
    if not root or root.val == val:
        return root
    
    # Use BST property to decide direction
    if val < root.val:
        return searchBST(root.left, val)
    else:
        return searchBST(root.right, val)
```

#### Solution - Iterative

```python
def searchBST(root: TreeNode, val: int) -> TreeNode:
    """
    Iterative search - preferred for BST
    
    Time: O(h)
    Space: O(1) - no recursion stack
    """
    current = root
    
    while current:
        if val == current.val:
            return current
        elif val < current.val:
            current = current.left
        else:
            current = current.right
    
    return None
```

**Why iterative is often better for BST:**
- No recursion overhead
- O(1) space instead of O(h)
- Simpler for this problem

#### Detailed Trace

```python
Tree:
      8
     / \
    3   10
   / \    \
  1   6   14
     / \  /
    4   7 13

Search for 13:

Iteration 1:
current = 8
13 > 8 → go right
current = 10

Iteration 2:
current = 10
13 > 10 → go right
current = 14

Iteration 3:
current = 14
13 < 14 → go left
current = 13

Iteration 4:
current = 13
13 == 13 → Found! ✓
Return node 13
```

```
Search for 5:

Iteration 1:
current = 8
5 < 8 → go left
current = 3

Iteration 2:
current = 3
5 > 3 → go right
current = 6

Iteration 3:
current = 6
5 < 6 → go left
current = 4

Iteration 4:
current = 4
5 > 4 → go right
current = None

current is None → Not found
Return None
```

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None, val = 5
Output: None
```

**Edge Case 2: Single node match**
```python
root = [1], val = 1
Output: [1]
```

**Edge Case 3: Single node no match**
```python
root = [1], val = 2
Output: None
```

**Edge Case 4: Value at root**
```python
root = [4,2,7], val = 4
Output: [4,2,7]
```

#### Common Mistakes

**Mistake 1: Checking both subtrees**
```python
# WRONG - doesn't use BST property
left = searchBST(root.left, val)
right = searchBST(root.right, val)
return left or right

# RIGHT - only check one direction
if val < root.val:
    return searchBST(root.left, val)
else:
    return searchBST(root.right, val)
```

**Mistake 2: Wrong comparison**
```python
# WRONG
if val <= root.val:  # Equal should return!
    return searchBST(root.left, val)

# RIGHT
if val < root.val:  # Only less than goes left
    return searchBST(root.left, val)
elif val > root.val:
    return searchBST(root.right, val)
else:
    return root  # Equal means found
```

**Mistake 3: Returning subtree incorrectly**
```python
# WRONG
if root.val == val:
    return root.val  # Returns integer, not TreeNode!

# RIGHT
if root.val == val:
    return root  # Return the node itself
```

#### Time & Space Complexity

**Time Complexity: O(h)**
- h = height of tree
- Balanced BST: O(log n)
- Skewed BST: O(n)

```
Balanced:           Skewed:
      4                 1
     / \                 \
    2   6                 2
   / \ / \                 \
  1  3 5  7                 3

Height: log n           Height: n
Time: O(log n)         Time: O(n)
```

**Space Complexity:**
- Recursive: O(h) - call stack
- Iterative: O(1) - no extra space

---

### Problem 2: Insert into a Binary Search Tree

**[LeetCode 701 - Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)**

#### Problem Statement

You are given the `root` node of a binary search tree (BST) and a `value` to insert into the tree. Return the root node of the BST after the insertion. It is **guaranteed** that the new value does not exist in the original BST.

**Example 1:**
```
Input: root = [4,2,7,1,3], val = 5

Before:
      4
     / \
    2   7
   / \
  1   3

After:
      4
     / \
    2   7
   / \ /
  1  3 5

Output: [4,2,7,1,3,5]
```

**Example 2:**
```
Input: root = [40,20,60,10,30,50,70], val = 25

Before:
        40
       /  \
     20    60
    / \   / \
   10 30 50 70

After:
        40
       /  \
     20    60
    / \   / \
   10 30 50 70
     /
    25

Output: [40,20,60,10,30,50,70,null,null,25]
```

#### Understanding the Problem

**Key insight:** New value always becomes a leaf!

```
Why?

We traverse down the tree following BST property
When we reach null, that's where new value belongs!

No need to restructure tree (unless balancing)
```

**Visual:**
```
Insert 5 into:
      4
     / \
    2   7
   / \
  1   3

5 < 7 → Go left from 7
Reach null → Insert 5 here!

      4
     / \
    2   7
   / \ /
  1  3 5
```

#### Approach: Find Correct Position

**Algorithm:**
1. Start at root
2. Compare value with current node
3. If value < current: go left
4. If value > current: go right
5. When reach null: insert here!

**Why this works:**
- Maintains BST property
- New value finds correct position
- No rebalancing needed (for now)

#### Visual Step-by-Step

```
Insert 5 into:
      4
     / \
    2   7
   / \
  1   3

Step 1: At 4
5 > 4 → Go right

      4
     / \
    2   7
         ↓
      Go right

Step 2: At 7
5 < 7 → Go left

      4
     / \
    2   7
       ↙
    Go left

Step 3: At null (7's left child)
Insert 5 here!

      4
     / \
    2   7
   / \ /
  1  3 5 ← New node!
```

#### Solution - Recursive

```python
def insertIntoBST(root: TreeNode, val: int) -> TreeNode:
    """
    Insert value into BST recursively
    
    Time: O(h) where h = height
    Space: O(h) - recursion stack
    """
    # Base case: found insertion point
    if not root:
        return TreeNode(val)
    
    # Recursive case: find correct subtree
    if val < root.val:
        root.left = insertIntoBST(root.left, val)
    else:
        root.right = insertIntoBST(root.right, val)
    
    return root
```

**Understanding the recursion:**
```python
root.left = insertIntoBST(root.left, val)

This does:
1. Recursively insert into left subtree
2. Return the (modified) left subtree
3. Reconnect it to current node

Same for right subtree
```

#### Solution - Iterative

```python
def insertIntoBST(root: TreeNode, val: int) -> TreeNode:
    """
    Insert value iteratively
    
    Time: O(h)
    Space: O(1) - no recursion
    """
    # Edge case: empty tree
    if not root:
        return TreeNode(val)
    
    # Find parent of insertion point
    current = root
    while True:
        if val < current.val:
            # Go left
            if not current.left:
                current.left = TreeNode(val)
                break
            current = current.left
        else:
            # Go right
            if not current.right:
                current.right = TreeNode(val)
                break
            current = current.right
    
    return root
```

**Why check for null child?**
```python
if not current.left:
    current.left = TreeNode(val)  # Insert here
    break

If we don't check:
- current = current.left would be None
- Loop would error on None.val
```

#### Detailed Trace

```python
Tree:
      4
     / \
    2   7
   / \
  1   3

Insert 5:

Recursive trace:

insertIntoBST(4, 5):
│ 5 > 4, go right
│ root.right = insertIntoBST(7, 5)
│   │
│   insertIntoBST(7, 5):
│   │ 5 < 7, go left
│   │ root.left = insertIntoBST(None, 5)
│   │   │
│   │   insertIntoBST(None, 5):
│   │   │ root is None
│   │   │ Return TreeNode(5)
│   │   
│   │ 7.left = TreeNode(5)
│   │ Return 7
│   
│ 4.right = 7 (with new child 5)
│ Return 4

Final tree:
      4
     / \
    2   7
   / \ /
  1  3 5
```

```
Iterative trace:

Tree:
      4
     / \
    2   7
   / \
  1   3

Insert 5:

current = 4
5 > 4, go right
current.right exists? Yes
current = 7

current = 7
5 < 7, go left
current.left exists? No
current.left = TreeNode(5)
break

Final tree:
      4
     / \
    2   7
   / \ /
  1  3 5
```

#### Edge Cases

**Edge Case 1: Insert into empty tree**
```python
root = None, val = 5
Output: TreeNode(5)
```

**Edge Case 2: Insert smaller than all**
```python
root = [4,2,7], val = 1

Before:
    4
   / \
  2   7

After:
    4
   / \
  2   7
 /
1
```

**Edge Case 3: Insert larger than all**
```python
root = [4,2,7], val = 10

Before:
    4
   / \
  2   7

After:
    4
   / \
  2   7
       \
        10
```

#### Common Mistakes

**Mistake 1: Not returning root**
```python
# WRONG
def insertIntoBST(root, val):
    if not root:
        return TreeNode(val)
    
    if val < root.val:
        insertIntoBST(root.left, val)  # Forgot to assign!
    else:
        insertIntoBST(root.right, val)
    
    return root

# RIGHT
if val < root.val:
    root.left = insertIntoBST(root.left, val)
```

**Mistake 2: Creating new root**
```python
# WRONG
def insertIntoBST(root, val):
    if not root:
        root = TreeNode(val)  # Doesn't modify original!
        return root

# RIGHT
if not root:
    return TreeNode(val)  # Return new node to parent
```

**Mistake 3: Not handling empty tree**
```python
# WRONG
def insertIntoBST(root, val):
    current = root  # Error if root is None!
    while current:
        ...

# RIGHT
if not root:
    return TreeNode(val)
```

#### Time & Space Complexity

**Time Complexity: O(h)**
- h = height of tree
- Balanced: O(log n)
- Skewed: O(n)

**Space Complexity:**
- Recursive: O(h) - call stack
- Iterative: O(1) - no extra space

---
