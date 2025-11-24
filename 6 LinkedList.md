# Complete Linked Lists Guide for Interview Preparation

## Table of Contents
1. [What is a Linked List?](#what-is-a-linked-list)
2. [Types of Linked Lists](#types-of-linked-lists)
3. [Core Concepts](#core-concepts)
4. [Common Patterns](#common-patterns)
5. [Problems](#problems)

---

## What is a Linked List?

A **linked list** is a linear data structure where elements (called **nodes**) are stored in separate objects that are connected using **pointers/references**.

### Why Use Linked Lists?

**Advantages over Arrays:**
- Dynamic size (can grow/shrink easily)
- Efficient insertions/deletions at the beginning (O(1))
- No wasted space from pre-allocation

**Disadvantages:**
- No random access (can't jump to index 5 directly)
- Extra memory for storing pointers
- Sequential access only

### Real-World Analogy

Think of a **treasure hunt** where each clue leads to the next location:
- Each location = a node
- Each clue = a pointer/reference to next location
- You must visit locations in order (no skipping!)

---

## Types of Linked Lists

### 1. Singly Linked List

Each node points to the **next** node only.

```
[Data|Next] -> [Data|Next] -> [Data|Next] -> NULL

Example:
[1|•] -> [2|•] -> [3|•] -> NULL
```

**Node Structure:**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val      # Data stored in node
        self.next = next    # Reference to next node
```

### 2. Doubly Linked List

Each node points to **both next and previous** nodes.

```
NULL <- [Prev|Data|Next] <-> [Prev|Data|Next] <-> [Prev|Data|Next] -> NULL

Example:
NULL <- [•|1|•] <-> [•|2|•] <-> [•|3|•] -> NULL
```

**Node Structure:**
```python
class DoublyListNode:
    def __init__(self, val=0, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev
```

### 3. Circular Linked List

The last node points back to the first node (forms a circle).

```
     +-----------------------+
     |                       |
     v                       |
[Data|•] -> [Data|•] -> [Data|•]

Example:
[1|•] -> [2|•] -> [3|•]
 ^                 |
 +-----------------+
```

---

## Core Concepts

### 1. Head and Tail

**Head:** The first node in the linked list
**Tail:** The last node in the linked list

```
HEAD                          TAIL
  |                            |
  v                            v
[1|•] -> [2|•] -> [3|•] -> [4|•] -> NULL
```

### 2. Traversal

**How to visit each node:**

```python
def traverse(head):
    current = head
    while current:
        print(current.val)
        current = current.next  # Move to next node
```

**Why we need traversal:**
- Can't access nodes by index like arrays
- Must start at head and follow pointers

### 3. The "Runner" Technique (Fast & Slow Pointers)

A powerful pattern where we use two pointers moving at different speeds.

**Use cases:**
- Finding the middle of a list
- Detecting cycles
- Finding the nth node from end

```python
slow = head
fast = head

while fast and fast.next:
    slow = slow.next           # Move 1 step
    fast = fast.next.next      # Move 2 steps
```

**Why this works:**
- Fast pointer moves twice as fast as slow
- When fast reaches end, slow is at middle
- If there's a cycle, fast will eventually meet slow

### 4. Dummy Head

A **dummy node** placed before the actual head to simplify edge cases.

```
DUMMY                    HEAD
  |                       |
  v                       v
[-1|•] -> [1|•] -> [2|•] -> [3|•] -> NULL
```

**Why use a dummy head:**
- Handles edge case when head needs to be deleted
- Simplifies code (no special case for head)
- Makes all nodes equal (no special treatment for first node)

**Example:**
```python
dummy = ListNode(-1)
dummy.next = head
# Now we can treat head like any other node!
```

---

## Common Patterns

### Pattern 1: Two Pointers (Slow & Fast)

**When to use:**
- Finding middle of list
- Detecting cycles
- Finding kth node from end

**Template:**
```python
slow = head
fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

### Pattern 2: Dummy Head

**When to use:**
- Deleting nodes (especially head)
- Merging lists
- Reordering lists

**Template:**
```python
dummy = ListNode(-1)
dummy.next = head
current = dummy

# ... do operations ...

return dummy.next  # New head
```

### Pattern 3: Previous Pointer

**When to use:**
- Reversing a list
- Deleting nodes
- Reordering nodes

**Template:**
```python
prev = None
current = head

while current:
    next_node = current.next  # Save next
    current.next = prev       # Reverse pointer
    prev = current           # Move prev forward
    current = next_node      # Move current forward
```

---

## Problems

### Problem 1: Middle of the Linked List

**[LeetCode 876 - Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)**

#### Problem Statement

Given the head of a singly linked list, return the middle node. If there are two middle nodes, return the **second** middle node.

**Example 1:**
```
Input: [1,2,3,4,5]
Output: [3,4,5] (node with value 3)

Visual:
[1] -> [2] -> [3] -> [4] -> [5] -> NULL
               ↑
            middle
```

**Example 2:**
```
Input: [1,2,3,4,5,6]
Output: [4,5,6] (second middle node)

Visual:
[1] -> [2] -> [3] -> [4] -> [5] -> [6] -> NULL
                      ↑
              second middle
```

#### Understanding the Problem

**What we're looking for:**
- Odd length: The exact middle node
- Even length: The second of the two middle nodes

**Naive approach (doesn't work well):**
1. Count total nodes (requires one full traversal)
2. Traverse again to middle (n/2 nodes)
3. Total: O(n) + O(n/2) = O(n) but two passes

**Better approach: Fast & Slow Pointers (Floyd's Tortoise and Hare)**

#### Why Fast & Slow Pointers Work

**The key insight:**
- If one pointer moves **2x faster** than another
- When fast pointer reaches end
- Slow pointer will be at middle!

**Mathematical proof:**
```
Fast pointer: moves 2 steps per iteration
Slow pointer: moves 1 step per iteration

After k iterations:
- Fast has moved: 2k steps
- Slow has moved: k steps

When fast reaches end (position n):
- 2k = n
- k = n/2
- Slow is at position n/2 (the middle!)
```

#### Visual Walkthrough

**Example: [1,2,3,4,5]**

```
Initial state:
S F
[1] -> [2] -> [3] -> [4] -> [5] -> NULL

Step 1:
       S           F
[1] -> [2] -> [3] -> [4] -> [5] -> NULL
Slow moved 1 step (now at 2)
Fast moved 2 steps (now at 3)

Step 2:
              S                    F
[1] -> [2] -> [3] -> [4] -> [5] -> NULL
Slow moved 1 step (now at 3)
Fast moved 2 steps (now at 5)

Step 3:
Fast.next is NULL, so we stop
Slow is at position 3 (the middle!)
Return node with value 3
```

**Example: [1,2,3,4,5,6]**

```
Initial state:
S F
[1] -> [2] -> [3] -> [4] -> [5] -> [6] -> NULL

Step 1:
       S           F
[1] -> [2] -> [3] -> [4] -> [5] -> [6] -> NULL

Step 2:
              S                    F
[1] -> [2] -> [3] -> [4] -> [5] -> [6] -> NULL

Step 3:
                     S                         F
[1] -> [2] -> [3] -> [4] -> [5] -> [6] -> NULL
Fast is NULL, so we stop
Slow is at position 4 (second middle!)
```

#### Solution

```python
def middleNode(head: ListNode) -> ListNode:
    # Initialize both pointers at head
    slow = head
    fast = head
    
    # Move until fast can't move anymore
    # fast moves 2 steps, slow moves 1 step
    while fast and fast.next:
        slow = slow.next           # 1 step
        fast = fast.next.next      # 2 steps
    
    # When fast reaches end, slow is at middle
    return slow
```

#### Why This Condition: `while fast and fast.next`

Let's understand each part:

**`fast` check:**
- Prevents error when fast reaches NULL (even length list)
- Example: [1,2,3,4]
  - Eventually fast becomes NULL
  - Without this check: `fast.next` would error!

**`fast.next` check:**
- Prevents error when we try to do `fast.next.next` (odd length list)
- Example: [1,2,3,4,5]
  - Eventually fast is at last node (5)
  - fast.next is NULL
  - Without this check: `fast.next.next` would error!

**Combined:**
```python
# This condition handles BOTH odd and even length lists!
while fast and fast.next:
    # Safe to do fast.next.next here
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- We traverse the list once
- Slow pointer visits n/2 nodes
- Fast pointer visits n nodes
- O(n/2) + O(n) = O(n)

**Space Complexity: O(1)**
- Only using two pointers (slow and fast)
- No extra data structures
- Constant space regardless of input size

---

### Problem 2: Linked List Cycle

**[LeetCode 141 - Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)**

#### Problem Statement

Given `head`, the head of a linked list, determine if the linked list has a **cycle** in it.

A cycle exists if you can reach the same node again by continuously following the `next` pointer.

**Example 1:**
```
Input: head = [3,2,0,-4], pos = 1
(pos indicates which node the tail connects to)

Visual:
    3 -> 2 -> 0 -> -4
         ↑          |
         +----------+
         
Output: true (there is a cycle)
```

**Example 2:**
```
Input: head = [1,2], pos = -1

Visual:
1 -> 2 -> NULL

Output: false (no cycle)
```

#### Understanding the Problem

**What is a cycle?**
- The tail node points back to some node in the list
- Instead of pointing to NULL
- This creates an infinite loop

**Why this is tricky:**
- Can't just check if we reach NULL (we never will if there's a cycle!)
- Can't use a counter (cycle could be anywhere)

#### Naive Approach (Works but not optimal)

**Approach 1: Use a Set to track visited nodes**

```python
def hasCycle(head: ListNode) -> bool:
    visited = set()
    current = head
    
    while current:
        if current in visited:
            return True  # We've seen this node before!
        visited.add(current)
        current = current.next
    
    return False  # Reached NULL, no cycle
```

**Why this works:**
- Track every node we visit
- If we visit a node twice = cycle!

**Problem:**
- Space complexity: O(n) - we store all nodes
- Can we do better? Yes!

#### Optimal Approach: Floyd's Cycle Detection (Tortoise and Hare)

**Key insight:**
- If there's a cycle, a faster pointer will eventually "lap" a slower pointer
- Like runners on a circular track!

**Analogy:**
Imagine two runners on a track:
- Slow runner: runs 1 lap per hour
- Fast runner: runs 2 laps per hour
- If track is circular (cycle), fast runner will eventually catch slow runner!

#### Why Fast & Slow Pointers Work for Cycle Detection

**Case 1: No cycle**
```
Fast pointer reaches NULL → No cycle

[1] -> [2] -> [3] -> [4] -> NULL
                          ↑
                         Fast reaches NULL
                         Return false
```

**Case 2: Has cycle**
```
Fast pointer eventually meets slow pointer

Step 1:
S F
[1] -> [2] -> [3] -> [4]
       ↑             |
       +-------------+

Step 2:
       S     F
[1] -> [2] -> [3] -> [4]
       ↑             |
       +-------------+

Step 3:
             S  F
[1] -> [2] -> [3] -> [4]
       ↑             |
       +-------------+

Step 4:
       F,S
[1] -> [2] -> [3] -> [4]
       ↑             |
       +-------------+
       
They meet! Return true
```

#### Mathematical Proof

**Why must they meet?**

Consider the distance between pointers:
- Each iteration: fast closes gap by 1 node
- If gap is k nodes, they'll meet in k iterations

**In a cycle:**
- Fast pointer enters cycle
- Slow pointer eventually enters cycle
- Once both in cycle, distance decreases by 1 each step
- Must eventually become 0 (they meet!)

#### Visual Walkthrough

**Example: [3,2,0,-4], pos=1**

```
    3 -> 2 -> 0 -> -4
         ↑          |
         +----------+

Initial:
S F (both at 3)

Step 1:
Slow at 2, Fast at 0

     3 -> [2] -> [0] -> -4
          S       F    |
          ↑            |
          +------------+

Step 2:
Slow at 0, Fast at 2

     3 -> [2] -> [0] -> -4
               S  ↑    |
          F <----+-----+

Step 3:
Slow at -4, Fast at 0

     3 -> [2] -> [0] -> [-4]
          ↑      F       S
          +---------------+

Step 4:
Slow at 2, Fast at 2

     3 -> [2] -> [0] -> -4
          S,F    ↑      |
          +-------------+

They meet! Return True
```

#### Solution

```python
def hasCycle(head: ListNode) -> bool:
    # Initialize pointers
    slow = head
    fast = head
    
    # Traverse while fast can move
    while fast and fast.next:
        slow = slow.next           # Move 1 step
        fast = fast.next.next      # Move 2 steps
        
        # If they meet, there's a cycle
        if slow == fast:
            return True
    
    # Fast reached end (NULL), no cycle
    return False
```

#### Understanding the Condition

**Why `while fast and fast.next`?**

```python
fast           # Check if fast is not NULL
fast.next      # Check if fast.next is not NULL

Without these checks:
- fast could be NULL → fast.next would error
- fast.next could be NULL → fast.next.next would error
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- If no cycle: Fast reaches end in n/2 iterations = O(n)
- If cycle exists: They meet within n iterations
  - Why? Fast gains 1 node on slow per iteration
  - Maximum gap before meeting = n nodes
  
**Space Complexity: O(1)**
- Only two pointers
- No extra data structures

**Comparison with Set approach:**
```
Set Approach:   Time O(n), Space O(n)
Two Pointers:   Time O(n), Space O(1) ✓ Better!
```

---

### Problem 3: Reverse Linked List

**[LeetCode 206 - Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)**

#### Problem Statement

Given the `head` of a singly linked list, reverse the list, and return the reversed list.

**Example:**
```
Input: [1,2,3,4,5]
Output: [5,4,3,2,1]

Visual:
Before: 1 -> 2 -> 3 -> 4 -> 5 -> NULL
After:  5 -> 4 -> 3 -> 2 -> 1 -> NULL
```

#### Understanding the Problem

**What does "reverse" mean?**
- Change direction of all arrows (next pointers)
- Head becomes tail
- Tail becomes head

**Original:**
```
1 -> 2 -> 3 -> 4 -> 5 -> NULL
```

**Reversed:**
```
NULL <- 1 <- 2 <- 3 <- 4 <- 5
```

#### Why This Is Tricky

**Problem: Losing references!**

If we just do:
```python
current.next = previous  # Reverse the arrow
```

We lose access to the rest of the list!

```
Before: 1 -> 2 -> 3 -> 4 -> 5
         ↑
      current

If we do: current.next = None
Result: 1 -> NULL  (We lost 2,3,4,5!)
         ↑
      current
```

**Solution: Save the next node before reversing!**

#### Approach: Three Pointers

We need three pointers:
1. **prev**: Previous node (what current should point to)
2. **current**: Current node we're processing
3. **next**: Next node (save it before we lose access!)

#### Visual Step-by-Step

**Initial Setup:**
```
prev = NULL, current = head

NULL    1 -> 2 -> 3 -> 4 -> 5 -> NULL
 ↑      ↑
prev  current
```

**Step 1: Save next, reverse pointer, move forward**
```
# Save next
next = current.next

NULL    1 -> 2 -> 3 -> 4 -> 5 -> NULL
 ↑      ↑    ↑
prev  current next

# Reverse pointer
current.next = prev

NULL <- 1    2 -> 3 -> 4 -> 5 -> NULL
        ↑    ↑
     current next

# Move forward
prev = current
current = next

NULL <- 1    2 -> 3 -> 4 -> 5 -> NULL
        ↑    ↑
       prev current
```

**Step 2: Repeat**
```
# Save next
next = current.next

NULL <- 1    2 -> 3 -> 4 -> 5 -> NULL
        ↑    ↑    ↑
       prev curr next

# Reverse pointer
NULL <- 1 <- 2    3 -> 4 -> 5 -> NULL
             ↑    ↑
           curr next

# Move forward
NULL <- 1 <- 2    3 -> 4 -> 5 -> NULL
        ↑    ↑    ↑
            prev curr
```

**Step 3: Continue until current is NULL**
```
NULL <- 1 <- 2 <- 3    4 -> 5 -> NULL
                  ↑    ↑
                 prev curr

NULL <- 1 <- 2 <- 3 <- 4    5 -> NULL
                       ↑    ↑
                      prev curr

NULL <- 1 <- 2 <- 3 <- 4 <- 5    NULL
                            ↑    ↑
                           prev curr

Current is NULL, stop!
Return prev (new head)
```

#### Solution

```python
def reverseList(head: ListNode) -> ListNode:
    # Initialize pointers
    prev = None
    current = head
    
    # Process each node
    while current:
        # Step 1: Save next node
        next_node = current.next
        
        # Step 2: Reverse the pointer
        current.next = prev
        
        # Step 3: Move pointers forward
        prev = current
        current = next_node
    
    # prev is now the new head
    return prev
```

#### Detailed Trace

```python
Input: 1 -> 2 -> 3 -> NULL

Iteration 1:
  prev = None, current = 1
  next_node = 2
  current.next = None     # 1 -> NULL
  prev = 1
  current = 2
  State: None <- 1    2 -> 3 -> NULL

Iteration 2:
  prev = 1, current = 2
  next_node = 3
  current.next = 1        # 2 -> 1
  prev = 2
  current = 3
  State: None <- 1 <- 2    3 -> NULL

Iteration 3:
  prev = 2, current = 3
  next_node = None
  current.next = 2        # 3 -> 2
  prev = 3
  current = None
  State: None <- 1 <- 2 <- 3

current is None, exit loop
Return prev (which is 3)

Final: 3 -> 2 -> 1 -> None
```

#### Common Mistakes

**Mistake 1: Forgetting to save next**
```python
# WRONG
current.next = prev
current = current.next  # Error! current.next is now prev!
```

**Mistake 2: Wrong return value**
```python
# WRONG
return current  # current is None at the end!

# CORRECT
return prev  # prev is the new head
```

**Mistake 3: Not handling empty list**
```python
# The solution handles it automatically!
# If head is None, while loop never executes
# Returns None (correct!)
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Visit each node exactly once
- n iterations for n nodes

**Space Complexity: O(1)**
- Only 3 pointers (prev, current, next)
- No recursion stack
- No extra data structures

#### Recursive Solution (Bonus)

**Why recursive is less preferred:**
- Uses O(n) space for call stack
- Harder to debug
- Risk of stack overflow for large lists

**But here's how it works:**

```python
def reverseListRecursive(head: ListNode) -> ListNode:
    # Base case: empty or single node
    if not head or not head.next:
        return head
    
    # Reverse rest of list
    new_head = reverseListRecursive(head.next)
    
    # Fix current node
    head.next.next = head
    head.next = None
    
    return new_head
```

**How it works:**
```
Original: 1 -> 2 -> 3 -> NULL

Call stack:
reverse(1) calls reverse(2)
  reverse(2) calls reverse(3)
    reverse(3) returns 3 (base case)
  
  In reverse(2):
    new_head = 3
    2.next.next = 2  # 3.next = 2
    2.next = None
    Returns 3
    
  In reverse(1):
    new_head = 3
    1.next.next = 1  # 2.next = 1
    1.next = None
    Returns 3

Final: 3 -> 2 -> 1 -> NULL
```

---

### Problem 4: Remove Linked List Elements

**[LeetCode 203 - Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)**

#### Problem Statement

Given the `head` of a linked list and an integer `val`, remove all nodes with `node.val == val`, and return the new head.

**Example 1:**
```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]

Visual:
Before: 1 -> 2 -> 6 -> 3 -> 4 -> 5 -> 6 -> NULL
After:  1 -> 2 -> 3 -> 4 -> 5 -> NULL
```

**Example 2:**
```
Input: head = [7,7,7,7], val = 7
Output: []
```

#### Understanding the Problem

**What we need to do:**
- Find all nodes with value = val
- Remove them from the list
- Return new head (might have changed!)

**Challenges:**
1. What if head needs to be removed?
2. What if multiple consecutive nodes need removal?
3. What if all nodes need removal?

#### Naive Approach (Many If Statements)

**Problems with this approach:**
```python
# Handle empty list
if not head:
    return None

# Handle head removal
while head and head.val == val:
    head = head.next

# Handle middle nodes
current = head
while current and current.next:
    if current.next.val == val:
        current.next = current.next.next
    else:
        current = current.next

# Too many special cases!
```

**Issues:**
- Complex logic
- Easy to miss edge cases
- Hard to maintain

#### Better Approach: Dummy Head

**Key insight:**
Use a dummy node to make ALL nodes equal (no special case for head!)

**Why dummy head works:**
```
Without dummy:
HEAD
 |
 v
[1] -> [2] -> [3] -> NULL
 ↑ Special case! Can't look at "previous"

With dummy:
DUMMY    HEAD
  |      |
  v      v
[-1] -> [1] -> [2] -> [3] -> NULL
Now head is just like any other node!
```

#### Visual Step-by-Step

**Example: Remove all 6s from [1,2,6,3,4,5,6]**

**Setup:**
```
Create dummy node pointing to head

DUMMY    HEAD
  |      |
  v      v
[-1] -> [1] -> [2] -> [6] -> [3] -> [4] -> [5] -> [6] -> NULL
  ↑
current
```

**Step 1: Check current.next**
```
current.next.val = 1 ≠ 6, move forward

DUMMY    
  |      
  v      
[-1] -> [1] -> [2] -> [6] -> [3] -> [4] -> [5] -> [6] -> NULL
         ↑
      current
```

**Step 2: Check current.next**
```
current.next.val = 2 ≠ 6, move forward

DUMMY    
  |      
  v      
[-1] -> [1] -> [2] -> [6] -> [3] -> [4] -> [5] -> [6] -> NULL
                ↑
             current
```

**Step 3: Check current.next**
```
current.next.val = 6! Skip it!

Before:
[-1] -> [1] -> [2] -> [6] -> [3] -> [4] -> [5] -> [6] -> NULL
                ↑      ↑
             current  Skip this

After:
[-1] -> [1] -> [2] --------> [3] -> [4] -> [5] -> [6] -> NULL
                ↑             
             current (don't move!)
             
Why don't we move current?
Because next node (3) also needs checking!
```

**Step 4-6: Continue...**
```
current.next = 3 ≠ 6, move forward
current.next = 4 ≠ 6, move forward  
current.next = 5 ≠ 6, move forward
```

**Step 7: Found another 6!**
```
current.next.val = 6! Skip it!

[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> [6] -> NULL
                                     ↑      ↑
                                  current  Skip

After:
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] --------> NULL
                                     ↑
                                  current
```

**Step 8: Done!**
```
current.next = None, exit loop
Return dummy.next (the real head)

Final: [1] -> [2] -> [3] -> [4] -> [5] -> NULL
```

#### Solution

```python
def removeElements(head: ListNode, val: int) -> ListNode:
    # Create dummy node
    dummy = ListNode(-1)  # Value doesn't matter
    dummy.next = head
    
    # Start from dummy
    current = dummy
    
    # Check each node's next
    while current.next:
        if current.next.val == val:
            # Skip the node
            current.next = current.next.next
            # Don't move current! Check next node too
        else:
            # Move forward
            current = current.next
    
    # Return new head
    return dummy.next
```

#### Why This Works for All Edge Cases

**Edge Case 1: Remove head**
```
Remove 1 from [1,2,3]

DUMMY
  |
  v
[-1] -> [1] -> [2] -> [3] -> NULL
  ↑      ↑
current  Remove this

current.next = current.next.next

[-1] --------> [2] -> [3] -> NULL
  ↑
dummy

Return dummy.next = 2 ✓
```

**Edge Case 2: Remove all nodes**
```
Remove 1 from [1,1,1]

[-1] -> [1] -> [1] -> [1] -> NULL
  ↑
current

After removing all:
[-1] --------> NULL
  ↑
dummy

Return dummy.next = None ✓
```

**Edge Case 3: Empty list**
```
head = None

[-1] -> NULL
  ↑
dummy

while current.next: (False, skip loop)
Return dummy.next = None ✓
```

#### Common Mistakes

**Mistake 1: Moving current after deletion**
```python
# WRONG
if current.next.val == val:
    current.next = current.next.next
    current = current.next  # Don't do this!

# Why wrong?
# Next node might also need deletion!
Example: [1,6,6,3]
         Remove first 6 but skip second 6
```

**Mistake 2: Not using dummy head**
```python
# WRONG - needs many special cases
if head.val == val:  # Special case
    head = head.next

while current.next:  # Different logic
    ...

# RIGHT - uniform logic with dummy
```

**Mistake 3: Returning head instead of dummy.next**
```python
# WRONG
return head  # Might have been deleted!

# RIGHT
return dummy.next  # Always correct new head
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Visit each node once
- Each operation is O(1)

**Space Complexity: O(1)**
- Only one dummy node
- Only one current pointer
- No extra data structures

---

### Problem 5: Reverse Linked List II

**[LeetCode 92 - Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)**

#### Problem Statement

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

**Example:**
```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]

Visual:
Before: 1 -> 2 -> 3 -> 4 -> 5 -> NULL
              ↑         ↑
            left      right
            
After:  1 -> 4 -> 3 -> 2 -> 5 -> NULL
```

#### Understanding the Problem

**What we're doing:**
1. Keep nodes before `left` unchanged
2. Reverse nodes from `left` to `right`
3. Keep nodes after `right` unchanged

**Challenges:**
- Can't just reverse entire list
- Need to reconnect reversed section properly
- What if left = 1 (reversing from head)?

#### Breaking Down The Problem

**Three distinct phases:**

```
Phase 1: Reach the node before 'left'
Phase 2: Reverse from 'left' to 'right'
Phase 3: Reconnect everything

Example: Reverse from position 2 to 4
[1] -> [2] -> [3] -> [4] -> [5] -> NULL

Phase 1: Get to node before position 2 (node 1)
[1] -> [2] -> [3] -> [4] -> [5] -> NULL
 ↑
left_prev

Phase 2: Reverse [2] -> [3] -> [4]
[1]    [2] <- [3] <- [4]    [5] -> NULL

Phase 3: Reconnect
[1] -> [4] -> [3] -> [2] -> [5] -> NULL
```

#### Why Use Dummy Head?

**Problem: What if left = 1?**

```
Without dummy:
[1] -> [2] -> [3] -> NULL
 ↑
Can't have a "node before 1"!

With dummy:
[-1] -> [1] -> [2] -> [3] -> NULL
  ↑      ↑
 left_prev  left
Now we always have a node before left!
```

#### Phase 1: Reach Position Before Left

**Goal:** Position `left_prev` at node before `left`

```python
# Create dummy
dummy = ListNode(-1)
dummy.next = head

# Position left_prev
left_prev = dummy
current = head

# Move left-1 times
for _ in range(left - 1):
    left_prev = current
    current = current.next
```

**Why `left - 1` iterations?**
```
Example: left = 3

Initial:
DUMMY    HEAD
[-1] -> [1] -> [2] -> [3] -> [4] -> NULL
  ↑      ↑
left_prev curr

Iteration 1:
[-1] -> [1] -> [2] -> [3] -> [4] -> NULL
         ↑      ↑
    left_prev curr

Iteration 2:
[-1] -> [1] -> [2] -> [3] -> [4] -> NULL
                ↑      ↑
           left_prev curr

After 2 iterations (left-1), left_prev is at node before left!
```

#### Phase 2: Reverse The Section

**This is just like reversing a normal linked list!**

But we only reverse `right - left + 1` nodes.

**Why `right - left + 1`?**
```
Example: left=2, right=4
Positions: 2, 3, 4
Count: 4 - 2 + 1 = 3 nodes ✓
```

**Reversing logic:**
```python
prev = None
for _ in range(right - left + 1):
    next_node = current.next    # Save next
    current.next = prev         # Reverse
    prev = current              # Move prev
    current = next_node         # Move current
```

**Visual:**
```
Initial: [2] -> [3] -> [4] -> [5]
          ↑
        current

After 1st iteration:
prev = [2] -> NULL
current = [3] -> [4] -> [5]

After 2nd iteration:
prev = [3] -> [2] -> NULL
current = [4] -> [5]

After 3rd iteration:
prev = [4] -> [3] -> [2] -> NULL
current = [5]

Note: current now points to first node after reversed section!
```

#### Phase 3: Reconnect Everything

**Two connections needed:**

```
Before reconnection:
[1]    [4] -> [3] -> [2] -> NULL    [5] -> NULL
 ↑      ↑                             ↑
left_  prev                        current
prev   (new head of reversed)

Need to:
1. Point left_prev.next to prev (4)
2. Point [2] to current (5)
```

**How to point [2] to [5]?**

The node at position `left` is now at the END of reversed section!
- We saved it as `left_prev.next` in phase 1
- So `left_prev.next.next = current`

```python
# Reconnect
left_prev.next.next = current  # [2] -> [5]
left_prev.next = prev           # [1] -> [4]
```

#### Complete Visual Walkthrough

**Example: Reverse positions 2-4 in [1,2,3,4,5]**

```
PHASE 1: Position pointers

DUMMY
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> NULL
  ↑      ↑
left_  current
prev

After left-1 iterations (1 iteration):
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> NULL
         ↑      ↑
     left_prev current


PHASE 2: Reverse (right-left+1 = 3 iterations)

Iteration 1:
prev = None
current = [2]
next = [3]

current.next = None:  [2] -> NULL
prev = [2]
current = [3]

Iteration 2:
next = [4]

current.next = [2]:  [3] -> [2] -> NULL
prev = [3]
current = [4]

Iteration 3:
next = [5]

current.next = [3]:  [4] -> [3] -> [2] -> NULL
prev = [4]
current = [5]

State now:
[-1] -> [1]    [4] -> [3] -> [2] -> NULL    [5] -> NULL
         ↑      ↑                             ↑
     left_prev prev                        current


PHASE 3: Reconnect

left_prev.next.next = current:
[-1] -> [1]    [4] -> [3] -> [2] --------> [5] -> NULL
         ↑      ↑                             
     left_prev prev                        

left_prev.next = prev:
[-1] -> [1] -> [4] -> [3] -> [2] -> [5] -> NULL
         ↑      ↑                             
     left_prev prev

Final: [1] -> [4] -> [3] -> [2] -> [5] -> NULL
```

#### Complete Solution

```python
def reverseBetween(head: ListNode, left: int, right: int) -> ListNode:
    # Create dummy to handle edge case of left=1
    dummy = ListNode(-1)
    dummy.next = head
    
    # PHASE 1: Position left_prev at node before 'left'
    left_prev = dummy
    current = head
    
    for _ in range(left - 1):
        left_prev = current
        current = current.next
    
    # PHASE 2: Reverse from 'left' to 'right'
    prev = None
    for _ in range(right - left + 1):
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    # PHASE 3: Reconnect
    # left_prev.next is the node at original 'left' position
    # It's now at the end of reversed section
    left_prev.next.next = current  # Connect to node after 'right'
    left_prev.next = prev           # Connect to new head of reversed section
    
    return dummy.next
```

#### Common Mistakes

**Mistake 1: Wrong number of iterations**
```python
# WRONG
for _ in range(left):  # Too many!

# RIGHT
for _ in range(left - 1):  # Positions left_prev correctly
```

**Mistake 2: Forgetting dummy head**
```python
# WRONG - fails when left=1
left_prev = head  # Can't get node before head!

# RIGHT
dummy = ListNode(-1)
dummy.next = head
left_prev = dummy
```

**Mistake 3: Wrong reconnection order**
```python
# WRONG
left_prev.next = prev           # Do this first...
left_prev.next.next = current   # ...then left_prev.next has changed!

# RIGHT
left_prev.next.next = current   # Use old left_prev.next first
left_prev.next = prev            # Then update it
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Phase 1: O(left) to position pointers
- Phase 2: O(right-left) to reverse
- Phase 3: O(1) to reconnect
- Total: O(n)

**Space Complexity: O(1)**
- Only pointers (dummy, left_prev, prev, current, next)
- No recursion
- No extra data structures

  ### Problem 6: Palindrome Linked List

**[LeetCode 234 - Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)**

#### Problem Statement

Given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.

**Example 1:**
```
Input: [1,2,2,1]
Output: true

Visual:
1 -> 2 -> 2 -> 1 -> NULL
Reads same forwards and backwards!
```

**Example 2:**
```
Input: [1,2]
Output: false
```

#### Understanding the Problem

**What is a palindrome?**
A sequence that reads the same forwards and backwards.
- "racecar" is a palindrome
- "hello" is not

**Examples:**
```
[1,2,3,2,1] → palindrome ✓
[1,2,3,4,5] → not palindrome ✗
[1] → palindrome ✓ (single element)
[] → palindrome ✓ (empty)
```

#### Naive Approach (Works but not optimal)

**Approach 1: Copy to array**

```python
def isPalindrome(head: ListNode) -> bool:
    # Copy all values to array
    values = []
    current = head
    while current:
        values.append(current.val)
        current = current.next
    
    # Check if array is palindrome
    return values == values[::-1]
```

**Why this works:**
- Arrays are easy to reverse and compare
- Simple and clear logic

**Problems:**
- Space: O(n) - storing all values
- Can we do better? Yes!

**Follow-up question asks: Can you solve it in O(1) space?**

#### Optimal Approach: Three Steps

**Key insight:**
1. Find middle of list (fast & slow pointers)
2. Reverse second half
3. Compare first half with reversed second half

**Why this works:**
```
Original: 1 -> 2 -> 3 -> 2 -> 1

Step 1 - Find middle:
1 -> 2 -> 3 -> 2 -> 1
          ↑
        middle

Step 2 - Reverse second half:
First:  1 -> 2 -> 3
Second: 1 -> 2 <- 3

Step 3 - Compare:
1 == 1 ✓
2 == 2 ✓
3 (middle can be ignored in odd length)
Result: Palindrome!
```

#### Step 1: Find The Middle

**Using fast & slow pointers (we've done this before!)**

```python
slow = head
fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

**For odd length:**
```
[1,2,3,2,1]

Initial:
S F
[1] -> [2] -> [3] -> [2] -> [1] -> NULL

Step 1:
       S           F
[1] -> [2] -> [3] -> [2] -> [1] -> NULL

Step 2:
              S                    F
[1] -> [2] -> [3] -> [2] -> [1] -> NULL

Slow at middle (3)
```

**For even length:**
```
[1,2,2,1]

Initial:
S F
[1] -> [2] -> [2] -> [1] -> NULL

Step 1:
       S           F
[1] -> [2] -> [2] -> [1] -> NULL

Step 2:
              S                F
[1] -> [2] -> [2] -> [1] -> NULL

Slow at start of second half
```

#### Step 2: Reverse Second Half

**This is just reversing a linked list!**

```python
prev = None
current = slow

while current:
    next_node = current.next
    current.next = prev
    prev = current
    current = next_node
```

**Visual:**
```
Original second half:
[2] -> [1] -> NULL

After reversal:
NULL <- [2] <- [1]
               ↑
              prev
```

#### Step 3: Compare Both Halves

**Walk through both halves simultaneously:**

```python
left = head
right = prev  # prev points to new head of reversed half

while right:  # right half might be shorter
    if left.val != right.val:
        return False
    left = left.next
    right = right.next

return True
```

**Why check `while right`?**
```
Odd length: [1,2,3,2,1]
First half:  1 -> 2 -> 3
Second half: 1 -> 2

We only need to compare 1,2 (ignore middle 3)
Right half is shorter, so check while right exists!
```

#### Complete Visual Walkthrough

**Example: [1,2,2,1]**

```
STEP 1: Find middle

Initial:
S F
[1] -> [2] -> [2] -> [1] -> NULL

After iterations:
              S
[1] -> [2] -> [2] -> [1] -> NULL
                     F (NULL)


STEP 2: Reverse second half

Before:
[1] -> [2] -> [2] -> [1] -> NULL
              ↑
            slow

Reverse from slow:
[1] -> [2]    [2] -> [1] -> NULL
              ↑
           reverse this part

After reversal:
[1] -> [2]    NULL <- [2] <- [1]
                               ↑
                             prev


STEP 3: Compare

left = [1]
right = [1]

Compare 1 == 1 ✓

left = [2]
right = [2]

Compare 2 == 2 ✓

right = NULL, stop

Return True (palindrome!)
```

**Example: [1,2,3]**

```
STEP 1: Find middle

              S
[1] -> [2] -> [3] -> NULL
                     F


STEP 2: Reverse second half

Before:
[1] -> [2] -> [3] -> NULL

After:
[1] -> [2]    NULL <- [3]
                      ↑
                     prev


STEP 3: Compare

left = [1]
right = [3]

Compare 1 != 3 ✗

Return False
```

#### Complete Solution

```python
def isPalindrome(head: ListNode) -> bool:
    # Edge case: empty or single node
    if not head or not head.next:
        return True
    
    # STEP 1: Find middle using fast & slow pointers
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # STEP 2: Reverse second half
    prev = None
    current = slow
    
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    # STEP 3: Compare both halves
    left = head
    right = prev  # Head of reversed second half
    
    while right:  # Right half might be shorter
        if left.val != right.val:
            return False
        left = left.next
        right = right.next
    
    return True
```

#### Common Mistakes

**Mistake 1: Not handling odd length correctly**
```python
# WRONG - comparing middle with itself
while left and right:
    if left.val != right.val:
        return False
    ...

# For [1,2,3,2,1], middle (3) shouldn't be compared

# RIGHT
while right:  # Right is shorter in odd length
    ...
```

**Mistake 2: Forgetting edge cases**
```python
# WRONG - doesn't handle empty or single node
def isPalindrome(head):
    slow = head
    fast = head
    # If head is None, this errors!

# RIGHT
if not head or not head.next:
    return True
```

**Mistake 3: Comparing wrong halves**
```python
# WRONG
left = slow  # Should start from head!
right = prev

# RIGHT  
left = head  # Start from beginning
right = prev
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Finding middle: O(n/2)
- Reversing second half: O(n/2)
- Comparing: O(n/2)
- Total: O(n)

**Space Complexity: O(1)**
- Only pointers (slow, fast, prev, current, left, right)
- No extra data structures
- Modifies list in place

**Comparison:**
```
Array approach:   Time O(n), Space O(n)
Two pointers:     Time O(n), Space O(1) ✓ Better!
```

---

### Problem 7: Merge Two Sorted Lists

**[LeetCode 21 - Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)**

#### Problem Statement

You are given the heads of two sorted linked lists `list1` and `list2`. Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

**Example:**
```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]

Visual:
List1: 1 -> 2 -> 4
List2: 1 -> 3 -> 4

Merged: 1 -> 1 -> 2 -> 3 -> 4 -> 4
```

#### Understanding the Problem

**Key observations:**
1. Both lists are **already sorted**
2. We need to **merge** them (not concatenate)
3. Order must be maintained
4. If values are equal, doesn't matter which comes first

**What "splice together" means:**
We're not creating new nodes - we're reusing existing nodes and changing their pointers!

```
Before:
List1: [1] -> [2] -> [4]
List2: [1] -> [3] -> [4]

After:
[1] -> [1] -> [2] -> [3] -> [4] -> [4]
 ↑      ↑      ↑      ↑      ↑      ↑
L2     L1     L1     L2     L1     L2
```

#### Approach: Two Pointers

**Similar to merging sorted arrays!**

**Core idea:**
1. Compare current nodes of both lists
2. Pick the smaller one
3. Add it to result
4. Move pointer forward in that list
5. Repeat

**Why use dummy head?**
```
Without dummy:
Need special logic to set first node as head

With dummy:
[-1] -> ...
Build freely, return dummy.next at end!
```

#### Visual Step-by-Step

**Example: Merge [1,2,4] and [1,3,4]**

```
SETUP: Create dummy and current pointer

DUMMY
[-1] -> NULL
 ↑
current

List1: 1 -> 2 -> 4 -> NULL
       ↑
      L1

List2: 1 -> 3 -> 4 -> NULL
       ↑
      L2


STEP 1: Compare L1 (1) and L2 (1)

They're equal, so we take L2 (else block)

DUMMY
[-1] -> [1] -> NULL
        ↑ (from L2)

Update pointers:
current = [1]
L2 = [3]

List1: 1 -> 2 -> 4 -> NULL
       ↑
      L1

List2: 1 -> 3 -> 4 -> NULL
            ↑
           L2


STEP 2: Compare L1 (1) and L2 (3)

L1 < L2, so take L1

DUMMY
[-1] -> [1] -> [1] -> NULL
        (L2)   (L1)

Update:
current = second [1]
L1 = [2]

List1: 1 -> 2 -> 4 -> NULL
            ↑
           L1

List2: 1 -> 3 -> 4 -> NULL
            ↑
           L2


STEP 3: Compare L1 (2) and L2 (3)

L1 < L2, take L1

DUMMY
[-1] -> [1] -> [1] -> [2] -> NULL
        (L2)   (L1)   (L1)

Update:
current = [2]
L1 = [4]


STEP 4: Compare L1 (4) and L2 (3)

L2 < L1, take L2

DUMMY
[-1] -> [1] -> [1] -> [2] -> [3] -> NULL
        (L2)   (L1)   (L1)   (L2)

Update:
current = [3]
L2 = [4]


STEP 5: Compare L1 (4) and L2 (4)

Equal, take L2

DUMMY
[-1] -> [1] -> [1] -> [2] -> [3] -> [4] -> NULL
                                     (L2)

Update:
current = [4]
L2 = NULL


STEP 6: L2 is NULL, exit while loop

Append remaining L1:
current.next = L1

DUMMY
[-1] -> [1] -> [1] -> [2] -> [3] -> [4] -> [4] -> NULL
                                     (L2)   (L1)

Return dummy.next = first [1]
```

#### Why We Can Append Remaining List

**Key insight: Lists are already sorted!**

```
If we have:
Merged: [1,2,3]
L1: [5,7,9]
L2: NULL

Since L1 is sorted and all elements so far ≤ 3,
all remaining elements in L1 must be ≥ 3!

So we can safely append entire L1:
[1,2,3] -> [5,7,9]
```

#### Complete Solution

```python
def mergeTwoLists(list1: ListNode, list2: ListNode) -> ListNode:
    # Create dummy head
    dummy = ListNode(-1)
    current = dummy
    
    # Compare and merge while both lists exist
    while list1 and list2:
        if list1.val < list2.val:
            current.next = list1
            list1 = list1.next
        else:  # list2.val <= list1.val
            current.next = list2
            list2 = list2.next
        
        current = current.next
    
    # Append remaining nodes (one list is empty)
    if list1:
        current.next = list1
    else:
        current.next = list2
    
    return dummy.next
```

#### Alternative: More Explicit

```python
def mergeTwoLists(list1: ListNode, list2: ListNode) -> ListNode:
    dummy = ListNode(-1)
    current = dummy
    
    while list1 and list2:
        if list1.val < list2.val:
            current.next = list1
            list1 = list1.next
        else:
            current.next = list2
            list2 = list2.next
        
        current = current.next
    
    # Append whichever list still has nodes
    current.next = list1 if list1 else list2
    
    return dummy.next
```

#### Common Mistakes

**Mistake 1: Creating new nodes**
```python
# WRONG - Creates new nodes (wastes space)
if list1.val < list2.val:
    current.next = ListNode(list1.val)

# RIGHT - Reuses existing nodes
current.next = list1
```

**Mistake 2: Forgetting to move current**
```python
# WRONG
if list1.val < list2.val:
    current.next = list1
    list1 = list1.next
# Current didn't move! Next iteration overwrites!

# RIGHT
if list1.val < list2.val:
    current.next = list1
    list1 = list1.next
current = current.next  # Move current!
```

**Mistake 3: Not handling remaining nodes**
```python
# WRONG - Might miss nodes at end
while list1 and list2:
    # merge logic
return dummy.next  # Missing nodes if one list longer!

# RIGHT
while list1 and list2:
    # merge logic
current.next = list1 if list1 else list2  # Append rest
return dummy.next
```

**Mistake 4: Returning dummy instead of dummy.next**
```python
# WRONG
return dummy  # Returns the -1 node!

# RIGHT
return dummy.next  # Returns first real node
```

#### Time & Space Complexity

**Time Complexity: O(n + m)**
- n = length of list1
- m = length of list2
- We visit each node exactly once

**Space Complexity: O(1)**
- Only dummy and current pointers
- Reusing existing nodes
- Not creating any new nodes

---

## Additional Important Problems

### Problem 8: Linked List Cycle II

**[LeetCode 142 - Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)**

#### Problem Statement

Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

**Example:**
```
Input: head = [3,2,0,-4], pos = 1

Visual:
    3 -> 2 -> 0 -> -4
         ↑          |
         +----------+
         
Output: Node with value 2 (where cycle starts)
```

#### Understanding the Problem

**Difference from Cycle Detection:**
- Before: Just detect IF there's a cycle
- Now: Find WHERE the cycle begins

**This is harder!**

#### Approach: Floyd's Algorithm Extended

**Two phases:**
1. Detect if cycle exists (fast & slow meet)
2. Find cycle start

**Mathematical proof:**

```
Let:
- L = distance from head to cycle start
- C = cycle length
- K = distance from cycle start to meeting point

When slow and fast meet:
- Slow traveled: L + K
- Fast traveled: L + K + nC (n complete cycles)
- Fast = 2 × Slow

Therefore:
L + K + nC = 2(L + K)
L + K + nC = 2L + 2K
nC = L + K
L = nC - K

This means: Distance from head to cycle start (L)
equals distance from meeting point to cycle start (nC - K)
```

**So the algorithm:**
1. Find meeting point (phase 1)
2. Start one pointer from head, one from meeting point
3. Move both one step at a time
4. They'll meet at cycle start!

#### Visual Walkthrough

**Example: Find cycle start**

```
    3 -> 2 -> 0 -> -4
         ↑          |
         +----------+

PHASE 1: Detect cycle and find meeting point

Initial:
S F
[3] -> [2] -> [0] -> [-4]
       ↑              |
       +--------------+

After several steps, they meet at:
        S,F
[3] -> [2] -> [0] -> [-4]
       ↑              |
       +--------------+
Let's say they meet at node 0


PHASE 2: Find cycle start

Place two pointers:
ptr1 at head
ptr2 at meeting point

ptr1
[3] -> [2] -> [0] -> [-4]
               ↑ptr2  |
       +---------------+

Move both one step:
       ptr1
[3] -> [2] -> [0] -> [-4]
       ↑      ptr2   |
       +--------------+

They meet at node 2!
This is the cycle start!
```

#### Solution

```python
def detectCycle(head: ListNode) -> ListNode:
    # Phase 1: Detect cycle
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:  # Cycle detected
            # Phase 2: Find cycle start
            ptr1 = head
            ptr2 = slow  # Or fast, same position
            
            while ptr1 != ptr2:
                ptr1 = ptr1.next
                ptr2 = ptr2.next
            
            return ptr1  # Cycle start
    
    # No cycle
    return None
```

#### Common Mistakes

**Mistake 1: Returning meeting point**
```python
# WRONG
if slow == fast:
    return slow  # This is NOT cycle start!

# RIGHT
if slow == fast:
    # Need phase 2 to find actual start
    ptr1 = head
    ptr2 = slow
    while ptr1 != ptr2:
        ptr1 = ptr1.next
        ptr2 = ptr2.next
    return ptr1
```

**Mistake 2: Moving pointers at different speeds in phase 2**
```python
# WRONG
ptr1 = ptr1.next
ptr2 = ptr2.next.next  # Wrong speed!

# RIGHT
ptr1 = ptr1.next
ptr2 = ptr2.next  # Both move one step!
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Phase 1: O(n) to detect cycle
- Phase 2: O(n) to find start
- Total: O(n)

**Space Complexity: O(1)**
- Only pointers used

---

### Problem 9: Intersection of Two Linked Lists

**[LeetCode 160 - Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)**

#### Problem Statement

Given the heads of two singly linked lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection, return `null`.

**Example:**
```
Input:
ListA: 4 -> 1 \
              8 -> 4 -> 5
ListB: 5 -> 6 -> 1 /

Output: Node with value 8 (intersection point)

Visual:
A:     4 -> 1 \
                8 -> 4 -> 5
B: 5 -> 6 -> 1 /
```

#### Understanding the Problem

**What is intersection?**
- Two lists share common nodes at the end
- NOT just same values - literally same node objects!
- After intersection, all remaining nodes are shared

**Key observations:**
```
If lists intersect:
A: a1 -> a2 -> c1 -> c2 -> c3
B: b1 -> b2 -> b3 -> c1 -> c2 -> c3
              ↑
         intersection

After c1, both lists are identical (same nodes)
```

#### Naive Approach (Works but not optimal)

**Approach 1: Use a set**

```python
def getIntersectionNode(headA, headB):
    seen = set()
    
    # Store all nodes from A
    current = headA
    while current:
        seen.add(current)
        current = current.next
    
    # Check nodes from B
    current = headB
    while current:
        if current in seen:
            return current  # First intersection
        current = current.next
    
    return None
```

**Problems:**
- Space: O(n) for the set
- Can we do O(1) space?

#### Optimal Approach: Two Pointers

**Key insight:**
If we traverse both lists, switching to the other list when we reach the end, we'll meet at the intersection!

**Why this works:**

```
ListA length: a + c (a unique, c common)
ListB length: b + c (b unique, c common)

Pointer A path: a + c + b + c
Pointer B path: b + c + a + c

After (a+c+b) and (b+c+a) steps, they're both at:
- Intersection if it exists
- null if no intersection

They traveled same total distance!
```

#### Visual Walkthrough

**Example: Find intersection**

```
ListA: 4 -> 1 -> 8 -> 4 -> 5
ListB: 5 -> 6 -> 1 -> 8 -> 4 -> 5
                     ↑
               intersection

INITIAL:
ptrA -> 4
ptrB -> 5

STEP 1:
ptrA -> 1
ptrB -> 6

STEP 2:
ptrA -> 8
ptrB -> 1

STEP 3:
ptrA -> 4
ptrB -> 8

STEP 4:
ptrA -> 5
ptrB -> 4

STEP 5:
ptrA -> NULL (end of A, jump to B)
ptrA -> 5 (now at headB)
ptrB -> 5

STEP 6:
ptrA -> 6
ptrB -> NULL (end of B, jump to A)
ptrB -> 4 (now at headA)

CONTINUE...
Eventually both will be at node 8 (intersection)!
```

**Detailed trace:**

```
A: 4 -> 1 -> 8 -> 4 -> 5
B: 5 -> 6 -> 1 -> 8 -> 4 -> 5

ptrA path: 4, 1, 8, 4, 5, NULL, [switch to B], 5, 6, 1, 8 ✓
ptrB path: 5, 6, 1, 8, 4, 5, NULL, [switch to A], 4, 1, 8 ✓

Both reach node 8 at same time!
```

**If no intersection:**

```
A: 1 -> 2 -> 3
B: 4 -> 5

ptrA path: 1, 2, 3, NULL, [switch], 4, 5, NULL
ptrB path: 4, 5, NULL, [switch], 1, 2, 3, NULL

Both reach NULL at same time!
```

#### Solution

```python
def getIntersectionNode(headA: ListNode, headB: ListNode) -> ListNode:
    if not headA or not headB:
        return None
    
    # Initialize two pointers
    ptrA = headA
    ptrB = headB
    
    # Traverse both lists
    while ptrA != ptrB:
        # Move to next, or switch to other list
        ptrA = ptrA.next if ptrA else headB
        ptrB = ptrB.next if ptrB else headA
    
    # Either intersection node or None
    return ptrA
```

#### Understanding the Logic

**Why `ptrA = ptrA.next if ptrA else headB`?**

```python
if ptrA:
    ptrA = ptrA.next  # Continue in current list
else:
    ptrA = headB  # Reached end, switch to other list
```

**Why does the loop terminate?**

```
Case 1: Intersection exists
- Both pointers meet at intersection node
- Loop exits

Case 2: No intersection
- Both pointers meet at None
- Loop exits

Both cases guaranteed to terminate!
```

#### Common Mistakes

**Mistake 1: Checking values instead of nodes**
```python
# WRONG
if ptrA.val == ptrB.val:  # Values could match without intersection!
    return ptrA

# RIGHT
if ptrA == ptrB:  # Check actual node objects
    return ptrA
```

**Mistake 2: Not switching lists**
```python
# WRONG
while ptrA != ptrB:
    ptrA = ptrA.next  # Just goes to None and stops
    ptrB = ptrB.next

# RIGHT
ptrA = ptrA.next if ptrA else headB  # Switch when None
```

**Mistake 3: Infinite loop concern**
```python
# Question: Can this loop forever?
while ptrA != ptrB:
    ptrA = ptrA.next if ptrA else headB
    ptrB = ptrB.next if ptrB else headA

# Answer: NO!
# After at most 2 full traversals, they meet
# Either at intersection or at None
```

#### Time & Space Complexity

**Time Complexity: O(n + m)**
- n = length of list A
- m = length of list B
- Each pointer traverses at most both lists once

**Space Complexity: O(1)**
- Only two pointers
- No extra data structures

---

### Problem 10: Remove Nth Node From End of List

**[LeetCode 19 - Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)**

#### Problem Statement

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example:**
```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

Visual:
Before: 1 -> 2 -> 3 -> 4 -> 5
                      ↑
                 2nd from end

After:  1 -> 2 -> 3 -> 5
```

#### Understanding the Problem

**"Nth from the end" means:**
```
[1,2,3,4,5], n=1: Remove 5 (1st from end)
[1,2,3,4,5], n=2: Remove 4 (2nd from end)
[1,2,3,4,5], n=5: Remove 1 (5th from end = head!)
```

**Challenge:**
- Singly linked list → can only move forward
- How to find nth from end without counting length first?

#### Naive Approach

**Approach 1: Two passes**

```python
def removeNthFromEnd(head: ListNode, n: int) -> ListNode:
    # Pass 1: Count length
    length = 0
    current = head
    while current:
        length += 1
        current = current.next
    
    # Pass 2: Remove (length - n)th from start
    target = length - n
    
    # Handle removing head
    if target == 0:
        return head.next
    
    current = head
    for _ in range(target - 1):
        current = current.next
    
    current.next = current.next.next
    return head
```

**Problems:**
- Two passes through list
- Can we do it in one pass?

#### Optimal Approach: Two Pointers with Gap

**Key insight:**
Maintain two pointers with a gap of `n` nodes between them!

**Why this works:**
```
If gap is n:
When right pointer reaches end,
left pointer is at (n+1)th from end

Example: n=2
[1] -> [2] -> [3] -> [4] -> [5] -> NULL
 ↑                    ↑
left (gap=2)        right

When right moves to NULL:
[1] -> [2] -> [3] -> [4] -> [5] -> NULL
              ↑                    ↑
            left                 right
            
left is now at node BEFORE target!
```

#### Visual Step-by-Step

**Example: Remove 2nd from end in [1,2,3,4,5]**

```
SETUP: Create dummy, create gap of n

DUMMY
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> NULL
  ↑      ↑
 left  right


STEP 1: Move right n times (2 times)

DUMMY
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> NULL
  ↑                    ↑
 left                right

Gap of 2 created!


STEP 2: Move both until right reaches NULL

Iteration 1:
DUMMY
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> NULL
         ↑                    ↑
        left                right

Iteration 2:
DUMMY
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> NULL
↑                    ↑
               left                right

Iteration 3:
DUMMY
[-1] -> [1] -> [2] -> [3] -> [4] -> [5] -> NULL
                       ↑                    ↑
                      left                right

right.next is NULL, stop!


STEP 3: Remove node

left is at 3, which is BEFORE target (4)

left.next = left.next.next

DUMMY
[-1] -> [1] -> [2] -> [3] --------> [5] -> NULL
                       ↑
                      left

Return dummy.next = [1]
```

#### Why Use Dummy Head?

**Problem: What if we remove head?**

```
Remove 5th from end (the head!) in [1,2,3,4,5]

Without dummy:
left starts at head
Need special case when target is head

With dummy:
left starts before head
Uniform logic for all cases!
```

#### Complete Solution

```python
def removeNthFromEnd(head: ListNode, n: int) -> ListNode:
    # Create dummy to handle removing head
    dummy = ListNode(-1)
    dummy.next = head
    
    # Initialize two pointers
    left = dummy
    right = head
    
    # Create gap of n nodes
    for _ in range(n):
        right = right.next
    
    # Move both until right reaches end
    while right:
        left = left.next
        right = right.next
    
    # Remove the target node
    left.next = left.next.next
    
    return dummy.next
```

#### Detailed Trace

**Example: [1,2,3,4,5], n=2**

```
Initial:
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
left     right

Create gap (move right 2 times):
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
left               right

Move both until right is NULL:

Step 1:
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
         left          right

Step 2:
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
                  left     right

Step 3:
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
                       left     right

right is NULL, stop

Remove target:
left.next = left.next.next

dummy -> 1 -> 2 -> 3 -> 5 -> NULL

Return dummy.next = 1
```

#### Common Mistakes

**Mistake 1: Not using dummy head**
```python
# WRONG - fails when removing head
left = head
# Can't access node before head!

# RIGHT
dummy = ListNode(-1)
dummy.next = head
left = dummy
```

**Mistake 2: Wrong gap size**
```python
# WRONG
for _ in range(n - 1):  # Gap too small!
    right = right.next

# RIGHT
for _ in range(n):  # Correct gap
    right = right.next
```

**Mistake 3: Moving pointers at different speeds**
```python
# WRONG
while right:
    left = left.next
    right = right.next.next  # Right moves too fast!

# RIGHT
while right:
    left = left.next
    right = right.next  # Both move same speed
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Create gap: O(n)
- Move to target: O(n)
- Total: One pass through list

**Space Complexity: O(1)**
- Only dummy and two pointers
- No extra data structures

---

## Linked List Concepts Summary

### Key Patterns

**1. Fast & Slow Pointers (Runner Technique)**
```
Use cases:
- Find middle
- Detect cycle
- Find kth from end

Pattern:
slow = head
fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

**2. Dummy Head**
```
Use cases:
- Removing nodes (especially head)
- Merging lists
- Building new lists

Pattern:
dummy = ListNode(-1)
dummy.next = head
current = dummy
# ... operations ...
return dummy.next
```

**3. Reversing**
```
Use cases:
- Reverse entire list
- Reverse part of list
- Check palindrome

Pattern:
prev = None
current = head
while current:
    next = current.next
    current.next = prev
    prev = current
    current = next
```

**4. Two Pointers with Gap**
```
Use cases:
- Find nth from end
- Remove nth from end

Pattern:
left = dummy
right = head
for _ in range(n):
    right = right.next
while right:
    left = left.next
    right = right.next
```

### Common Mistakes to Avoid

1. **Losing references**
```python
# WRONG
current.next = new_node  # Lost rest of list!

# RIGHT
next = current.next  # Save reference
current.next = new_node
new_node.next = next
```

2. **Not handling null pointers**
```python
# WRONG
while current:
    current = current.next.next  # Error if next is None!

# RIGHT
while current and current.next:
    current = current.next.next
```

3. **Forgetting to move current**
```python
# WRONG - infinite loop
while current:
    # do something
    # forgot current = current.next!

# RIGHT
while current:
    # do something
    current = current.next
```

4. **Comparing values instead of nodes**
```python
# WRONG
if node1.val == node2.val:  # Different nodes!

# RIGHT
if node1 == node2:  # Same node object
```

### Time Complexity Summary

| Operation | Time | Space |
|-----------|------|-------|
| Traversal | O(n) | O(1) |
| Insert at head | O(1) | O(1) |
| Insert at tail | O(n) | O(1) |
| Delete node | O(n) | O(1) |
| Reverse list | O(n) | O(1) |
| Find middle | O(n) | O(1) |
| Detect cycle | O(n) | O(1) |
| Merge sorted | O(n+m) | O(1) |

---

# Stacks Section

## What is a Stack?

A **stack** is a linear data structure that follows the **LIFO** (Last In, First Out) principle.

### Real-World Analogies

**1. Stack of plates:**
```
    [Plate 3]  ← Last in, First out
    [Plate 2]
    [Plate 1]
    [  Table  ]

You can only:
- Add plate on top (push)
- Remove plate from top (pop)
```

**2. Browser history:**
```
Page 3 (current) ← Back button goes here
Page 2
Page 1 (homepage)

Pressing "back" removes current page (pop)
Visiting new page adds to top (push)
```

**3. Undo functionality:**
```
Action 3 (most recent) ← Undo removes this
Action 2
Action 1

Each undo removes the last action
```

### Stack Operations

**Basic operations:**
```python
push(item)    # Add item to top - O(1)
pop()         # Remove top item - O(1)
peek()/top()  # View top item - O(1)
isEmpty()     # Check if empty - O(1)
size()        # Get number of items - O(1)
```

### Stack Implementation

**Using Python list:**
```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.isEmpty():
            return self.items.pop()
        return None
    
    def peek(self):
        if not self.isEmpty():
            return self.items[-1]
        return None
    
    def isEmpty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

**Visual:**
```
Operations:
push(1): [1]
push(2): [1, 2]
push(3): [1, 2, 3]
         ↑
        top

pop():   [1, 2] → returns 3
peek():  [1, 2] → returns 2 (doesn't remove)
```

---

### Problem 11: Min Stack

**[LeetCode 155 - Min Stack](https://leetcode.com/problems/min-stack/)**

#### Problem Statement

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object
- `void push(int val)` pushes element val onto the stack
- `void pop()` removes the element on top of the stack
- `int top()` gets the top element of the stack
- `int getMin()` retrieves the minimum element in the stack

All operations must be **O(1)** time complexity!

**Example:**
```python
minStack = MinStack()
minStack.push(-2)
minStack.push(0)
minStack.push(-3)
minStack.getMin()   # Returns -3
minStack.pop()
minStack.top()      # Returns 0
minStack.getMin()   # Returns -2
```

#### Understanding the Problem

**The challenge:**
Regular stack operations are O(1), but finding minimum is O(n) if we search entire stack each time.

**Naive approach:**
```python
def getMin(self):
    return min(self.stack)  # O(n) - Not allowed!
```

**What we need:**
- Track minimum at all times
- Update minimum when push/pop
- All operations stay O(1)

#### Key Insight

**Store the minimum WITH each element!**

When we push a value, also store what the minimum is at that point.

```
Stack visualization:
[(val, min_at_this_point)]

Example:
push(-2): [(-2, -2)]
push(0):  [(-2, -2), (0, -2)]
push(-3): [(-2, -2), (0, -2), (-3, -3)]
                                    ↑
                              current min
```

#### Approach 1: Stack of Tuples

**Store (value, current_min) pairs**

```python
class MinStack:
    def __init__(self):
        self.stack = []
    
    def push(self, val: int) -> None:
        # If stack is empty, val is the min
        if not self.stack:
            self.stack.append((val, val))
        else:
            # Min is smaller of val and previous min
            current_min = min(val, self.stack[-1][1])
            self.stack.append((val, current_min))
    
    def pop(self) -> None:
        self.stack.pop()
    
    def top(self) -> int:
        return self.stack[-1][0]  # Return value, not min
    
    def getMin(self) -> int:
        return self.stack[-1][1]  # Return current min
```

#### Visual Walkthrough

```
Operation: push(-2)

Stack: [(-2, -2)]
        val  min

Operation: push(0)

Current min = min(0, -2) = -2
Stack: [(-2, -2), (0, -2)]

Operation: push(-3)

Current min = min(-3, -2) = -3
Stack: [(-2, -2), (0, -2), (-3, -3)]

getMin() → -3 ✓

Operation: pop()

Stack: [(-2, -2), (0, -2)]

getMin() → -2 ✓ (previous min restored!)
```

#### Why This Works

**Key observation:**
When we pop, the previous minimum is automatically restored!

```
Before pop:
[(-2, -2), (0, -2), (-3, -3)]
                         ↑
                    min is -3

After pop:
[(-2, -2), (0, -2)]
                ↑
            min is -2 (automatically!)
```

#### Approach 2: Two Stacks

**Use separate stack for minimums**

```python
class MinStack:
    def __init__(self):
        self.stack = []      # Regular stack
        self.min_stack = []  # Stack of minimums
    
    def push(self, val: int) -> None:
        self.stack.append(val)
        
        # Push to min_stack if it's new minimum
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
    
    def pop(self) -> None:
        val = self.stack.pop()
        
        # If we're removing current min, pop from min_stack
        if val == self.min_stack[-1]:
            self.min_stack.pop()
    
    def top(self) -> int:
        return self.stack[-1]
    
    def getMin(self) -> int:
        return self.min_stack[-1]
```

#### Visual Comparison

**Approach 1: Tuples**
```
push(-2): [(-2,-2)]
push(0):  [(-2,-2), (0,-2)]
push(-3): [(-2,-2), (0,-2), (-3,-3)]

Space: O(2n) = O(n)
```

**Approach 2: Two Stacks**
```
push(-2):
stack:     [-2]
min_stack: [-2]

push(0):
stack:     [-2, 0]
min_stack: [-2]     ← 0 not pushed (not new min)

push(-3):
stack:     [-2, 0, -3]
min_stack: [-2, -3]

Space: Better! min_stack only stores actual minimums
```

#### Common Mistakes

**Mistake 1: Using < instead of <= in approach 2**
```python
# WRONG
if val < self.min_stack[-1]:  # Miss duplicate mins!
    self.min_stack.append(val)

Example:
push(1): min_stack = [1]
push(1): min_stack = [1]  ← Should push again!
pop():   min_stack = []   ← Oops! Lost minimum!

# RIGHT
if val <= self.min_stack[-1]:  # Handle duplicates
    self.min_stack.append(val)
```

**Mistake 2: Not checking equality in pop (approach 2)**
```python
# WRONG
def pop(self):
    self.stack.pop()
    self.min_stack.pop()  # Always pop min_stack!

# RIGHT
def pop(self):
    val = self.stack.pop()
    if val == self.min_stack[-1]:  # Only if it's current min
        self.min_stack.pop()
```

**Mistake 3: Returning tuple in top() (approach 1)**
```python
# WRONG
def top(self):
    return self.stack[-1]  # Returns (val, min) tuple!

# RIGHT
def top(self):
    return self.stack[-1][0]  # Return only value
```

#### Time & Space Complexity

**Both approaches:**

**Time Complexity: O(1)** for all operations
- push: O(1)
- pop: O(1)
- top: O(1)
- getMin: O(1)

**Space Complexity:**
- Approach 1: O(2n) = O(n) - store value and min for each element
- Approach 2: O(n + k) where k is number of minimums - often better!
