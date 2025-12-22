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

### Problem 3: Convert Sorted Array to Binary Search Tree

**[LeetCode 108 - Convert Sorted Array to BST](https://leetcode.com/problems/convert-sorted-array-to-bst/)**

#### Problem Statement

Given an integer array `nums` where the elements are sorted in **ascending order**, convert it to a **height-balanced** binary search tree.

A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

**Example 1:**
```
Input: nums = [-10,-3,0,5,9]

Output:     0
           / \
         -3   9
         /   /
       -10  5

(Other valid outputs exist)
```

**Example 2:**
```
Input: nums = [1,3]

Output:   3      OR      1
         /                \
        1                  3

(Both are valid)
```

#### Understanding the Problem

**What we need:**
1. Create BST from sorted array
2. BST must be **balanced**

**What is balanced?**
```
Balanced (height difference ≤ 1):
      2
     / \
    1   3

Heights: left=1, right=1
Difference: 0 ≤ 1 ✓


Not balanced:
      1
       \
        2
         \
          3

Heights: left=0, right=2
Difference: 2 > 1 ✗
```

**Key insight: Use middle element as root!**

```
Array: [1, 2, 3, 4, 5]

Why middle (3)?

If we use 3 as root:
- Left: [1, 2] (2 elements)
- Right: [4, 5] (2 elements)
- Balanced! ✓

      3
     / \
    ?   ?
   2    2 elements each

If we use 1 as root:
- Left: [] (0 elements)
- Right: [2, 3, 4, 5] (4 elements)
- Not balanced! ✗

      1
       \
        ?
        4 elements
```

#### Why Middle Element Works

**Mathematical proof:**

```
Array of size n:
Middle index = n // 2

Left side: 0 to mid-1 = mid elements
Right side: mid+1 to n-1 = n-mid-1 elements

For n = 5:
mid = 2
Left: 2 elements
Right: 2 elements
Difference: 0 ✓

For n = 6:
mid = 3
Left: 3 elements
Right: 2 elements
Difference: 1 ✓
```

**Visual:**
```
Array: [-10, -3, 0, 5, 9]
Index:   0    1  2  3  4

Middle: index 2, value 0

      0
     / \
  [-10,-3] [5,9]
   2 elem  2 elem
   
Balanced!
```

#### Approach: Recursive Middle Selection

**Algorithm:**
1. Find middle of current array segment
2. Make it root
3. Recursively build left subtree from left half
4. Recursively build right subtree from right half

**Why recursion?**
```
Same pattern repeats at every level:
- Find middle
- Make it root
- Recurse on both halves

Perfect for recursion!
```

#### Visual Step-by-Step

```
Array: [1, 2, 3, 4, 5]

Step 1: Find middle of [1,2,3,4,5]
mid = 2 (value 3)

      3
     / \
    ?   ?

Step 2: Left subtree from [1,2]
mid = 0 (value 1)

      3
     / \
    1   ?
     \
      ?

Step 3: Right of 1 from [2]
mid = 0 (value 2)

      3
     / \
    1   ?
     \
      2

Step 4: Right subtree of 3 from [4,5]
mid = 0 (value 4)

      3
     / \
    1   4
     \   \
      2   ?

Step 5: Right of 4 from [5]
mid = 0 (value 5)

      3
     / \
    1   4
     \   \
      2   5

Done! ✓
```

#### Solution with Detailed Comments

```python
def sortedArrayToBST(nums: list[int]) -> TreeNode:
    """
    Convert sorted array to balanced BST
    
    Time: O(n) - visit each element once
    Space: O(log n) - recursion depth for balanced tree
    """
    
    def buildBST(left: int, right: int) -> TreeNode:
        """
        Build BST from array segment [left, right]
        
        Parameters:
        - left: start index (inclusive)
        - right: end index (inclusive)
        
        Returns:
        - Root of BST for this segment
        """
        
        # Base case: invalid range
        if left > right:
            return None
        
        # Find middle index
        # Use (left + right) // 2
        mid = (left + right) // 2
        
        # Create root node with middle value
        root = TreeNode(nums[mid])
        
        # Recursively build left subtree
        # Left half: from left to mid-1
        root.left = buildBST(left, mid - 1)
        
        # Recursively build right subtree
        # Right half: from mid+1 to right
        root.right = buildBST(mid + 1, right)
        
        return root
    
    # Start with entire array
    return buildBST(0, len(nums) - 1)
```

**Understanding the helper function:**

```python
def buildBST(left, right):
```

**Why use indices instead of slicing?**

```python
# SLOWER - creates new arrays
def buildBST(nums):
    if not nums:
        return None
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    root.left = buildBST(nums[:mid])      # New array!
    root.right = buildBST(nums[mid+1:])   # New array!
    return root

# FASTER - uses indices
def buildBST(left, right):
    if left > right:
        return None
    mid = (left + right) // 2
    root = TreeNode(nums[mid])
    root.left = buildBST(left, mid - 1)   # No copying!
    root.right = buildBST(mid + 1, right) # No copying!
    return root

# Array slicing is O(n) per call
# Using indices is O(1) per call
```

#### Detailed Code Trace

```python
nums = [1, 2, 3]

Call: buildBST(0, 2)
│ left=0, right=2
│ mid = (0 + 2) // 2 = 1
│ root = TreeNode(nums[1]) = TreeNode(2)
│
├─ root.left = buildBST(0, 0)
│  │ left=0, right=0
│  │ mid = (0 + 0) // 2 = 0
│  │ root = TreeNode(nums[0]) = TreeNode(1)
│  │
│  ├─ root.left = buildBST(0, -1)
│  │  │ left=0, right=-1
│  │  │ left > right ✓
│  │  │ Return None
│  │
│  └─ root.right = buildBST(1, 0)
│     │ left=1, right=0
│     │ left > right ✓
│     │ Return None
│  │
│  Return TreeNode(1)
│
└─ root.right = buildBST(2, 2)
   │ left=2, right=2
   │ mid = (2 + 2) // 2 = 2
   │ root = TreeNode(nums[2]) = TreeNode(3)
   │
   ├─ root.left = buildBST(2, 1)
   │  │ left=2, right=1
   │  │ left > right ✓
   │  │ Return None
   │
   └─ root.right = buildBST(3, 2)
      │ left=3, right=2
      │ left > right ✓
      │ Return None
   │
   Return TreeNode(3)
│
Return TreeNode(2) with children 1 and 3

Final tree:
    2
   / \
  1   3
```

**Another detailed example:**

```python
nums = [-10, -3, 0, 5, 9]
indices: 0   1   2  3  4

buildBST(0, 4):
  left=0, right=4
  mid = (0 + 4) // 2 = 2
  root = TreeNode(0)
  
  Left subtree: buildBST(0, 1)
    Array segment: [-10, -3]
    left=0, right=1
    mid = (0 + 1) // 2 = 0
    root = TreeNode(-10)
    
    Left: buildBST(0, -1) → None
    Right: buildBST(1, 1)
      Array segment: [-3]
      left=1, right=1
      mid = 1
      root = TreeNode(-3)
      Left: buildBST(1, 0) → None
      Right: buildBST(2, 1) → None
      Return TreeNode(-3)
    
    Return TreeNode(-10) with right child -3
  
  Right subtree: buildBST(3, 4)
    Array segment: [5, 9]
    left=3, right=4
    mid = (3 + 4) // 2 = 3
    root = TreeNode(5)
    
    Left: buildBST(3, 2) → None
    Right: buildBST(4, 4)
      Array segment: [9]
      left=4, right=4
      mid = 4
      root = TreeNode(9)
      Left: buildBST(4, 3) → None
      Right: buildBST(5, 4) → None
      Return TreeNode(9)
    
    Return TreeNode(5) with right child 9
  
  Return TreeNode(0) with children

Final tree:
      0
     / \
   -10  5
     \   \
     -3   9
```

#### Understanding Index Calculations

**Why `mid = (left + right) // 2`?**

```python
# Example: [1, 2, 3, 4, 5]
# Indices:  0  1  2  3  4

left = 0, right = 4
mid = (0 + 4) // 2 = 2

Array: [1, 2, 3, 4, 5]
Index:  0  1  2  3  4
              ↑
            mid

Element at mid: nums[2] = 3 ✓

Left portion: indices 0 to 1 (elements 1, 2)
Right portion: indices 3 to 4 (elements 4, 5)
```

**Why `mid - 1` and `mid + 1`?**

```python
# Left subtree
root.left = buildBST(left, mid - 1)

# mid is already used for root
# So left goes from left to mid-1

# Right subtree  
root.right = buildBST(mid + 1, right)

# mid is already used for root
# So right goes from mid+1 to right

Example:
Array: [1, 2, 3, 4, 5]
mid = 2 (value 3 is root)

Left: buildBST(0, 1) → [1, 2]
Right: buildBST(3, 4) → [4, 5]
```

#### Alternative: Choosing Different Middle

**For even length, two middles possible:**

```python
Array: [1, 2, 3, 4]
Length: 4

Option 1: mid = (0 + 3) // 2 = 1
Use nums[1] = 2

      2
     / \
    1   ?

Option 2: mid = (0 + 3 + 1) // 2 = 2
Use nums[2] = 3

      3
     / \
    ?   4

Both valid!
```

**To prefer right middle:**

```python
mid = (left + right + 1) // 2
```

#### Edge Cases

**Edge Case 1: Empty array**
```python
nums = []

buildBST(0, -1)
left=0, right=-1
left > right ✓
Return None
```

**Edge Case 2: Single element**
```python
nums = [1]

buildBST(0, 0)
left=0, right=0
mid = 0
root = TreeNode(1)

root.left = buildBST(0, -1) → None
root.right = buildBST(1, 0) → None

Tree:
  1
```

**Edge Case 3: Two elements**
```python
nums = [1, 2]

buildBST(0, 1)
mid = 0
root = TreeNode(1)

root.left = buildBST(0, -1) → None
root.right = buildBST(1, 1) → TreeNode(2)

Tree:
  1
   \
    2
```

**Edge Case 4: All same values**
```python
nums = [1, 1, 1]

Valid BST? No! BST can't have duplicates!

But if problem allows:
     1
    / \
   1   1
```

#### Common Mistakes

**Mistake 1: Off-by-one errors**
```python
# WRONG
root.left = buildBST(left, mid)  # Includes mid twice!

# RIGHT
root.left = buildBST(left, mid - 1)  # Excludes mid
```

**Mistake 2: Creating new arrays (inefficient)**
```python
# WRONG - O(n) space per call
def sortedArrayToBST(nums):
    if not nums:
        return None
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    root.left = sortedArrayToBST(nums[:mid])
    root.right = sortedArrayToBST(nums[mid+1:])
    return root

# RIGHT - O(1) space per call
def buildBST(left, right):
    # Use indices, don't create new arrays
```

**Mistake 3: Not handling empty range**
```python
# WRONG
def buildBST(left, right):
    if left == right:  # Wrong base case!
        return TreeNode(nums[left])
    # What if left > right?

# RIGHT
def buildBST(left, right):
    if left > right:  # Handles empty range
        return None
```

**Mistake 4: Forgetting to return**
```python
# WRONG
def buildBST(left, right):
    if left > right:
        return None
    mid = (left + right) // 2
    root = TreeNode(nums[mid])
    buildBST(left, mid - 1)  # Forgot to assign!
    buildBST(mid + 1, right)

# RIGHT
root.left = buildBST(left, mid - 1)
root.right = buildBST(mid + 1, right)
return root
```

#### Time & Space Complexity

**Time Complexity: O(n)**
```
Why?

Visit each element once to create node
Each element appears in exactly one recursive call

T(n) = T(n/2) + T(n/2) + O(1)
     = 2T(n/2) + O(1)

By Master Theorem: O(n)

Detailed:
n nodes → n TreeNode creations → O(n)
```

**Space Complexity: O(log n)**
```
Recursion depth = height of tree
Balanced tree height = log n

Call stack at any time:
buildBST(0, n-1)
  buildBST(0, n/2)
    buildBST(0, n/4)
      ...
        buildBST(small range)

Depth = log n
Space = O(log n)

Note: Output tree uses O(n) space
But that's not counted in space complexity
(We'd need that space anyway)
```

**Worst case (if tree weren't balanced):**
```
If we always picked first/last element:
Tree would be skewed
Space: O(n)

But middle selection ensures balance!
Space: O(log n) ✓
```

---

### Problem 4: Two Sum IV - Input is a BST

**[LeetCode 653 - Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)**

#### Problem Statement

Given the `root` of a Binary Search Tree and a target number `k`, return `true` if there exist two elements in the BST such that their sum is equal to `k`, or `false` otherwise.

**Example 1:**
```
Input: root = [5,3,6,2,4,null,7], k = 9

Tree:
      5
     / \
    3   6
   / \   \
  2   4   7

Output: true
Explanation: 3 + 6 = 9
```

**Example 2:**
```
Input: root = [5,3,6,2,4,null,7], k = 28
Output: false
```

#### Understanding the Problem

**What we're looking for:**
- Two DIFFERENT nodes in BST
- Their values sum to k
- Return true/false

**Examples:**
```
Tree:
      5
     / \
    3   6
   / \   \
  2   4   7

k = 9
Pairs:
2+7=9 ✓
3+6=9 ✓
4+5=9 ✓

Answer: True (any pair works)


k = 15
Pairs:
Largest: 7+6=13
No pair sums to 15

Answer: False
```

**Key insight: This is just Two Sum problem on a tree!**

```
Classic Two Sum:
Array: [2, 3, 4, 5, 6, 7]
Target: 9

Solution: Use set to check for complement

Same idea here!
```

#### Approach 1: Convert to Array (Easy but not optimal)

**Algorithm:**
1. Do inorder traversal → sorted array
2. Use two-pointer technique
3. Check if any pair sums to k

```python
def findTarget(root: TreeNode, k: int) -> bool:
    """
    Convert BST to sorted array, then two pointers
    
    Time: O(n)
    Space: O(n) - array storage
    """
    # Step 1: Inorder traversal to get sorted array
    def inorder(node):
        if not node:
            return []
        return inorder(node.left) + [node.val] + inorder(node.right)
    
    arr = inorder(root)
    
    # Step 2: Two pointers on sorted array
    left = 0
    right = len(arr) - 1
    
    while left < right:
        current_sum = arr[left] + arr[right]
        
        if current_sum == k:
            return True
        elif current_sum < k:
            left += 1  # Need larger sum
        else:
            right -= 1  # Need smaller sum
    
    return False
```

**Why two pointers work on sorted array:**

```
Array: [2, 3, 4, 5, 6, 7]
Target: 9

left=0, right=5
2 + 7 = 9 ✓ Found!

If sum too small: move left (increase sum)
If sum too large: move right (decrease sum)
```

**Pros:**
- Simple to understand
- Leverages BST sorted property

**Cons:**
- Uses O(n) extra space
- Two passes through tree

#### Approach 2: Set with Complement (Optimal)

**Algorithm:**
1. Traverse tree (any order)
2. For each node, check if complement exists in set
3. Add current value to set

**What is complement?**
```
If target = 9 and current = 3
Complement = 9 - 3 = 6

If 6 exists in set → Found pair!
```

```python
def findTarget(root: TreeNode, k: int) -> bool:
    """
    Use set to track seen values
    
    Time: O(n) - visit each node once
    Space: O(n) - set storage
    """
    seen = set()
    
    def dfs(node):
        """Check current node and recurse"""
        if not node:
            return False
        
        # Calculate complement
        complement = k - node.val
        
        # Check if complement exists
        if complement in seen:
            return True
        
        # Add current value to set
        seen.add(node.val)
        
        # Check left and right subtrees
        return dfs(node.left) or dfs(node.right)
    
    return dfs(root)
```

**Understanding the code:**

```python
complement = k - node.val
```
**What this does:**
```
If k = 9 and node.val = 3
complement = 9 - 3 = 6

We're asking: "Is there a 6 in the tree?"
If yes, then 3 + 6 = 9 ✓
```

```python
if complement in seen:
    return True
```
**What this does:**
```
Check if we've already seen the complement

Example:
k = 9, current = 3
complement = 6

If 6 is in seen:
  We saw 6 earlier
  3 + 6 = 9 ✓
  Found pair!
```

```python
seen.add(node.val)
```
**What this does:**
```
Add current value to set
So future nodes can check if complement exists

Example:
Process node 3:
- Check if 6 exists (not yet)
- Add 3 to seen
- Later, when we process 6:
  - Check if 3 exists (yes!)
  - Found pair!
```

#### Detailed Trace

```python
Tree:
      5
     / \
    3   6
   / \
  2   4

k = 9
seen = {}


DFS(5):
│ complement = 9 - 5 = 4
│ Is 4 in seen? No
│ Add 5 to seen: {5}
│
├─ DFS(3):
│  │ complement = 9 - 3 = 6
│  │ Is 6 in seen? No
│  │ Add 3 to seen: {5, 3}
│  │
│  ├─ DFS(2):
│  │  │ complement = 9 - 2 = 7
│  │  │ Is 7 in seen? No
│  │  │ Add 2 to seen: {5, 3, 2}
│  │  │ Left: DFS(None) → False
│  │  │ Right: DFS(None) → False
│  │  │ Return False
│  │
│  └─ DFS(4):
│     │ complement = 9 - 4 = 5
│     │ Is 5 in seen? Yes! ✓
│     │ Return True
│
└─ (Returns True before checking right)

Return True ✓
```

**Another example:**

```python
Tree:
      5
     / \
    3   6
   /     \
  2       7

k = 10
seen = {}


DFS(5):
│ complement = 10 - 5 = 5
│ Is 5 in seen? No
│ Add 5 to seen: {5}
│
├─ DFS(3):
│  │ complement = 10 - 3 = 7
│  │ Is 7 in seen? No
│  │ Add 3 to seen: {5, 3}
│  │
│  ├─ DFS(2):
│  │  │ complement = 10 - 2 = 8
│  │  │ Is 8 in seen? No
│  │  │ Add 2 to seen: {5, 3, 2}
│  │  │ Return False
│  │
│  └─ DFS(None) → False
│  │
│  Return False
│
└─ DFS(6):
   │ complement = 10 - 6 = 4
   │ Is 4 in seen? No
   │ Add 6 to seen: {5, 3, 2, 6}
   │
   ├─ DFS(None) → False
   │
   └─ DFS(7):
      │ complement = 10 - 7 = 3
      │ Is 3 in seen? Yes! ✓
      │ Return True

Return True ✓
```

#### Why Use DFS (or BFS)?

**Both work! Choice depends on preference.**

**DFS version (above):**
```python
def dfs(node):
    if not node:
        return False
    
    complement = k - node.val
    if complement in seen:
        return True
    
    seen.add(node.val)
    return dfs(node.left) or dfs(node.right)
```

**BFS version:**
```python
def findTarget_BFS(root: TreeNode, k: int) -> bool:
    """
    BFS with set
    """
    if not root:
        return False
    
    seen = set()
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        
        complement = k - node.val
        if complement in seen:
            return True
        
        seen.add(node.val)
        
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    
    return False
```

**BFS trace:**

```python
Tree:
      5
     / \
    3   6
   /
  2

k = 8
seen = {}
queue = [5]


Iteration 1:
node = 5
complement = 8 - 5 = 3
Is 3 in seen? No
Add 5 to seen: {5}
Add children: queue = [3, 6]


Iteration 2:
node = 3
complement = 8 - 3 = 5
Is 5 in seen? Yes! ✓
Return True
```

#### Edge Cases

**Edge Case 1: Empty tree**
```python
root = None, k = 0
Output: False
(No nodes to sum)
```

**Edge Case 2: Single node**
```python
root = [1], k = 2
Output: False
(Need TWO different nodes)
```

**Edge Case 3: Can't use same node twice**
```python
root = [2], k = 4

Can't do 2 + 2 = 4 using same node!
Output: False

How code handles this:
- At node 2: complement = 4 - 2 = 2
- Is 2 in seen? No (haven't added yet)
- Add 2 to seen
- No children
- Return False ✓
```

**Edge Case 4: Pair exists**
```python
root = [2,1,3], k = 4

Tree:
    2
   / \
  1   3

1 + 3 = 4 ✓
Output: True
```

#### Common Mistakes

**Mistake 1: Adding to set before checking**
```python
# WRONG
seen.add(node.val)
if (k - node.val) in seen:
    return True

# Problem: node 2, k = 4
# Add 2 to seen: {2}
# Check if 2 in seen: Yes!
# But can't use same node twice!

# RIGHT
if (k - node.val) in seen:
    return True
seen.add(node.val)  # Add after checking
```

**Mistake 2: Not using OR correctly**
```python
# WRONG
return dfs(node.left) and dfs(node.right)
# Requires BOTH subtrees to have answer

# RIGHT
return dfs(node.left) or dfs(node.right)
# Need at least ONE subtree with answer
```

**Mistake 3: Forgetting base case**
```python
# WRONG
def dfs(node):
    complement = k - node.val  # Error if node is None!

# RIGHT
def dfs(node):
    if not node:
        return False
    complement = k - node.val
```

#### Time & Space Complexity

**Both approaches:**

**Time Complexity: O(n)**
- Visit each node once
- Set lookup: O(1)
- Total: O(n)

**Space Complexity: O(n)**
- Set stores up to n values
- Recursion depth: O(h)
  - DFS: O(h) call stack
  - BFS: O(w) queue size
- Total: O(n) for set

**Comparison:**

```
Approach 1 (Array + Two Pointers):
Time: O(n)
Space: O(n)
Passes: 2 (inorder + two pointers)

Approach 2 (Set):
Time: O(n)
Space: O(n)
Passes: 1 (single traversal)

Both O(n), but set approach:
✓ Single pass
✓ Can stop early (return True immediately)
✓ Simpler code
```

---
