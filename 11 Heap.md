# Complete Heaps Guide for Beginners 🌳

## Table of Contents
1. [Introduction to Heaps](#introduction-to-heaps)
2. [Heap Fundamentals](#heap-fundamentals)
3. [Heap Operations](#heap-operations)
4. [Python Implementation](#python-implementation)
5. [LeetCode Problems](#leetcode-problems)
6. [Additional Important Concepts](#additional-important-concepts)

---

## Introduction to Heaps

### What is a Heap?

A **heap** is a special type of data structure that helps us quickly find the smallest (or largest) element in a collection. Think of it like a tournament bracket where the winner (smallest or largest value) is always at the top.

**Real-world analogy:** Imagine a hospital emergency room where patients are treated based on severity (priority), not arrival time. A heap helps manage this priority queue efficiently.

### Why Do We Need Heaps?

**Problem:** You have a list of numbers and frequently need to:
- Find the minimum/maximum value
- Remove the minimum/maximum value
- Add new values

**Without Heap:**
```python
# Finding minimum in unsorted list - O(n) every time!
numbers = [5, 3, 8, 1, 9, 2]
minimum = min(numbers)  # Must check all elements

# Maintaining sorted list - O(n) for every insertion!
numbers.append(4)
numbers.sort()  # Re-sort entire list
```

**With Heap:**
```python
import heapq
heap = [5, 3, 8, 1, 9, 2]
heapq.heapify(heap)  # O(n) once
minimum = heap[0]  # O(1) - instant access!
heapq.heappush(heap, 4)  # O(log n) - much faster!
```

---

## Heap Fundamentals

### 1. Complete Binary Tree

Before understanding heaps, you need to understand **complete binary trees**.

#### What is a Binary Tree?

A **binary tree** is a tree where each node has **at most 2 children** (left and right).

```
       5
      / \
     3   8
    / \
   1   2
```

#### What is a COMPLETE Binary Tree?

A **complete binary tree** fills levels from **left to right**, and all levels are completely filled except possibly the last level.

**✅ COMPLETE Binary Tree:**
```
       5
      / \
     3   8
    / \
   1   2
```
*All levels filled from left to right*

**❌ NOT COMPLETE Binary Tree:**
```
       5
      / \
     3   8
        /
       1
```
*Last level has gap on the left!*

**Why does this matter?** Complete binary trees can be efficiently stored in arrays without wasting space.

### 2. Heap Property

A heap must satisfy the **heap property** in addition to being a complete binary tree.

#### Min-Heap Property

**Rule:** Every parent node is **smaller than or equal to** its children.

```
       1  ← Root (smallest)
      / \
     3   2
    / \
   5   4
```

**Key insight:** The smallest element is ALWAYS at the root (top)!

```python
# Example: Building a min-heap
import heapq

numbers = [5, 3, 8, 1, 9, 2]
heapq.heapify(numbers)  # Converts to min-heap

print(numbers)  # [1, 3, 2, 5, 9, 8]
print(numbers[0])  # 1 - Always the minimum!
```

#### Max-Heap Property

**Rule:** Every parent node is **greater than or equal to** its children.

```
       9  ← Root (largest)
      / \
     5   8
    / \
   1   3
```

**Important:** Python's `heapq` module only implements **min-heaps** by default!

**Trick to create Max-Heap:** Negate all values!

```python
import heapq

# Want max-heap of [5, 3, 8, 1, 9, 2]
numbers = [5, 3, 8, 1, 9, 2]

# Negate all values for max-heap
max_heap = [-x for x in numbers]
heapq.heapify(max_heap)

# Get maximum (remember to negate back!)
maximum = -max_heap[0]  # -(-9) = 9
print(maximum)  # 9
```

### 3. Array Representation

**Key Concept:** Heaps are stored as arrays, but represent tree structures!

#### Parent-Child Relationship

For any element at index `i`:
- **Left child** is at index: `2*i + 1`
- **Right child** is at index: `2*i + 2`
- **Parent** is at index: `(i-1) // 2`

**Visual Example:**
```
Array: [1, 3, 2, 5, 4, 8, 6]
Index:  0  1  2  3  4  5  6

Tree representation:
       1 (index 0)
      / \
     3   2 (indices 1, 2)
    / \ / \
   5  4 8  6 (indices 3,4,5,6)
```

**Let's verify the relationships:**

```python
heap = [1, 3, 2, 5, 4, 8, 6]

# Element at index 1 (value 3)
i = 1
left_child = 2*i + 1  # Index 3 → value 5
right_child = 2*i + 2  # Index 4 → value 4
parent = (i-1) // 2    # Index 0 → value 1

print(f"Node: {heap[i]}")  # 3
print(f"Left child: {heap[left_child]}")   # 5
print(f"Right child: {heap[right_child]}")  # 4
print(f"Parent: {heap[parent]}")  # 1
```

---

## Heap Operations

### 1. Heapify - O(n)

**What it does:** Converts an unordered array into a valid heap.

**Why O(n) and not O(n log n)?** This is a common interview question!

**Intuitive explanation:**
- Start from the bottom of the tree
- Most nodes are near the bottom and require few operations
- Only a few nodes at the top require many operations
- Mathematical analysis shows this averages to O(n)

```python
import heapq

# Unsorted array
arr = [5, 3, 8, 1, 9, 2, 7]

# Convert to min-heap in O(n) time
heapq.heapify(arr)

print(arr)  # [1, 3, 2, 5, 9, 8, 7]
# Now arr[0] is always the minimum!
```

**Visual Process:**
```
Before heapify: [5, 3, 8, 1, 9, 2, 7]

       5
      / \
     3   8
    / \ / \
   1  9 2  7

After heapify: [1, 3, 2, 5, 9, 8, 7]

       1  ← Smallest at root!
      / \
     3   2
    / \ / \
   5  9 8  7
```

### 2. Insert (Push) - O(log n)

**What it does:** Adds a new element while maintaining heap property.

**Algorithm (Bubble Up / Sift Up):**
1. Add element at the end of array
2. Compare with parent
3. If violates heap property, swap with parent
4. Repeat until heap property satisfied

**Why O(log n)?** Maximum swaps = height of tree = log n

```python
import heapq

heap = [1, 3, 2, 5, 9, 8, 7]

# Insert 0 into the heap
heapq.heappush(heap, 0)

print(heap)  # [0, 1, 2, 3, 9, 8, 7, 5]
```

**Visual Process:**
```
Step 1: Add 0 at end
       1
      / \
     3   2
    / \ / \
   5  9 8  7
  /
 0

Step 2: Compare 0 with parent (5) - Swap!
       1
      / \
     3   2
    / \ / \
   0  9 8  7
  /
 5

Step 3: Compare 0 with parent (3) - Swap!
       1
      / \
     0   2
    / \ / \
   3  9 8  7
  /
 5

Step 4: Compare 0 with parent (1) - Swap!
       0  ← New minimum!
      / \
     1   2
    / \ / \
   3  9 8  7
  /
 5
```

### 3. Extract Min/Max (Pop) - O(log n)

**What it does:** Removes and returns the root (min/max element).

**Algorithm (Bubble Down / Sift Down):**
1. Remove root element
2. Move last element to root
3. Compare with children
4. Swap with smaller child (min-heap)
5. Repeat until heap property satisfied

```python
import heapq

heap = [1, 3, 2, 5, 9, 8, 7]

# Remove minimum element
min_val = heapq.heappop(heap)

print(min_val)  # 1
print(heap)     # [2, 3, 7, 5, 9, 8]
```

**Visual Process:**
```
Step 1: Remove root (1)
       ?
      / \
     3   2
    / \ / 
   5  9 8  7

Step 2: Move last element (7) to root
       7
      / \
     3   2
    / \ / 
   5  9 8

Step 3: Compare 7 with children (3, 2)
        Swap with 2 (smaller child)
       2
      / \
     3   7
    / \ / 
   5  9 8

Step 4: Compare 7 with child (8)
        7 < 8, STOP
       2  ← New minimum!
      / \
     3   7
    / \ / 
   5  9 8
```

### 4. Peek - O(1)

**What it does:** Returns the root element without removing it.

```python
import heapq

heap = [1, 3, 2, 5, 9, 8, 7]

# Peek at minimum
minimum = heap[0]
print(minimum)  # 1

# Heap unchanged
print(heap)  # [1, 3, 2, 5, 9, 8, 7]
```

---

## Python Implementation

### Using heapq Module

Python's `heapq` module provides a min-heap implementation.

```python
import heapq

# 1. Create an empty heap
heap = []

# 2. Add elements
heapq.heappush(heap, 5)
heapq.heappush(heap, 3)
heapq.heappush(heap, 8)
heapq.heappush(heap, 1)

print(heap)  # [1, 3, 8, 5]

# 3. Remove minimum
min_val = heapq.heappop(heap)
print(min_val)  # 1
print(heap)     # [3, 5, 8]

# 4. Convert existing list to heap
numbers = [5, 3, 8, 1, 9, 2]
heapq.heapify(numbers)
print(numbers)  # [1, 3, 2, 5, 9, 8]

# 5. Get n largest/smallest elements
numbers = [5, 3, 8, 1, 9, 2, 7, 4]
largest_3 = heapq.nlargest(3, numbers)
smallest_3 = heapq.nsmallest(3, numbers)

print(largest_3)   # [9, 8, 7]
print(smallest_3)  # [1, 2, 3]
```

### Max-Heap Implementation Trick

Since Python only has min-heap, use negation for max-heap:

```python
import heapq

# Original values
values = [5, 3, 8, 1, 9, 2]

# Create max-heap by negating
max_heap = [-x for x in values]
heapq.heapify(max_heap)

print(max_heap)  # [-9, -5, -8, -1, -3, -2]

# Get maximum (negate back!)
maximum = -max_heap[0]
print(maximum)  # 9

# Remove maximum
max_val = -heapq.heappop(max_heap)
print(max_val)  # 9
print([-x for x in max_heap])  # [8, 5, 2, 1, 3]
```

### Advanced heapq Functions

```python
import heapq

heap = [1, 3, 2, 5, 9, 8, 7]

# 1. heappushpop: Push then pop (more efficient than separate operations)
result = heapq.heappushpop(heap, 4)
print(result)  # 1 (old minimum)
print(heap[0])  # 2 (new minimum)

# 2. heapreplace: Pop then push
heap = [1, 3, 2, 5, 9, 8, 7]
result = heapq.heapreplace(heap, 0)
print(result)  # 1 (old minimum)
print(heap[0])  # 0 (new minimum)

# 3. Merge multiple sorted iterables
merged = heapq.merge([1, 3, 5], [2, 4, 6], [0, 7, 8])
print(list(merged))  # [0, 1, 2, 3, 4, 5, 6, 7, 8]
```

---

## LeetCode Problems

### Problem 1: Kth Largest Element in an Array

**🔗 LeetCode Link:** [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

**Difficulty:** Medium

#### Problem Statement

Given an integer array `nums` and an integer `k`, return the `kth` largest element in the array.

Note that it is the `kth` largest element in sorted order, not the `kth` distinct element.

**Example 1:**
```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

**Example 2:**
```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

#### Approach 1: Brute Force (Sorting)

**Thought process:** The simplest solution is to sort the array and return the kth element from the end.

**Why doesn't this work optimally?** Sorting takes O(n log n) time, but we only need the kth largest element, not the entire sorted array.

```python
def findKthLargest(nums, k):
    """
    Time: O(n log n) - sorting
    Space: O(1) - in-place sort (Python's sort is in-place)
    """
    nums.sort()
    return nums[-k]  # kth from end

# Test
nums = [3,2,1,5,6,4]
k = 2
print(findKthLargest(nums, k))  # 5
```

**Why is this not the best solution?**
- We're doing unnecessary work (sorting entire array)
- Time complexity is O(n log n)
- Interview expects better solution

#### Approach 2: Min-Heap of Size K (Optimal)

**💡 Key Insight:** We only need to track the K largest elements, not all elements!

**Thought process:**
1. Use a min-heap of size K
2. The root of this heap will be the Kth largest element
3. When heap size > K, remove the smallest element

**Why min-heap and not max-heap?**
- In a min-heap of K elements containing the K largest values
- The root (minimum of these K elements) is the Kth largest overall!

**Visual Example:**
```
nums = [3, 2, 1, 5, 6, 4], k = 2

Step 1: Add 3
Heap: [3]
Size: 1 < k, keep going

Step 2: Add 2
Heap: [2, 3]
Size: 2 = k, heap is full

Step 3: Add 1
Compare: 1 < 2 (root), don't add
Heap: [2, 3]

Step 4: Add 5
Compare: 5 > 2 (root), replace!
Heap before: [2, 3]
Remove 2, add 5
Heap after: [3, 5]

Step 5: Add 6
Compare: 6 > 3 (root), replace!
Heap before: [3, 5]
Remove 3, add 6
Heap after: [5, 6]

Step 6: Add 4
Compare: 4 < 5 (root), don't add
Heap: [5, 6]

Answer: heap[0] = 5 (2nd largest)
```

**Solution 1: Using heapq.nlargest() (Most Concise)**

```python
import heapq

def findKthLargest(nums, k):
    """
    Time: O(n log k) - heapq.nlargest internally uses a heap
    Space: O(k) - heap of size k
    
    This is the most Pythonic solution!
    """
    # nlargest returns list of k largest elements in descending order
    return heapq.nlargest(k, nums)[-1]

# Test
nums = [3,2,1,5,6,4]
k = 2
print(findKthLargest(nums, k))  # 5

# Let's see what nlargest returns
print(heapq.nlargest(2, nums))  # [6, 5]
print(heapq.nlargest(3, nums))  # [6, 5, 4]
# Last element is kth largest!
```

**Solution 2: Manual Min-Heap (Shows Understanding)**

```python
import heapq

def findKthLargest(nums, k):
    """
    Time: O(n log k)
    - Heapify first k elements: O(k)
    - Process remaining (n-k) elements: O((n-k) * log k)
    - Overall: O(k + (n-k)log k) ≈ O(n log k) when n >> k
    
    Space: O(k) - heap of size k
    """
    # Step 1: Create min-heap with first k elements
    heap = nums[:k]
    heapq.heapify(heap)  # O(k) time
    
    # Step 2: Process remaining elements
    for num in nums[k:]:  # O(n-k) iterations
        if num > heap[0]:  # If larger than kth largest so far
            heapq.heapreplace(heap, num)  # O(log k)
    
    # Step 3: Root is kth largest
    return heap[0]

# Test
nums = [3,2,1,5,6,4]
k = 2
print(findKthLargest(nums, k))  # 5
```

**Let's trace through the algorithm:**

```python
# Detailed trace
nums = [3, 2, 1, 5, 6, 4]
k = 2

# Step 1: heap = [3, 2], heapify → [2, 3]
print("After heapify:", [2, 3])

# Step 2: Process remaining elements
# num = 1: 1 > 2? No, skip
print("After 1:", [2, 3])

# num = 5: 5 > 2? Yes, replace!
# Remove 2, add 5 → [3, 5]
print("After 5:", [3, 5])

# num = 6: 6 > 3? Yes, replace!
# Remove 3, add 6 → [5, 6]
print("After 6:", [5, 6])

# num = 4: 4 > 5? No, skip
print("After 4:", [5, 6])

# Answer: 5
```

**Solution 3: Using heappushpop (Alternative)**

```python
import heapq

def findKthLargest(nums, k):
    """
    Time: O(n log k)
    Space: O(k)
    
    Uses heappushpop which is slightly more efficient
    than separate push and pop operations
    """
    heap = []
    
    for num in nums:
        if len(heap) < k:
            heapq.heappush(heap, num)
        else:
            # heappushpop: pushes num, then pops smallest
            # More efficient than: push then pop
            heapq.heappushpop(heap, num)
    
    return heap[0]

# Test
nums = [3,2,1,5,6,4]
k = 2
print(findKthLargest(nums, k))  # 5
```

#### Approach 3: Quick Select (Advanced)

**Time:** O(n) average, O(n²) worst case  
**Space:** O(1)

This is beyond the scope of beginners but worth mentioning for completeness.

```python
import random

def findKthLargest(nums, k):
    """
    Quick Select algorithm - similar to QuickSort
    Average O(n), but O(n²) worst case
    """
    def quickSelect(left, right, k_smallest):
        if left == right:
            return nums[left]
        
        # Random pivot for average O(n)
        pivot_index = random.randint(left, right)
        
        # Partition around pivot
        pivot_index = partition(left, right, pivot_index)
        
        if k_smallest == pivot_index:
            return nums[k_smallest]
        elif k_smallest < pivot_index:
            return quickSelect(left, pivot_index - 1, k_smallest)
        else:
            return quickSelect(pivot_index + 1, right, k_smallest)
    
    def partition(left, right, pivot_index):
        pivot = nums[pivot_index]
        # Move pivot to end
        nums[pivot_index], nums[right] = nums[right], nums[pivot_index]
        
        store_index = left
        for i in range(left, right):
            if nums[i] < pivot:
                nums[store_index], nums[i] = nums[i], nums[store_index]
                store_index += 1
        
        # Move pivot to final position
        nums[right], nums[store_index] = nums[store_index], nums[right]
        return store_index
    
    # kth largest = (n-k)th smallest (0-indexed)
    return quickSelect(0, len(nums) - 1, len(nums) - k)
```

#### Complexity Comparison

| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| Sorting | O(n log n) | O(1) | Simple, small arrays |
| Min-Heap | O(n log k) | O(k) | **Best for interviews**, k << n |
| Quick Select | O(n) avg | O(1) | Optimal time, but complex |

#### Common Mistakes

```python
# ❌ MISTAKE 1: Using max-heap
def findKthLargest_WRONG(nums, k):
    # Wrong! Need MIN-heap of K elements, not max-heap
    max_heap = [-x for x in nums]
    heapq.heapify(max_heap)
    
    for _ in range(k):
        result = -heapq.heappop(max_heap)
    
    return result  # This works but is O(n + k log n), worse than O(n log k)

# ❌ MISTAKE 2: Forgetting to maintain heap size
def findKthLargest_WRONG(nums, k):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)  # Heap grows to size n!
        # Missing: if len(heap) > k: heappop()
    return heap[0]  # Wrong! This returns minimum, not kth largest

# ✅ CORRECT
def findKthLargest_CORRECT(nums, k):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)  # Maintain size k
    return heap[0]
```

---

### Problem 2: K Closest Points to Origin

**🔗 LeetCode Link:** [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

**Difficulty:** Medium

#### Problem Statement

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points is the **Euclidean distance**: `√((x1-x2)² + (y1-y2)²)`

**Example:**
```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation: 
Distance from (1,3) to origin: √(1²+3²) = √10 ≈ 3.16
Distance from (-2,2) to origin: √((-2)²+2²) = √8 ≈ 2.83
(-2,2) is closer!
```

#### Understanding Distance Formula

**Euclidean Distance from Origin:**
```
Distance = √(x² + y²)
```

**Why can we skip the square root?** For comparison purposes!

```python
# Both give same ordering
distance1 = (1**2 + 3**2)**0.5  # √10 ≈ 3.16
distance2 = (2**2 + 2**2)**0.5  # √8 ≈ 2.83

# Comparing: 3.16 > 2.83

# Without square root (faster!)
dist1_squared = 1**2 + 3**2  # 10
dist2_squared = 2**2 + 2**2  # 8

# Comparing: 10 > 8 (same result!)
```

**💡 Key Insight:** Since `√a > √b` if and only if `a > b` (for positive numbers), we can compare squared distances!

#### Approach 1: Sorting (Brute Force)

**Thought process:** Calculate all distances, sort, return first k.

```python
def kClosest(points, k):
    """
    Time: O(n log n) - sorting
    Space: O(n) - storing distances
    """
    # Calculate distances and store with points
    distances = []
    for x, y in points:
        dist = x**2 + y**2  # Squared distance (skip sqrt)
        distances.append((dist, [x, y]))
    
    # Sort by distance
    distances.sort()
    
    # Return first k points
    return [point for dist, point in distances[:k]]

# Test
points = [[1,3],[-2,2],[2,-2]]
k = 2
print(kClosest(points, k))  # [[-2,2], [2,-2]]
```

**Why not optimal?** Sorting all n points when we only need k!

#### Approach 2: Max-Heap of Size K (Optimal)

**💡 Key Insight:** Similar to Kth Largest Element, but we use **max-heap** this time!

**Why max-heap?**
- We want k CLOSEST (smallest distances)
- Max-heap keeps track of the LARGEST distance among our k closest
- If new point is closer, remove the farthest point

**Visual Example:**
```
points = [[1,3], [-2,2], [2,-2]], k = 2

Distances:
[1,3]: 1² + 3² = 10
[-2,2]: 4 + 4 = 8
[2,-2]: 4 + 4 = 8

Step 1: Add [1,3] (dist=10)
Max-heap: [(10, [1,3])]

Step 2: Add [-2,2] (dist=8)
Max-heap: [(10, [1,3]), (8, [-2,2])]
Size = k = 2, heap full!

Step 3: Check [2,-2] (dist=8)
Compare: 8 < 10 (max in heap)
Remove (10, [1,3]), add (8, [2,-2])
Max-heap: [(8, [-2,2]), (8, [2,-2])]

Result: [[-2,2], [2,-2]]
```

**Solution:**

```python
import heapq

def kClosest(points, k):
    """
    Time: O(n log k)
    Space: O(k)
    
    Uses max-heap to maintain k closest points
    """
    max_heap = []
    
    for x, y in points:
        dist = -(x**2 + y**2)  # Negative for max-heap!
        
        if len(max_heap) < k:
            # Heap not full, just add
            heapq.heappush(max_heap, (dist, [x, y]))
        else:
            # Heap full, check if closer than farthest
            # heappushpop: push then pop largest
            heapq.heappushpop(max_heap, (dist, [x, y]))
    
    # Extract points (ignore distances)
    return [point for dist, point in max_heap]

# Test
points = [[1,3],[-2,2],[2,-2]]
k = 2
print(kClosest(points, k))  # [[-2,2], [2,-2]]
```

**Detailed trace:**

```python
# Let's trace step by step
points = [[1,3], [-2,2], [2,-2]]
k = 2

# Process [1,3]
# dist = -(1²+3²) = -10
# max_heap = [(-10, [1,3])]

# Process [-2,2]
# dist = -(4+4) = -8
# max_heap = [(-10, [1,3]), (-8, [-2,2])]
# Heap full! (size = k)

# Process [2,-2]
# dist = -(4+4) = -8
# heappushpop:
#   1. Push (-8, [2,-2])
#   2. Pop largest: (-8, something)
# max_heap = [(-10, [1,3]), (-8, [2,-2])]
# Actually becomes: [(-8, [-2,2]), (-8, [2,-2])]
```

**Why heappushpop instead of separate operations?**

```python
# ❌ Less efficient (2 operations)
if len(max_heap) >= k:
    heapq.heappush(max_heap, (dist, point))  # O(log k)
    heapq.heappop(max_heap)                   # O(log k)
    # Total: 2 * O(log k)

# ✅ More efficient (1 optimized operation)
heapq.heappushpop(max_heap, (dist, point))  # O(log k)
# Combines operations more efficiently
```

#### Alternative: Using heapq.nsmallest

```python
import heapq

def kClosest(points, k):
    """
    Time: O(n log k)
    Space: O(k)
    
    Most Pythonic solution!
    """
    # nsmallest can use custom key function
    return heapq.nsmallest(k, points, key=lambda p: p[0]**2 + p[1]**2)

# Test
points = [[1,3],[-2,2],[2,-2]]
k = 2
print(kClosest(points, k))  # [[-2,2], [2,-2]]
```

#### Common Mistakes

```python
# ❌ MISTAKE 1: Forgetting to negate for max-heap
def kClosest_WRONG(points, k):
    heap = []
    for x, y in points:
        dist = x**2 + y**2  # Missing negation!
        heapq.heappush(heap, (dist, [x, y]))
        if len(heap) > k:
            heapq.heappop(heap)
    # This gives FARTHEST points, not closest!
    return [point for dist, point in heap]

# ❌ MISTAKE 2: Including square root (unnecessary computation)
def kClosest_SLOW(points, k):
    heap = []
    for x, y in points:
        dist = (x**2 + y**2)**0.5  # Slower! sqrt unnecessary
        # ... rest of code

# ✅ CORRECT
def kClosest_CORRECT(points, k):
    heap = []
    for x, y in points:
        dist = -(x**2 + y**2)  # Negative, no sqrt
        if len(heap) < k:
            heapq.heappush(heap, (dist, [x, y]))
        else:
            heapq.heappushpop(heap, (dist, [x, y]))
    return [point for dist, point in heap]
```

---

### Problem 3: Top K Frequent Elements

**🔗 LeetCode Link:** [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

**Difficulty:** Medium

#### Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

**Example:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
Explanation: 1 appears 3 times, 2 appears 2 times, 3 appears 1 time
```

#### Understanding Frequency Counting

**What is frequency?** How many times an element appears.

```python
nums = [1, 1, 1, 2, 2, 3]

# Manual counting
freq = {}
for num in nums:
    if num in freq:
        freq[num] += 1
    else:
        freq[num] = 1

print(freq)  # {1: 3, 2: 2, 3: 1}
```

**Using Counter (Better):**

```python
from collections import Counter

nums = [1, 1, 1, 2, 2, 3]
freq = Counter(nums)

print(freq)  # Counter({1: 3, 2: 2, 3: 1})
print(freq[1])  # 3
print(freq.most_common(2))  # [(1, 3), (2, 2)]
```

**What is Counter?**
- Special dictionary that counts elements
- Part of Python's `collections` module
- Provides helpful methods like `most_common()`

#### Approach 1: Counter + Sorting

**Thought process:** Count frequencies, sort by frequency, return top k.

```python
from collections import Counter

def topKFrequent(nums, k):
    """
    Time: O(n log n) - sorting by frequency
    Space: O(n) - counter dictionary
    """
    # Count frequencies
    freq = Counter(nums)
    
    # Sort by frequency (descending)
    sorted_items = sorted(freq.items(), key=lambda x: x[1], reverse=True)
    
    # Return first k elements
    return [num for num, count in sorted_items[:k]]

# Test
nums = [1,1,1,2,2,3]
k = 2
print(topKFrequent(nums, k))  # [1, 2]
```

**Understanding lambda:**
```python
# lambda x: x[1] means "use second element for sorting"
items = [(1, 3), (2, 2), (3, 1)]

# Sort by second element (frequency)
sorted_items = sorted(items, key=lambda x: x[1], reverse=True)
print(sorted_items)  # [(1, 3), (2, 2), (3, 1)]
```

**Why not optimal?** Sorting takes O(n log n), but we only need top k!

#### Approach 2: Min-Heap of Size K (Optimal)

**💡 Key Insight:** Maintain min-heap of k elements with highest frequencies!

**Why min-heap?**
- Root has minimum frequency among top k
- If new element has higher frequency, remove root

**Visual Example:**
```
nums = [1,1,1,2,2,3,4,4,4,4], k = 2

Frequencies: {1: 3, 2: 2, 3: 1, 4: 4}

Step 1: Add (1, freq=3)
Heap: [(3, 1)]

Step 2: Add (2, freq=2)
Heap: [(2, 2), (3, 1)]
Size = k, heap full!

Step 3: Check (3, freq=1)
1 < 2 (min in heap), skip

Step 4: Check (4, freq=4)
4 > 2 (min in heap), replace!
Remove (2, 2), add (4, 4)
Heap: [(3, 1), (4, 4)]

Result: [1, 4]
```

**Solution:**

```python
from collections import Counter
import heapq

def topKFrequent(nums, k):
    """
    Time: O(n log k)
    - Counter: O(n)
    - Heap operations: O(n log k)
    
    Space: O(n)
    - Counter: O(n)
    - Heap: O(k)
    """
    # Step 1: Count frequencies - O(n)
    freq = Counter(nums)
    
    # Step 2: Build min-heap of size k
    heap = []
    
    for num, count in freq.items():
        if len(heap) < k:
            # Heap not full, just add
            heapq.heappush(heap, (count, num))
        else:
            # Heap full, check if more frequent
            if count > heap[0][0]:  # Compare with min frequency
                heapq.heapreplace(heap, (count, num))
    
    # Step 3: Extract numbers (ignore frequencies)
    return [num for count, num in heap]

# Test
nums = [1,1,1,2,2,3]
k = 2
print(topKFrequent(nums, k))  # [1, 2] (order may vary)
```

**Detailed trace:**

```python
nums = [1,1,1,2,2,3]
k = 2

# Step 1: Count
freq = {1: 3, 2: 2, 3: 1}

# Step 2: Process frequencies
# Process (1, 3):
#   heap = [(3, 1)]

# Process (2, 2):
#   heap = [(2, 2), (3, 1)]
#   Size = k, heap full!

# Process (3, 1):
#   1 < 2 (root), skip
#   heap = [(2, 2), (3, 1)]

# Step 3: Extract
# Result: [2, 1]
```

#### Approach 3: Using nlargest (Most Concise)

```python
from collections import Counter
import heapq

def topKFrequent(nums, k):
    """
    Time: O(n log k)
    Space: O(n)
    
    Most Pythonic solution!
    """
    freq = Counter(nums)
    
    # nlargest with custom key
    return heapq.nlargest(k, freq.keys(), key=freq.get)

# Test
nums = [1,1,1,2,2,3]
k = 2
print(topKFrequent(nums, k))  # [1, 2]
```

**Understanding this solution:**
```python
freq = {1: 3, 2: 2, 3: 1}

# freq.keys() = [1, 2, 3]
# freq.get(1) = 3
# freq.get(2) = 2
# freq.get(3) = 1

# nlargest finds k elements with largest freq.get(key)
# So it finds elements with highest frequencies!
```

#### Approach 4: Bucket Sort (O(n) time!)

**Advanced technique** - achieves O(n) time complexity!

**Idea:** Create buckets for each frequency (0 to n).

```python
from collections import Counter

def topKFrequent(nums, k):
    """
    Time: O(n)
    Space: O(n)
    
    Most optimal time complexity!
    """
    # Step 1: Count frequencies
    freq = Counter(nums)
    
    # Step 2: Create buckets
    # bucket[i] = list of numbers with frequency i
    bucket = [[] for _ in range(len(nums) + 1)]
    
    for num, count in freq.items():
        bucket[count].append(num)
    
    # Step 3: Collect k most frequent
    result = []
    for i in range(len(bucket) - 1, -1, -1):  # Start from highest frequency
        for num in bucket[i]:
            result.append(num)
            if len(result) == k:
                return result
    
    return result

# Test
nums = [1,1,1,2,2,3]
k = 2
print(topKFrequent(nums, k))  # [1, 2]
```

**Visual Example:**
```
nums = [1,1,1,2,2,3], k = 2
freq = {1: 3, 2: 2, 3: 1}

Buckets (index = frequency):
bucket[0] = []
bucket[1] = [3]       ← frequency 1
bucket[2] = [2]       ← frequency 2
bucket[3] = [1]       ← frequency 3

Iterate from high to low frequency:
- bucket[3]: add 1 → result = [1]
- bucket[2]: add 2 → result = [1, 2] ← size = k, done!

Answer: [1, 2]
```

#### Complexity Comparison

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Sorting | O(n log n) | O(n) | Simple but slow |
| Min-Heap | O(n log k) | O(n) | **Best for interview** |
| nlargest | O(n log k) | O(n) | Most Pythonic |
| Bucket Sort | O(n) | O(n) | Optimal but complex |

---

### Problem 4: Task Scheduler

**🔗 LeetCode Link:** [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/)

**Difficulty:** Medium

#### Problem Statement

Given a characters array `tasks`, representing tasks a CPU needs to do, where each letter represents a different task, and an integer `n` representing the cooldown period between two same tasks.

Return the least number of intervals needed to complete all tasks.

**Example 1:**
```
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: 
A -> B -> idle -> A -> B -> idle -> A -> B
Intervals: 1  2    3     4  5    6     7  8
```

**Example 2:**
```
Input: tasks = ["A","A","A","B","B","B"], n = 0
Output: 6
Explanation: No cooldown needed
A -> A -> A -> B -> B -> B
```

#### Understanding the Problem

**Key concepts:**
1. **Cooldown (n):** Minimum intervals between same tasks
2. **Idle time:** When CPU waits (counts as interval!)
3. **Goal:** Minimize total intervals

**Visual Example:**
```
tasks = ["A","A","A","B","B","B"], n = 2

❌ BAD schedule (random order):
A -> A -> (must wait 2) -> B -> A -> (problem!)
Can't do A yet! Just did A 2 intervals ago.

✅ GOOD schedule (most frequent first):
A -> B -> idle -> A -> B -> idle -> A -> B
     ↑              ↑              ↑
After A, need 2 intervals before next A

Total intervals: 8
```

#### Why Process Most Frequent First?

**💡 Key Insight:** Most frequent tasks determine schedule length!

**Example:**
```
tasks = ["A","A","A","B","C"], n = 2

If we do B, C first:
B -> C -> idle -> A -> idle -> idle -> A -> idle -> idle -> A
Many wasted idle intervals!

If we do A (most frequent) first:
A -> B -> C -> A -> (any) -> (any) -> A
Fewer idle intervals!
```

#### Approach: Max-Heap + Queue

**Strategy:**
1. Use max-heap to always process most frequent task
2. Use queue to track cooldown times
3. Add task back to heap when cooldown expires

**Data structures needed:**
```python
# Max-heap: stores task frequencies (most frequent at root)
heap = [-3, -3, -2]  # 3 A's, 3 B's, 2 C's (negated)

# Queue: stores (remaining_count, available_time)
queue = [(2, 3), (1, 5)]  # (2 A's left, available at time 3)
```

**Visual Walkthrough:**
```
tasks = ["A","A","A","B","B","B"], n = 2

Initial state:
- freq = {A: 3, B: 3}
- heap = [-3, -3]  (negated for max-heap)
- time = 0
- queue = []

Time 1:
- Pop -3 (A) from heap → process A
- A has 2 left, available at time 1+2 = 3
- queue = [(-2, 3)]  (2 A's, available at time 3)
- time = 1

Time 2:
- Pop -3 (B) from heap → process B
- B has 2 left, available at time 2+2 = 4
- queue = [(-2, 3), (-2, 4)]
- time = 2

Time 3:
- Heap empty! Must wait (idle time)
- Check queue: (-2, 3) available now!
- Add -2 back to heap
- queue = [(-2, 4)]
- time = 3

Time 4:
- Pop -2 (A) from heap → process A
- A has 1 left, available at time 4+2 = 6
- queue = [(-2, 4), (-1, 6)]
- time = 4

Time 5:
- Heap empty! Check queue
- (-2, 4) available now!
- Add -2 to heap
- Pop -2 (B) from heap → process B
- queue = [(-1, 6), (-1, 7)]
- time = 5

Time 6:
- Heap empty! Idle time
- Check queue: (-1, 6) available!
- Add -1 to heap
- time = 6

Time 7:
- Pop -1 (A) from heap → process A
- A done (count = 0), don't add to queue
- Check queue: (-1, 7) available!
- Add -1 to heap
- time = 7

Time 8:
- Pop -1 (B) from heap → process B
- B done!
- time = 8

Total time: 8 intervals
```

**Solution:**

```python
from collections import Counter
import heapq

def leastInterval(tasks, n):
    """
    Time: O(N * log M) where N = total tasks, M = unique tasks
    Space: O(M) for heap and queue
    """
    # Step 1: Count frequencies
    freq = Counter(tasks)
    
    # Step 2: Create max-heap (negate for max-heap)
    heap = [-count for count in freq.values()]
    heapq.heapify(heap)
    
    # Step 3: Initialize tracking variables
    time = 0
    queue = []  # (remaining_count, available_time)
    
    # Step 4: Process tasks
    while heap or queue:
        time += 1
        
        if heap:
            # Process most frequent task
            count = heapq.heappop(heap)
            count += 1  # Reduce absolute value (it's negative)
            
            if count < 0:  # Task not finished
                # Add to queue with available time
                queue.append((count, time + n))
        
        # Check if any task in queue is available
        if queue and queue[0][1] == time:
            # Task available, add back to heap
            heapq.heappush(heap, queue.pop(0)[0])
    
    return time

# Test
tasks = ["A","A","A","B","B","B"]
n = 2
print(leastInterval(tasks, n))  # 8
```

**Understanding the code step by step:**

```python
# Example trace
tasks = ["A","A","A","B","B","B"]
n = 2

# Step 1: freq = {A: 3, B: 3}
# Step 2: heap = [-3, -3]
# Step 3: time = 0, queue = []

# Iteration 1:
# time = 1
# heap = [-3, -3] → pop -3
# count = -3 + 1 = -2 (2 remaining)
# queue = [(-2, 3)]  # Available at time 1+2=3
# Check queue: queue[0][1]=3, time=1, not available

# Iteration 2:
# time = 2
# heap = [-3] → pop -3
# count = -3 + 1 = -2
# queue = [(-2, 3), (-2, 4)]
# Check queue: 3 ≠ 2, not available

# Iteration 3:
# time = 3
# heap = [] (empty, idle time!)
# Check queue: 3 == 3, available!
# Add -2 to heap
# heap = [-2], queue = [(-2, 4)]

# Continue...
```

#### Alternative Solution: Mathematical Formula

**For interviews, the heap solution is preferred**, but there's also a clever math formula:

```python
from collections import Counter

def leastInterval(tasks, n):
    """
    Time: O(N) - just counting
    Space: O(1) - fixed 26 letters max
    
    Uses mathematical formula
    """
    # Count frequencies
    freq = Counter(tasks)
    
    # Find maximum frequency
    max_freq = max(freq.values())
    
    # Count tasks with maximum frequency
    max_count = sum(1 for f in freq.values() if f == max_freq)
    
    # Formula:
    # (max_freq - 1) * (n + 1) + max_count
    intervals = (max_freq - 1) * (n + 1) + max_count
    
    # Can't be less than total tasks
    return max(intervals, len(tasks))

# Test
tasks = ["A","A","A","B","B","B"]
n = 2
print(leastInterval(tasks, n))  # 8
```

**Understanding the formula:**
```
tasks = ["A","A","A","B","B","B"], n = 2

max_freq = 3 (both A and B appear 3 times)
max_count = 2 (2 tasks with max frequency)

Formula breakdown:
- (max_freq - 1) = 2 "chunks" of tasks
- (n + 1) = 3 slots per chunk
- + max_count = 2 final tasks

Visualization:
A _ _ | A _ _ | A
^     ^     ^
Chunk1 Chunk2 Final

Each chunk needs (n+1) slots: A B idle
Total: 2 * 3 + 2 = 8

Why +2 at end? For final A and B
```

#### Common Mistakes

```python
# ❌ MISTAKE 1: Not handling idle time
def leastInterval_WRONG(tasks, n):
    # Missing: while heap or queue
    while heap:  # Wrong! Stops when heap empty even if queue has items
        # ...

# ❌ MISTAKE 2: Forgetting to check queue availability
def leastInterval_WRONG(tasks, n):
    while heap or queue:
        time += 1
        if heap:
            # Process task...
        # Missing: check if queue tasks are available!

# ❌ MISTAKE 3: Using min-heap instead of max-heap
def leastInterval_WRONG(tasks, n):
    heap = list(freq.values())  # Positive values = min-heap!
    # Should be: heap = [-count for count in freq.values()]

# ✅ CORRECT
def leastInterval_CORRECT(tasks, n):
    freq = Counter(tasks)
    heap = [-count for count in freq.values()]  # Max-heap!
    heapq.heapify(heap)
    
    time = 0
    queue = []
    
    while heap or queue:  # Continue while work remains
        time += 1
        
        if heap:
            count = heapq.heappop(heap) + 1
            if count < 0:
                queue.append((count, time + n))
        
        if queue and queue[0][1] == time:  # Check availability
            heapq.heappush(heap, queue.pop(0)[0])
    
    return time
```

---

## Additional Important Concepts

### 1. Heap vs Priority Queue

**What's the difference?**

**Priority Queue:**
- Abstract data type (concept)
- Supports: insert, delete-min/max, peek
- Implementation: Can use heaps, sorted arrays, etc.

**Heap:**
- Concrete data structure
- Most efficient way to implement priority queue
- Binary heap, Fibonacci heap, etc.

```python
# Python's heapq IS a priority queue implementation
import heapq

# Priority queue operations:
pq = []
heapq.heappush(pq, (priority, item))  # Insert with priority
item = heapq.heappop(pq)  # Remove highest priority
```

### 2. When to Use Heaps?

Use heaps when you need:

✅ **Repeatedly find min/max** - O(1) peek, O(log n) extract  
✅ **Top K elements** - Maintain heap of size K  
✅ **Merge K sorted lists** - Use min-heap  
✅ **Running median** - Use two heaps  
✅ **Priority scheduling** - Tasks with priorities

❌ **Don't use heaps when:**
- Need to search for arbitrary elements - O(n)
- Need sorted output of all elements - use sorting instead
- Need to access middle elements - use other structures

### 3. Heap in Different Languages

**Python:**
```python
import heapq
heap = []
heapq.heappush(heap, 5)
min_val = heapq.heappop(heap)
```

**Java:**
```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
minHeap.add(5);
int min = minHeap.poll();
```

**C++:**
```cpp
priority_queue<int> maxHeap;  // Max-heap by default
priority_queue<int, vector<int>, greater<int>> minHeap;  // Min-heap
maxHeap.push(5);
int max = maxHeap.top();
maxHeap.pop();
```

### 4. Custom Comparisons

Sometimes we need custom heap ordering:

```python
import heapq

# Heap of tuples: sorts by first element, then second, etc.
heap = []
heapq.heappush(heap, (priority, name, data))

# Custom objects: use tuple with comparison key
class Task:
    def __init__(self, name, priority):
        self.name = name
        self.priority = priority

tasks = [Task("A", 3), Task("B", 1), Task("C", 2)]

# Convert to tuples for heap
heap = [(task.priority, task.name, task) for task in tasks]
heapq.heapify(heap)

# Pop returns: (1, "B", Task("B", 1))
priority, name, task = heapq.heappop(heap)
```

### 5. Running Median Problem

**🔗 LeetCode Link:** [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

**Classic two-heap problem!**

**Strategy:**
- Use max-heap for smaller half
- Use min-heap for larger half
- Median is between the two heaps

```python
import heapq

class MedianFinder:
    def __init__(self):
        self.small = []  # Max-heap (smaller half)
        self.large = []  # Min-heap (larger half)
    
    def addNum(self, num):
        # Add to max-heap (negate for max-heap)
        heapq.heappush(self.small, -num)
        
        # Balance: largest of small <= smallest of large
        if self.small and self.large and (-self.small[0] > self.large[0]):
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        
        # Balance sizes (differ by at most 1)
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)
    
    def findMedian(self):
        if len(self.small) > len(self.large):
            return -self.small[0]
        elif len(self.large) > len(self.small):
            return self.large[0]
        else:
            return (-self.small[0] + self.large[0]) / 2

# Test
mf = MedianFinder()
mf.addNum(1)    # [1]
mf.addNum(2)    # [1, 2]
print(mf.findMedian())  # 1.5
mf.addNum(3)    # [1, 2, 3]
print(mf.findMedian())  # 2
```

### 6. Merge K Sorted Lists

**🔗 LeetCode Link:** [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

**Perfect heap use case!**

```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeKLists(lists):
    """
    Time: O(N log k) where N = total nodes, k = number of lists
    Space: O(k) for heap
    """
    heap = []
    
    # Add first node from each list
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst.val, i, lst))
    
    dummy = ListNode(0)
    current = dummy
    
    while heap:
        val, i, node = heapq.heappop(heap)
        current.next = node
        current = current.next
        
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    
    return dummy.next
```

---

## Practice Problems by Difficulty

### Easy
1. **[703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)** - Min-heap of size K
2. **[1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)** - Max-heap simulation

### Medium
3. **[215. Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/)** - Core heap problem
4. **[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)** - Frequency + heap
5. **[373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)** - Min-heap of pairs
6. **[621. Task Scheduler](https://leetcode.com/problems/task-scheduler/)** - Heap + queue
7. **[767. Reorganize String](https://leetcode.com/problems/reorganize-string/)** - Similar to task scheduler
8. **[973. K Closest Points](https://leetcode.com/problems/k-closest-points-to-origin/)** - Distance + heap

### Hard
9. **[23. Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)** - Classic heap application
10. **[295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)** - Two heaps
11. **[502. IPO](https://leetcode.com/problems/ipo/)** - Two heaps strategy
12. **[1000. Minimum Cost to Merge Stones](https://leetcode.com/problems/minimum-cost-to-merge-stones/)** - Advanced heap + DP

---

## Interview Tips

### 1. Common Heap Patterns

**Pattern 1: Top K Elements**
- Maintain heap of size K
- Use min-heap for K largest
- Use max-heap for K smallest

**Pattern 2: K-way Merge**
- Use heap to track smallest from each source
- Pop smallest, add next from same source

**Pattern 3: Two Heaps**
- Split data into two halves
- Max-heap for smaller half, min-heap for larger half
- Use for median, scheduling problems

### 2. Time Complexity Cheat Sheet

| Operation | Time | Explanation |
|-----------|------|-------------|
| heapify | O(n) | Bottom-up construction |
| heappush | O(log n) | Bubble up |
| heappop | O(log n) | Bubble down |
| heap[0] | O(1) | Peek at root |
| heapreplace | O(log n) | Pop + push combined |
| heappushpop | O(log n) | Push + pop combined |
| nlargest(k, arr) | O(n log k) | Uses heap internally |

### 3. Space Complexity

- Heap of size K: O(K)
- Heap of all elements: O(N)
- Usually: Space = O(heap size)

### 4. Common Interview Questions

**Q: "Why use heap instead of sorting?"**
**A:** "Sorting is O(n log n) for all elements. Heap is O(n log k) when we only need top K elements, which is much faster when k << n."

**Q: "When would you NOT use a heap?"**
**A:** "When you need to search for arbitrary elements (O(n) in heap), or when you need all elements sorted (better to just sort), or when accessing elements by index is important."

