# Arrays Section - Complete Deep Dive

Arrays are the most fundamental data structure and the foundation for understanding all other structures. This section covers basic problems, advanced techniques, two pointers, and sliding window patterns.

---

## 1. Understanding Arrays

### 1.1 What is an Array?

An array is a contiguous block of memory that stores elements of the same type. Each element can be accessed directly using its index.

**Key Properties**:
- **Fixed or Dynamic Size**: In Python, lists are dynamic arrays
- **Zero-Indexed**: First element is at index 0
- **Random Access**: O(1) access to any element by index
- **Contiguous Memory**: Elements stored next to each other

### 1.2 Array Operations Time Complexity

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| Access by index | O(1) | `arr[i]` |
| Search (unsorted) | O(n) | Must check each element |
| Search (sorted) | O(log n) | Binary search |
| Insert at end | O(1) amortized | `append()` |
| Insert at beginning | O(n) | Must shift all elements |
| Insert at middle | O(n) | Must shift subsequent elements |
| Delete at end | O(1) | `pop()` |
| Delete at beginning | O(n) | Must shift all elements |
| Delete at middle | O(n) | Must shift subsequent elements |

### 1.3 Python List Methods You Must Know

```python
# Creation
arr = []                    # Empty list
arr = [1, 2, 3]            # With values
arr = [0] * n              # n zeros
arr = list(range(n))       # [0, 1, 2, ..., n-1]

# Adding elements
arr.append(x)              # Add to end - O(1)
arr.insert(i, x)           # Insert at index i - O(n)
arr.extend([4, 5])         # Add multiple - O(k)

# Removing elements
arr.pop()                  # Remove last - O(1)
arr.pop(i)                 # Remove at index - O(n)
arr.remove(x)              # Remove first occurrence of x - O(n)

# Accessing
arr[0]                     # First element
arr[-1]                    # Last element
arr[1:4]                   # Slice from index 1 to 3

# Searching
x in arr                   # Check existence - O(n)
arr.index(x)               # Find index of x - O(n)
arr.count(x)               # Count occurrences - O(n)

# Sorting
arr.sort()                 # In-place sort - O(n log n)
sorted(arr)                # Returns new sorted list - O(n log n)
arr.sort(reverse=True)     # Descending order

# Other useful
len(arr)                   # Length - O(1)
min(arr), max(arr)         # Min/Max - O(n)
sum(arr)                   # Sum all elements - O(n)
arr.reverse()              # Reverse in-place - O(n)
```

---

## 2. Basic Array Problems

### 2.1 Contains Duplicate

**Problem**: Given an integer array `nums`, return `true` if any value appears at least twice, and `false` if every element is distinct.

**Examples**:
```
Input: nums = [1, 2, 3, 1]
Output: true (1 appears twice)

Input: nums = [1, 2, 3, 4]
Output: false (all distinct)
```

#### Approach 1: Brute Force (Don't use this)

```python
def containsDuplicate(nums):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] == nums[j]:
                return True
    return False
```
- **Time**: O(n²) - nested loops
- **Space**: O(1)

#### Approach 2: Sorting

```python
def containsDuplicate(nums):
    nums.sort()
    for i in range(1, len(nums)):
        if nums[i] == nums[i-1]:
            return True
    return False
```
- **Time**: O(n log n) - dominated by sort
- **Space**: O(1) or O(n) depending on sort implementation

#### Approach 3: Using a Set (Optimal)

**Key Insight**: Sets in Python do not allow duplicates. If we convert to a set and the length changes, there were duplicates.

```python
def containsDuplicate(nums):
    return len(nums) != len(set(nums))
```

**Alternative with early exit**:
```python
def containsDuplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return True
        seen.add(num)
    return False
```

- **Time**: O(n) - single pass, set operations are O(1)
- **Space**: O(n) - storing up to n elements in set

**Why Sets Work**:
- Set creation iterates through all elements: O(n)
- Set lookup/insertion is O(1) average
- Sets automatically handle duplicates

**Python Set Quick Reference**:
```python
s = set()           # Create empty set
s.add(x)            # Add element - O(1)
s.remove(x)         # Remove element - O(1), raises error if missing
s.discard(x)        # Remove element - O(1), no error if missing
x in s              # Check membership - O(1)
len(s)              # Size - O(1)
```

---

### 2.2 Missing Number

**Problem**: Given an array `nums` containing n distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

**Examples**:
```
Input: nums = [3, 0, 1]
Output: 2 (range is 0-3, missing 2)

Input: nums = [0, 1]
Output: 2 (range is 0-2, missing 2)

Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
```

#### Approach 1: Sorting (Not optimal)

```python
def missingNumber(nums):
    nums.sort()
    for i, num in enumerate(nums):
        if i != num:
            return i
    return len(nums)
```
- **Time**: O(n log n) - sorting bottleneck
- **Space**: O(1) or O(n)

#### Approach 2: Mathematical Formula (Optimal)

**Key Insight**: The sum of numbers from 0 to n is `n*(n+1)/2`. The difference between expected sum and actual sum is the missing number.

```python
def missingNumber(nums):
    n = len(nums)
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(nums)
    return expected_sum - actual_sum
```

- **Time**: O(n) - single pass for sum
- **Space**: O(1) - only storing a few variables

**Step-by-step for [3, 0, 1]**:
1. n = 3 (length of array)
2. expected_sum = 3 * 4 / 2 = 6 (sum of 0+1+2+3)
3. actual_sum = 3 + 0 + 1 = 4
4. missing = 6 - 4 = 2 ✓

#### Approach 3: XOR Bit Manipulation

**Key Insight**: XOR has these properties:
- `a ^ a = 0` (same numbers cancel out)
- `a ^ 0 = a` (XOR with zero gives same number)
- XOR is commutative and associative

```python
def missingNumber(nums):
    xor = len(nums)
    for i, num in enumerate(nums):
        xor ^= i ^ num
    return xor
```

- **Time**: O(n)
- **Space**: O(1)

**Why it works**: XOR all indices (0 to n-1) with all values and n. All numbers that exist cancel out, leaving only the missing one.

---

### 2.3 Find All Numbers Disappeared in an Array

**Problem**: Given an array `nums` of n integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

**Example**:
```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
Range is 1-8, missing 5 and 6
```

#### Approach 1: Set Comparison

```python
def findDisappearedNumbers(nums):
    num_set = set(nums)
    result = []
    for i in range(1, len(nums) + 1):
        if i not in num_set:
            result.append(i)
    return result
```

- **Time**: O(n)
- **Space**: O(n) for the set

#### Approach 2: In-Place Marking (Optimal Space)

**Key Insight**: Use the array itself as a hash table. Mark indices as "seen" by negating the value at that index.

```python
def findDisappearedNumbers(nums):
    # Mark seen numbers by negating value at corresponding index
    for num in nums:
        index = abs(num) - 1  # -1 because array is 0-indexed but values are 1-n
        nums[index] = -abs(nums[index])
    
    # Find indices with positive values (not seen)
    result = []
    for i in range(len(nums)):
        if nums[i] > 0:
            result.append(i + 1)  # +1 to convert back to 1-indexed
    return result
```

- **Time**: O(n)
- **Space**: O(1) - modifying input in place

**Walkthrough for [4,3,2,7,8,2,3,1]**:
```
Initial: [4, 3, 2, 7, 8, 2, 3, 1]

Process 4: negate index 3 → [4, 3, 2, -7, 8, 2, 3, 1]
Process 3: negate index 2 → [4, 3, -2, -7, 8, 2, 3, 1]
Process 2: negate index 1 → [4, -3, -2, -7, 8, 2, 3, 1]
Process 7: negate index 6 → [4, -3, -2, -7, 8, 2, -3, 1]
Process 8: negate index 7 → [4, -3, -2, -7, 8, 2, -3, -1]
Process 2: already negative, skip
Process 3: already negative, skip
Process 1: negate index 0 → [-4, -3, -2, -7, 8, 2, -3, -1]

Positive values at indices 4 and 5 → missing numbers are 5 and 6
```

---

### 2.4 Two Sum

**Problem**: Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`. Assume exactly one solution exists.

**Example**:
```
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
Because nums[0] + nums[1] = 2 + 7 = 9
```

#### Approach 1: Brute Force

```python
def twoSum(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
```
- **Time**: O(n²)
- **Space**: O(1)

#### Approach 2: Hash Map (Optimal)

**Key Insight**: For each number, the only value that works is `target - current_number`. We can check if this complement exists in a hash map.

```python
def twoSum(nums, target):
    hashmap = {}  # value -> index
    
    for i, num in enumerate(nums):
        diff = target - num
        if diff in hashmap:
            return [hashmap[diff], i]
        hashmap[num] = i
    
    return []  # No solution found
```

- **Time**: O(n) - single pass
- **Space**: O(n) - storing hash map

**Why we build the hash map while iterating** (not before):
- Prevents using the same element twice
- Example: nums = [3, 3], target = 6
- If we pre-built: both 3's would map to same key
- Building while iterating: first 3 goes in map, second 3 finds it

**Detailed Walkthrough for [2, 7, 11, 15], target = 9**:
```
hashmap = {}

i=0, num=2:
  diff = 9 - 2 = 7
  7 not in hashmap
  hashmap = {2: 0}

i=1, num=7:
  diff = 9 - 7 = 2
  2 IS in hashmap at index 0
  return [0, 1] ✓
```

**Detailed Walkthrough for [3, 2, 4], target = 6**:
```
hashmap = {}

i=0, num=3:
  diff = 6 - 3 = 3
  3 not in hashmap
  hashmap = {3: 0}

i=1, num=2:
  diff = 6 - 2 = 4
  4 not in hashmap
  hashmap = {3: 0, 2: 1}

i=2, num=4:
  diff = 6 - 4 = 2
  2 IS in hashmap at index 1
  return [1, 2] ✓
```

---

## 3. Array Techniques

### 3.1 How Many Numbers Are Smaller Than the Current Number

**Problem**: For each `nums[i]`, find out how many numbers in the array are smaller than it.

**Example**:
```
Input: nums = [8, 1, 2, 2, 3]
Output: [4, 0, 1, 1, 3]

8 is bigger than 4 numbers (1, 2, 2, 3)
1 is bigger than 0 numbers
2 is bigger than 1 number (1)
2 is bigger than 1 number (1)
3 is bigger than 3 numbers (1, 2, 2)
```

#### Approach: Sort + Dictionary

**Key Insight**: If we sort the array, each number's index tells us how many numbers are smaller. For duplicates, we only record the first occurrence's index.

```python
def smallerNumbersThanCurrent(nums):
    # Create sorted copy
    temp = sorted(nums)
    
    # Build dictionary: number -> count of smaller numbers
    count_dict = {}
    for i, num in enumerate(temp):
        if num not in count_dict:  # Only first occurrence
            count_dict[num] = i
    
    # Build result using dictionary lookup
    return [count_dict[num] for num in nums]
```

- **Time**: O(n log n) - dominated by sorting
- **Space**: O(n) - for sorted array and dictionary

**Walkthrough for [8, 1, 2, 2, 3]**:
```
sorted: [1, 2, 2, 3, 8]
indices: 0  1  2  3  4

Building dictionary (only first occurrences):
1 at index 0 → {1: 0}
2 at index 1 → {1: 0, 2: 1}
2 at index 2 → skip (already exists)
3 at index 3 → {1: 0, 2: 1, 3: 3}
8 at index 4 → {1: 0, 2: 1, 3: 3, 8: 4}

Result for [8, 1, 2, 2, 3]:
8 → 4
1 → 0
2 → 1
2 → 1
3 → 3
Output: [4, 0, 1, 1, 3] ✓
```

---

### 3.2 Minimum Time Visiting All Points

**Problem**: On a 2D plane, given an array of coordinate points, return the minimum time to visit all points in order. You can move diagonally (1 step), horizontally (1 step), or vertically (1 step).

**Example**:
```
Input: points = [[1,1], [3,4], [-1,0]]
Output: 7

From [1,1] to [3,4]: max(|3-1|, |4-1|) = max(2, 3) = 3 steps
From [3,4] to [-1,0]: max(|-1-3|, |0-4|) = max(4, 4) = 4 steps
Total: 3 + 4 = 7
```

**Key Insight**: The distance between two points is the **maximum** of the absolute differences in x and y coordinates. This is because diagonal movement covers both x and y distance simultaneously.

```
Moving from (0,0) to (3,5):
- x difference: 3
- y difference: 5
- Move diagonally 3 times → reach (3,3)
- Move vertically 2 times → reach (3,5)
- Total: 5 steps = max(3, 5)
```

```python
def minTimeToVisitAllPoints(points):
    time = 0
    for i in range(1, len(points)):
        x1, y1 = points[i-1]
        x2, y2 = points[i]
        time += max(abs(x2 - x1), abs(y2 - y1))
    return time
```

- **Time**: O(n) - visit each point once
- **Space**: O(1) - only tracking time

---

### 3.3 Spiral Matrix (Medium)

**Problem**: Given an m x n matrix, return all elements in spiral order.

**Example**:
```
Input: matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

**Visual**:
```
→ → →
    ↓
← ← ↓
↑   ↓
↑ ← ←
```

**Key Insight**: Process layer by layer, using the matrix itself. Pop rows and columns as you go:
1. Pop and add the first row (left to right)
2. Pop and add the last element of each remaining row (top to bottom)
3. Pop and add the last row in reverse (right to left)
4. Pop and add the first element of each remaining row in reverse (bottom to top)

```python
def spiralOrder(matrix):
    result = []
    
    while matrix:
        # Step 1: Add first row left to right
        result += matrix.pop(0)
        
        # Step 2: Add last column top to bottom
        if matrix and matrix[0]:
            for row in matrix:
                result.append(row.pop())
        
        # Step 3: Add last row right to left
        if matrix:
            result += matrix.pop()[::-1]
        
        # Step 4: Add first column bottom to top
        if matrix and matrix[0]:
            for row in matrix[::-1]:
                result.append(row.pop(0))
    
    return result
```

- **Time**: O(m × n) - visit each element once
- **Space**: O(1) - excluding output array (modifying input)

**Alternative: Four Pointers Approach**:

```python
def spiralOrder(matrix):
    result = []
    if not matrix:
        return result
    
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    
    while top <= bottom and left <= right:
        # Left to right
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1
        
        # Top to bottom
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1
        
        # Right to left
        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1
        
        # Bottom to top
        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1
    
    return result
```

---

### 3.4 Number of Islands (Medium)

**Problem**: Given an m x n 2D binary grid where '1' represents land and '0' represents water, count the number of islands. An island is surrounded by water and formed by connecting adjacent lands horizontally or vertically.

**Example**:
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

**Key Insight**: Use BFS or DFS to "flood fill" each island when found, marking visited cells. Each time we find an unvisited '1', it's a new island.

#### BFS Solution:

```python
from collections import deque

def numIslands(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    visited = set()
    islands = 0
    
    def bfs(r, c):
        queue = deque([(r, c)])
        visited.add((r, c))
        
        while queue:
            row, col = queue.popleft()
            # Check all 4 directions
            directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
            
            for dr, dc in directions:
                new_r, new_c = row + dr, col + dc
                # Check bounds, is land, not visited
                if (0 <= new_r < rows and 
                    0 <= new_c < cols and 
                    grid[new_r][new_c] == "1" and 
                    (new_r, new_c) not in visited):
                    queue.append((new_r, new_c))
                    visited.add((new_r, new_c))
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1" and (r, c) not in visited:
                bfs(r, c)
                islands += 1
    
    return islands
```

#### DFS Solution:

```python
def numIslands(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    islands = 0
    
    def dfs(r, c):
        # Base cases
        if r < 0 or r >= rows or c < 0 or c >= cols:
            return
        if grid[r][c] != "1":
            return
        
        # Mark as visited by changing to "0"
        grid[r][c] = "0"
        
        # Explore all 4 directions
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1":
                dfs(r, c)
                islands += 1
    
    return islands
```

- **Time**: O(m × n) - visit each cell once
- **Space**: O(m × n) - for visited set or recursion stack

**Walkthrough**:
```
Grid:
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1

Start at (0,0): Found '1', BFS/DFS marks all connected '1's
Visit: (0,0), (0,1), (1,0), (1,1)
Islands = 1

Continue scanning, find '1' at (2,2)
Visit: (2,2)
Islands = 2

Continue scanning, find '1' at (3,3)
Visit: (3,3), (3,4)
Islands = 3

Final answer: 3
```

---

## 4. Two Pointers Pattern

The two-pointer technique uses two pointers to iterate through data, typically from opposite ends or at different speeds.

### 4.1 Best Time to Buy and Sell Stock

**Problem**: Given an array `prices` where `prices[i]` is the price on day i, find the maximum profit from one buy and one sell. You must buy before you sell.

**Example**:
```
Input: prices = [7, 1, 5, 3, 6, 4]
Output: 5
Buy at price 1, sell at price 6 → profit = 5
```

**Key Insight**: Use a "greedy" approach. Track the minimum price seen so far and calculate potential profit at each step.

```python
def maxProfit(prices):
    left = 0   # Buy pointer
    right = 1  # Sell pointer
    max_profit = 0
    
    while right < len(prices):
        if prices[left] < prices[right]:
            # Profitable trade possible
            profit = prices[right] - prices[left]
            max_profit = max(max_profit, profit)
        else:
            # Found a new minimum, move buy pointer
            left = right
        right += 1
    
    return max_profit
```

**Alternative (single variable)**:
```python
def maxProfit(prices):
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)
        max_profit = max(max_profit, price - min_price)
    
    return max_profit
```

- **Time**: O(n) - single pass
- **Space**: O(1)

**Walkthrough for [7, 1, 5, 3, 6, 4]**:
```
left=0 (price 7), right=1 (price 1)
7 > 1, so move left to 1
left=1 (price 1)

right=2 (price 5)
1 < 5, profit = 4, max_profit = 4

right=3 (price 3)
1 < 3, profit = 2, max_profit = 4

right=4 (price 6)
1 < 6, profit = 5, max_profit = 5

right=5 (price 4)
1 < 4, profit = 3, max_profit = 5

Final: 5
```

---

### 4.2 Squares of a Sorted Array

**Problem**: Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

**Example**:
```
Input: nums = [-4, -1, 0, 3, 10]
Output: [0, 1, 9, 16, 100]
```

**Challenge**: Negative numbers when squared become positive and may be larger than positive numbers' squares.

#### Approach 1: Simple Sort

```python
def sortedSquares(nums):
    return sorted(x * x for x in nums)
```
- **Time**: O(n log n)
- **Space**: O(n)

#### Approach 2: Two Pointers (Optimal)

**Key Insight**: After squaring, the largest values are at either end (because of absolute values). Use two pointers from both ends and build result from largest to smallest.

```python
def sortedSquares(nums):
    n = len(nums)
    result = [0] * n
    left, right = 0, n - 1
    position = n - 1  # Fill from the end
    
    while left <= right:
        left_sq = nums[left] ** 2
        right_sq = nums[right] ** 2
        
        if left_sq > right_sq:
            result[position] = left_sq
            left += 1
        else:
            result[position] = right_sq
            right -= 1
        position -= 1
    
    return result
```

- **Time**: O(n)
- **Space**: O(n) for result (O(1) extra)

**Walkthrough for [-4, -1, 0, 3, 10]**:
```
result = [_, _, _, _, _]
left=0 (-4), right=4 (10)

Iteration 1:
  left_sq = 16, right_sq = 100
  100 > 16, place 100 at end
  result = [_, _, _, _, 100]
  right = 3

Iteration 2:
  left_sq = 16, right_sq = 9
  16 > 9, place 16
  result = [_, _, _, 16, 100]
  left = 1

Iteration 3:
  left_sq = 1, right_sq = 9
  9 > 1, place 9
  result = [_, _, 9, 16, 100]
  right = 2

Iteration 4:
  left_sq = 1, right_sq = 0
  1 > 0, place 1
  result = [_, 1, 9, 16, 100]
  left = 2

Iteration 5:
  left = right = 2
  left_sq = 0, right_sq = 0
  place 0
  result = [0, 1, 9, 16, 100]

Final: [0, 1, 9, 16, 100]
```

---

### 4.3 Three Sum (Medium)

**Problem**: Given an integer array `nums`, return all unique triplets `[nums[i], nums[j], nums[k]]` such that `i != j != k` and `nums[i] + nums[j] + nums[k] = 0`.

**Example**:
```
Input: nums = [-1, 0, 1, 2, -1, -4]
Output: [[-1, -1, 2], [-1, 0, 1]]
```

**Key Insight**: 
1. Sort the array first
2. For each number, use two pointers to find pairs that sum to its negative
3. Skip duplicates to avoid duplicate triplets

```python
def threeSum(nums):
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates for i
        if i > 0 and nums[i] == nums[i-1]:
            continue
        
        # Early termination: if first number is positive, no solution possible
        if nums[i] > 0:
            break
        
        left = i + 1
        right = len(nums) - 1
        target = -nums[i]
        
        while left < right:
            current_sum = nums[left] + nums[right]
            
            if current_sum < target:
                left += 1
            elif current_sum > target:
                right -= 1
            else:
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates for left and right
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                
                left += 1
                right -= 1
    
    return result
```

- **Time**: O(n²) - outer loop O(n), inner two-pointer O(n)
- **Space**: O(1) or O(n) depending on sort implementation

**Walkthrough for [-1, 0, 1, 2, -1, -4]**:

```
After sorting: [-4, -1, -1, 0, 1, 2]

i=0, nums[i]=-4, target=4
  left=1 (-1), right=5 (2)
  -1 + 2 = 1 < 4, move left
  left=2 (-1), right=5 (2)
  -1 + 2 = 1 < 4, move left
  left=3 (0), right=5 (2)
  0 + 2 = 2 < 4, move left
  left=4 (1), right=5 (2)
  1 + 2 = 3 < 4, move left
  left=5, left >= right, exit

i=1, nums[i]=-1, target=1
  left=2 (-1), right=5 (2)
  -1 + 2 = 1 == 1 ✓
  Found: [-1, -1, 2]
  Skip duplicate lefts, skip duplicate rights
  left=3, right=4
  0 + 1 = 1 == 1 ✓
  Found: [-1, 0, 1]
  left=4, right=3, exit

i=2, nums[i]=-1
  Same as previous -1, skip (duplicate check)

i=3, nums[i]=0, target=0
  left=4 (1), right=5 (2)
  1 + 2 = 3 > 0, move right
  left=4, right=4, exit

Final: [[-1, -1, 2], [-1, 0, 1]]
```

**Why sorting works for avoiding duplicates**:
- Duplicates become adjacent after sorting
- We can simply check `if nums[i] == nums[i-1]: continue`
- This ensures we don't start a new search with the same first element

---

### 4.4 Longest Mountain in Array

**Problem**: Given an integer array `arr`, return the length of the longest subarray which is a mountain. A mountain array has:
- Length >= 3
- There exists some index i where elements strictly increase from start to i, then strictly decrease from i to end

**Example**:
```
Input: arr = [2, 1, 4, 7, 3, 2, 5]
Output: 5
The mountain is [1, 4, 7, 3, 2] with peak at 7
```

**Key Insight**: For each potential peak, expand outward using two pointers to find the mountain length.

```python
def longestMountain(arr):
    n = len(arr)
    if n < 3:
        return 0
    
    longest = 0
    
    for i in range(1, n - 1):
        # Check if current position is a peak
        if arr[i] > arr[i-1] and arr[i] > arr[i+1]:
            # Expand left
            left = i
            while left > 0 and arr[left] > arr[left - 1]:
                left -= 1
            
            # Expand right
            right = i
            while right < n - 1 and arr[right] > arr[right + 1]:
                right += 1
            
            # Calculate length
            length = right - left + 1
            longest = max(longest, length)
    
    return longest
```

- **Time**: O(n) average, O(n²) worst case
- **Space**: O(1)

**Optimized Single Pass**:

```python
def longestMountain(arr):
    n = len(arr)
    longest = 0
    i = 1
    
    while i < n - 1:
        # Check if peak
        if arr[i] > arr[i-1] and arr[i] > arr[i+1]:
            # Find left boundary
            left = i - 1
            while left > 0 and arr[left] > arr[left - 1]:
                left -= 1
            
            # Find right boundary
            right = i + 1
            while right < n - 1 and arr[right] > arr[right + 1]:
                right += 1
            
            longest = max(longest, right - left + 1)
            i = right  # Skip to end of mountain
        else:
            i += 1
    
    return longest
```

- **Time**: O(n) - each element visited at most twice
- **Space**: O(1)

**Walkthrough for [2, 1, 4, 7, 3, 2, 5]**:
```
i=1: arr[1]=1
  Not a peak (1 < 2 on left)

i=2: arr[2]=4
  Not a peak (4 < 7 on right)

i=3: arr[3]=7
  IS a peak! (7 > 4 and 7 > 3)
  Expand left: 7>4>1, stop at index 1
  Expand right: 7>3>2, stop at index 5
  Length = 5 - 1 + 1 = 5

i=4: arr[4]=3
  Not a peak

i=5: arr[5]=2
  Not a peak (at boundary)

Longest = 5
```

---

## 5. Sliding Window Pattern

The sliding window technique maintains a "window" over a portion of data, sliding it along to process different sections efficiently.

### 5.1 When to Use Sliding Window

Use sliding window when:
- Problem involves contiguous sequences (subarrays, substrings)
- Looking for longest/shortest/specific window that meets criteria
- Need to track sum, count, or properties within a range

**Two types**:
1. **Fixed-size window**: Window size is predetermined
2. **Dynamic window**: Window size changes based on conditions

### 5.2 Contains Duplicate II

**Problem**: Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` such that `nums[i] == nums[j]` and `|i - j| <= k`.

**Example**:
```
Input: nums = [1, 2, 3, 1], k = 3
Output: true
nums[0] == nums[3], and |0 - 3| = 3 <= 3

Input: nums = [1, 2, 3, 1, 2, 3], k = 2
Output: false
```

**Key Insight**: Maintain a sliding window of size k using a set. If we find a duplicate within this window, return true.

```python
def containsNearbyDuplicate(nums, k):
    window = set()
    
    for i, num in enumerate(nums):
        # Check if duplicate exists in current window
        if num in window:
            return True
        
        # Add current number to window
        window.add(num)
        
        # Maintain window size of k
        if len(window) > k:
            window.remove(nums[i - k])
    
    return False
```

- **Time**: O(n) - single pass
- **Space**: O(k) - set stores at most k elements

**Walkthrough for [1, 2, 3, 1], k=3**:
```
i=0, num=1:
  1 not in window (empty)
  window = {1}
  len(1) <= 3, no removal

i=1, num=2:
  2 not in window
  window = {1, 2}
  len(2) <= 3, no removal

i=2, num=3:
  3 not in window
  window = {1, 2, 3}
  len(3) <= 3, no removal

i=3, num=1:
  1 IS in window!
  Return True ✓
```

**Walkthrough for [1, 2, 3, 1, 2, 3], k=2**:
```
i=0: window = {1}
i=1: window = {1, 2}
i=2: window = {1, 2, 3}, len > k=2, remove nums[0]=1
     window = {2, 3}
i=3: 1 not in {2, 3}
     window = {2, 3, 1}, remove nums[1]=2
     window = {3, 1}
i=4: 2 not in {3, 1}
     window = {3, 1, 2}, remove nums[2]=3
     window = {1, 2}
i=5: 3 not in {1, 2}
     window = {1, 2, 3}, remove nums[3]=1
     window = {2, 3}

Return False (no duplicate found within k distance)
```

---

### 5.3 Minimum Absolute Difference

**Problem**: Given an array of distinct integers `arr`, find all pairs of elements with the minimum absolute difference.

**Example**:
```
Input: arr = [4, 2, 1, 3]
Output: [[1, 2], [2, 3], [3, 4]]
Minimum difference is 1
```

**Key Insight**: After sorting, minimum differences must be between adjacent elements. Use a two-element sliding window.

```python
def minimumAbsDifference(arr):
    arr.sort()
    
    # First pass: find minimum difference
    min_diff = float('inf')
    for i in range(1, len(arr)):
        min_diff = min(min_diff, arr[i] - arr[i-1])
    
    # Second pass: collect all pairs with minimum difference
    result = []
    for i in range(1, len(arr)):
        if arr[i] - arr[i-1] == min_diff:
            result.append([arr[i-1], arr[i]])
    
    return result
```

**Combined single pass**:
```python
def minimumAbsDifference(arr):
    arr.sort()
    min_diff = float('inf')
    result = []
    
    for i in range(1, len(arr)):
        diff = arr[i] - arr[i-1]
        
        if diff < min_diff:
            min_diff = diff
            result = [[arr[i-1], arr[i]]]  # Reset result
        elif diff == min_diff:
            result.append([arr[i-1], arr[i]])
    
    return result
```

- **Time**: O(n log n) - dominated by sorting
- **Space**: O(n) - for sorted array (if not in-place) and result

**Walkthrough for [4, 2, 1, 3]**:
```
After sorting: [1, 2, 3, 4]

i=1: diff = 2-1 = 1
  1 < inf, min_diff = 1
  result = [[1, 2]]

i=2: diff = 3-2 = 1
  1 == 1, append
  result = [[1, 2], [2, 3]]

i=3: diff = 4-3 = 1
  1 == 1, append
  result = [[1, 2], [2, 3], [3, 4]]

Final: [[1, 2], [2, 3], [3, 4]]
```

---

### 5.4 Minimum Size Subarray Sum (Medium)

**Problem**: Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray whose sum is greater than or equal to target. If none exists, return 0.

**Example**:
```
Input: target = 7, nums = [2, 3, 1, 2, 4, 3]
Output: 2
Subarray [4, 3] has sum 7 with minimum length 2
```

**Key Insight**: Use a dynamic sliding window. Expand right to increase sum, shrink left when sum meets target.

```python
def minSubArrayLen(target, nums):
    min_length = float('inf')
    left = 0
    current_sum = 0
    
    for right in range(len(nums)):
        # Expand window
        current_sum += nums[right]
        
        # Shrink window while condition is met
        while current_sum >= target:
            # Update minimum length
            min_length = min(min_length, right - left + 1)
            # Remove left element and shrink
            current_sum -= nums[left]
            left += 1
    
    return min_length if min_length != float('inf') else 0
```

- **Time**: O(n) - each element visited at most twice (once by right, once by left)
- **Space**: O(1)

**Detailed Walkthrough for target=7, nums=[2, 3, 1, 2, 4, 3]**:

```
Initial: left=0, current_sum=0, min_length=inf

right=0: nums[0]=2
  current_sum = 0 + 2 = 2
  2 < 7, don't shrink
  Window: [2]

right=1: nums[1]=3
  current_sum = 2 + 3 = 5
  5 < 7, don't shrink
  Window: [2, 3]

right=2: nums[2]=1
  current_sum = 5 + 1 = 6
  6 < 7, don't shrink
  Window: [2, 3, 1]

right=3: nums[3]=2
  current_sum = 6 + 2 = 8
  8 >= 7, start shrinking!
    length = 3 - 0 + 1 = 4
    min_length = 4
    Remove nums[0]=2: current_sum = 8 - 2 = 6
    left = 1
  6 < 7, stop shrinking
  Window: [3, 1, 2]

right=4: nums[4]=4
  current_sum = 6 + 4 = 10
  10 >= 7, shrink!
    length = 4 - 1 + 1 = 4
    min_length = min(4, 4) = 4
    Remove nums[1]=3: current_sum = 10 - 3 = 7
    left = 2
  7 >= 7, shrink again!
    length = 4 - 2 + 1 = 3
    min_length = min(4, 3) = 3
    Remove nums[2]=1: current_sum = 7 - 1 = 6
    left = 3
  6 < 7, stop shrinking
  Window: [2, 4]

right=5: nums[5]=3
  current_sum = 6 + 3 = 9
  9 >= 7, shrink!
    length = 5 - 3 + 1 = 3
    min_length = min(3, 3) = 3
    Remove nums[3]=2: current_sum = 9 - 2 = 7
    left = 4
  7 >= 7, shrink again!
    length = 5 - 4 + 1 = 2
    min_length = min(3, 2) = 2 ✓
    Remove nums[4]=4: current_sum = 7 - 4 = 3
    left = 5
  3 < 7, stop shrinking
  Window: [3]

Final: min_length = 2
```

**Why O(n) and not O(n²)?**

Although there's a nested while loop inside the for loop, each element is added once (by right pointer) and removed at most once (by left pointer). The left pointer only moves forward, never backward. Total operations: at most 2n.

---

## 6. Common Array Patterns Summary

| Pattern | When to Use | Key Technique |
|---------|-------------|---------------|
| **Hash Map/Set** | Finding pairs, duplicates, counting | O(1) lookup |
| **Sorting** | When order helps, finding pairs | O(n log n) preprocessing |
| **Two Pointers** | Sorted arrays, pairs summing to target | Move pointers based on comparison |
| **Sliding Window (Fixed)** | Subarrays of fixed size | Maintain window, slide |
| **Sliding Window (Dynamic)** | Min/max subarray with constraint | Expand right, shrink left |
| **In-place Marking** | Finding missing/duplicate in range [1,n] | Use array as hash table |
| **Prefix Sum** | Range sum queries | Precompute cumulative sums |

---

## 7. Practice Problems Checklist

**Easy**:
- [ ] Contains Duplicate
- [ ] Missing Number
- [ ] Find All Numbers Disappeared
- [ ] Two Sum
- [ ] Best Time to Buy and Sell Stock
- [ ] Squares of Sorted Array

**Medium**:
- [ ] Three Sum
- [ ] Spiral Matrix
- [ ] Number of Islands
- [ ] Longest Mountain
- [ ] Minimum Size Subarray Sum
- [ ] Container With Most Water
- [ ] Product of Array Except Self

**Hard** (for later):
- [ ] Trapping Rain Water
- [ ] Sliding Window Maximum
- [ ] First Missing Positive

---

## 8. Key Takeaways

1. **Master the basics first**: Contains Duplicate, Two Sum, Missing Number
2. **Know when to use hash structures**: Trading space for time is usually worth it
3. **Sorting enables many optimizations**: Three Sum, Minimum Absolute Difference
4. **Two pointers work on sorted data**: Save O(n) over brute force
5. **Sliding window for contiguous subarrays**: Fixed or dynamic based on problem
6. **Draw it out**: Especially for Spiral Matrix, Number of Islands
7. **Think about edge cases**: Empty arrays, single elements, all same values
