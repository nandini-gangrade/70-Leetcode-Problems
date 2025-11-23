# Arrays Section - Complete Beginner's Deep Dive

This guide assumes you're starting from scratch. Every concept, term, and technique is explained thoroughly before we use it.

---

## Table of Contents
1. [What is an Array?](#1-what-is-an-array)
2. [Python Lists - Everything You Need to Know](#2-python-lists---everything-you-need-to-know)
3. [Basic Array Problems](#3-basic-array-problems)
4. [Array Techniques](#4-array-techniques)
5. [Two Pointers Pattern](#5-two-pointers-pattern)
6. [Sliding Window Pattern](#6-sliding-window-pattern)

---

## 1. What is an Array?

### 1.1 Definition

An **array** is like a row of boxes, where each box can hold one item (a number, string, etc.). Each box has a **number** (called an **index**) so you can find it easily.

```
Index:    0     1     2     3     4
        +-----+-----+-----+-----+-----+
Array:  |  5  |  2  |  8  |  1  |  9  |
        +-----+-----+-----+-----+-----+
```

**Key Terms**:
- **Element**: Each item stored in the array (5, 2, 8, 1, 9 are elements)
- **Index**: The position number of each element (starts at 0, not 1!)
- **Length**: Total number of elements (this array has length 5)

### 1.2 Why Arrays Start at Index 0 (Zero-Indexed)

This is a common source of confusion for beginners. In most programming languages including Python:
- The **first** element is at index **0**
- The **second** element is at index **1**
- The **last** element is at index **length - 1**

```python
arr = [5, 2, 8, 1, 9]
print(arr[0])  # Output: 5 (first element)
print(arr[4])  # Output: 9 (last element, because length is 5, so 5-1=4)
print(arr[5])  # ERROR! Index out of range
```

### 1.3 Why Arrays Are Important

Arrays are the most fundamental data structure because:
1. **Fast access**: Get any element instantly using its index - O(1)
2. **Foundation for other structures**: Stacks, queues, heaps are built on arrays
3. **Most common in interviews**: ~40% of coding questions involve arrays

### 1.4 Array vs List in Python

In Python, we use **lists** which are dynamic arrays:

| Feature | Traditional Array | Python List |
|---------|-------------------|-------------|
| Size | Fixed when created | Can grow/shrink |
| Data types | All same type | Can mix types |
| Syntax | Varies by language | `[1, 2, 3]` |

```python
# Python list - can hold different types and grow
my_list = [1, "hello", 3.14, True]
my_list.append(100)  # Now has 5 elements
```

---

## 2. Python Lists - Everything You Need to Know

Before solving problems, you MUST know these operations by heart.

### 2.1 Creating Lists

```python
# Empty list
empty = []

# List with values
numbers = [1, 2, 3, 4, 5]

# List of n zeros (very useful!)
n = 5
zeros = [0] * n  # [0, 0, 0, 0, 0]

# List from 0 to n-1
indices = list(range(5))  # [0, 1, 2, 3, 4]

# List from 1 to n
one_to_five = list(range(1, 6))  # [1, 2, 3, 4, 5]
```

### 2.2 Accessing Elements

```python
arr = [10, 20, 30, 40, 50]

# By positive index (from start)
arr[0]   # 10 (first)
arr[2]   # 30 (third)

# By negative index (from end) - VERY USEFUL!
arr[-1]  # 50 (last element)
arr[-2]  # 40 (second to last)

# Slicing: arr[start:end] - end is EXCLUSIVE
arr[1:4]   # [20, 30, 40] (index 1, 2, 3 - NOT 4)
arr[:3]    # [10, 20, 30] (first 3 elements)
arr[2:]    # [30, 40, 50] (from index 2 to end)
arr[:]     # [10, 20, 30, 40, 50] (copy of entire list)

# Slicing with step: arr[start:end:step]
arr[::2]   # [10, 30, 50] (every 2nd element)
arr[::-1]  # [50, 40, 30, 20, 10] (REVERSED! Very useful)
```

### 2.3 Modifying Lists

```python
arr = [1, 2, 3]

# Change an element
arr[0] = 100  # [100, 2, 3]

# Add to end - O(1)
arr.append(4)  # [100, 2, 3, 4]

# Add multiple to end - O(k) where k is items added
arr.extend([5, 6])  # [100, 2, 3, 4, 5, 6]

# Insert at specific position - O(n) because shifts elements
arr.insert(1, 999)  # [100, 999, 2, 3, 4, 5, 6]
#          ^ index  ^ value

# Remove last element - O(1)
last = arr.pop()  # Returns 6, arr is now [100, 999, 2, 3, 4, 5]

# Remove at specific index - O(n)
removed = arr.pop(1)  # Returns 999, arr is now [100, 2, 3, 4, 5]

# Remove first occurrence of value - O(n)
arr.remove(3)  # [100, 2, 4, 5] - removes the 3

# Clear all elements
arr.clear()  # []
```

### 2.4 Searching and Counting

```python
arr = [1, 2, 3, 2, 4, 2]

# Check if element exists - O(n)
if 3 in arr:
    print("Found!")

if 99 not in arr:
    print("Not found!")

# Find index of first occurrence - O(n)
idx = arr.index(2)  # Returns 1 (first 2 is at index 1)
# arr.index(99)  # ERROR if not found!

# Count occurrences - O(n)
count = arr.count(2)  # Returns 3 (there are three 2's)
```

### 2.5 Sorting

```python
arr = [3, 1, 4, 1, 5, 9, 2, 6]

# Sort in place (modifies original) - O(n log n)
arr.sort()  # arr is now [1, 1, 2, 3, 4, 5, 6, 9]

# Sort descending
arr.sort(reverse=True)  # [9, 6, 5, 4, 3, 2, 1, 1]

# Create new sorted list (original unchanged) - O(n log n)
original = [3, 1, 4]
new_sorted = sorted(original)  # new_sorted = [1, 3, 4], original unchanged

# Sort with custom key
words = ["banana", "pie", "apple"]
words.sort(key=len)  # Sort by length: ["pie", "apple", "banana"]

# Sort by custom function
pairs = [[1, 5], [3, 2], [2, 8]]
pairs.sort(key=lambda x: x[1])  # Sort by second element: [[3,2], [1,5], [2,8]]
```

### 2.6 Other Essential Operations

```python
arr = [3, 1, 4, 1, 5]

# Length - O(1)
length = len(arr)  # 5

# Sum - O(n)
total = sum(arr)  # 14

# Min and Max - O(n)
smallest = min(arr)  # 1
largest = max(arr)   # 5

# Reverse in place - O(n)
arr.reverse()  # [5, 1, 4, 1, 3]

# Create reversed copy
reversed_copy = arr[::-1]  # Original unchanged
```

### 2.7 Looping Through Arrays

```python
arr = [10, 20, 30, 40]

# Method 1: Loop through values only
for value in arr:
    print(value)  # 10, 20, 30, 40

# Method 2: Loop through indices
for i in range(len(arr)):
    print(f"Index {i}: {arr[i]}")

# Method 3: Loop through both (RECOMMENDED!)
for index, value in enumerate(arr):
    print(f"Index {index}: {value}")
# Output:
# Index 0: 10
# Index 1: 20
# Index 2: 30
# Index 3: 40
```

**What is `enumerate()`?**

`enumerate()` is a built-in Python function that gives you both the index AND value at each position. It's like getting the box number and what's inside at the same time.

```python
arr = ['a', 'b', 'c']
for i, val in enumerate(arr):
    print(i, val)
# 0 a
# 1 b
# 2 c
```

---

## 3. Basic Array Problems

Now let's solve actual LeetCode problems. For each problem:
1. We'll understand what's being asked
2. Think about approaches (bad → good)
3. Write the code
4. Analyze time/space complexity

---

### 3.1 Contains Duplicate

📌 **LeetCode Link**: [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

#### Problem Statement

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct (unique).

#### Examples

```
Example 1:
Input: nums = [1, 2, 3, 1]
Output: true
Explanation: 1 appears twice (at index 0 and index 3)

Example 2:
Input: nums = [1, 2, 3, 4]
Output: false
Explanation: All elements are unique

Example 3:
Input: nums = [1, 1, 1, 3, 3, 4, 3, 2, 4, 2]
Output: true
```

#### Approach 1: Brute Force (Nested Loops)

**Idea**: Compare every element with every other element.

```python
def containsDuplicate(nums):
    # For each element...
    for i in range(len(nums)):
        # Compare with every element after it
        for j in range(i + 1, len(nums)):
            if nums[i] == nums[j]:
                return True
    return False
```

**Why this is bad**:
- **Time**: O(n²) - For n elements, we do n × n comparisons
- **Space**: O(1) - No extra storage
- If array has 10,000 elements, we do ~100,000,000 comparisons!

#### Approach 2: Sorting First

**Idea**: If we sort the array, duplicates will be next to each other.

```python
def containsDuplicate(nums):
    nums.sort()  # Sort the array
    
    # Check adjacent elements
    for i in range(1, len(nums)):
        if nums[i] == nums[i - 1]:
            return True
    return False
```

**Analysis**:
- **Time**: O(n log n) - Sorting takes n log n, then one pass through array
- **Space**: O(1) or O(n) - Depends on sort implementation
- Better than brute force, but we can do better!

#### Approach 3: Using a Set (Optimal) ⭐

**What is a Set?**

A **set** is another data structure that:
1. **Cannot contain duplicates** - if you add something twice, it only stores once
2. **Has O(1) lookup** - checking if something exists is instant
3. **Has no order** - elements aren't in any particular sequence

```python
# Set basics
my_set = set()           # Empty set
my_set = {1, 2, 3}       # Set with values

my_set.add(4)            # Add element - O(1)
my_set.add(2)            # 2 already exists, set unchanged

print(2 in my_set)       # True - O(1) lookup!
print(len(my_set))       # 4

# Convert list to set (removes duplicates!)
arr = [1, 2, 2, 3, 3, 3]
unique = set(arr)        # {1, 2, 3}
```

**The Key Insight**:
If we convert a list to a set, duplicates are removed. So:
- If `len(list) != len(set(list))`, there were duplicates!

```python
def containsDuplicate(nums):
    return len(nums) != len(set(nums))
```

**That's it! One line!**

**Analysis**:
- **Time**: O(n) - Creating set requires looking at each element once
- **Space**: O(n) - Set stores up to n elements

**Alternative with Early Exit**:

If there's a duplicate early, we can stop immediately:

```python
def containsDuplicate(nums):
    seen = set()  # Track what we've seen
    
    for num in nums:
        if num in seen:      # Already saw this number!
            return True
        seen.add(num)        # Haven't seen it, add to set
    
    return False             # Checked all, no duplicates
```

**Walkthrough for [1, 2, 3, 1]**:
```
seen = {}

num = 1:
  Is 1 in seen? No
  Add 1: seen = {1}

num = 2:
  Is 2 in seen? No
  Add 2: seen = {1, 2}

num = 3:
  Is 3 in seen? No
  Add 3: seen = {1, 2, 3}

num = 1:
  Is 1 in seen? YES!
  Return True ✓
```

---

### 3.2 Missing Number

📌 **LeetCode Link**: [268. Missing Number](https://leetcode.com/problems/missing-number/)

#### Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

**Key Point**: The array has n numbers, but the range is 0 to n (which is n+1 possible numbers). So exactly one number is missing.

#### Examples

```
Example 1:
Input: nums = [3, 0, 1]
Output: 2
Explanation: n = 3 (length of array)
             Range is [0, 1, 2, 3]
             Array has [0, 1, 3]
             Missing: 2

Example 2:
Input: nums = [0, 1]
Output: 2
Explanation: n = 2, range is [0, 1, 2]
             Array has [0, 1]
             Missing: 2

Example 3:
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9, range is [0, 1, 2, ..., 9]
             Array has everything except 8
```

#### Approach 1: Sorting (Not Optimal)

```python
def missingNumber(nums):
    nums.sort()
    
    for i, num in enumerate(nums):
        if i != num:  # Index should equal value in sorted [0,1,2,3...]
            return i
    
    return len(nums)  # If loop completes, missing number is n
```

**Analysis**:
- **Time**: O(n log n) - Sorting dominates
- **Space**: O(1) or O(n)

#### Approach 2: Mathematical Formula (Optimal) ⭐

**The Key Insight**:

The sum of numbers from 0 to n is: `n × (n + 1) / 2`

This is a famous formula (sometimes called Gauss's formula).

**Why does this work?**
```
Sum of 0 to 4: 0 + 1 + 2 + 3 + 4 = 10
Using formula: 4 × 5 / 2 = 10 ✓
```

**The Trick**:
```
Expected sum (if no number missing) - Actual sum = Missing number
```

```python
def missingNumber(nums):
    n = len(nums)
    expected_sum = n * (n + 1) // 2  # Use // for integer division
    actual_sum = sum(nums)
    return expected_sum - actual_sum
```

**Analysis**:
- **Time**: O(n) - `sum()` iterates through array once
- **Space**: O(1) - Just storing a few variables

**Walkthrough for [3, 0, 1]**:
```
n = 3 (length of array)

Expected sum = 3 × 4 / 2 = 6
(This is 0 + 1 + 2 + 3 = 6)

Actual sum = 3 + 0 + 1 = 4

Missing = 6 - 4 = 2 ✓
```

#### Approach 3: XOR (Bit Manipulation)

**What is XOR?**

XOR (exclusive or) is a binary operation with these properties:
- `a ^ a = 0` (same numbers cancel out)
- `a ^ 0 = a` (XOR with zero gives same number)
- Order doesn't matter: `a ^ b ^ c = c ^ a ^ b`

```python
# XOR basics
print(5 ^ 5)   # 0 (same numbers cancel)
print(5 ^ 0)   # 5 (XOR with 0 gives same)
print(1 ^ 2 ^ 1)  # 2 (the 1s cancel, leaving 2)
```

**The Trick**:
XOR all indices (0 to n) with all values. Pairs cancel out, leaving the missing number!

```python
def missingNumber(nums):
    result = len(nums)  # Start with n
    
    for i, num in enumerate(nums):
        result ^= i ^ num
    
    return result
```

**Walkthrough for [3, 0, 1]**:
```
result = 3 (which is n)

i=0, num=3: result = 3 ^ 0 ^ 3 = 0
i=1, num=0: result = 0 ^ 1 ^ 0 = 1
i=2, num=1: result = 1 ^ 2 ^ 1 = 2

Final: 2 ✓
```

**Why it works**: We XOR indices 0,1,2,3 and values 3,0,1. All numbers 0,1,3 appear twice (as index or value) and cancel. Only 2 appears once (as index only), so it remains.

---

### 3.3 Find All Numbers Disappeared in an Array

📌 **LeetCode Link**: [448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

#### Problem Statement

Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do **not** appear in `nums`.

**Key Difference from Missing Number**: 
- Missing Number: exactly one missing
- This problem: could be multiple missing, and duplicates exist

#### Examples

```
Example 1:
Input: nums = [4, 3, 2, 7, 8, 2, 3, 1]
Output: [5, 6]
Explanation: n = 8, range is [1, 2, 3, 4, 5, 6, 7, 8]
             Present: 1, 2, 3, 4, 7, 8 (note: 2 and 3 appear twice)
             Missing: 5, 6

Example 2:
Input: nums = [1, 1]
Output: [2]
Explanation: n = 2, range is [1, 2]
             Present: 1 (appears twice)
             Missing: 2
```

#### Approach 1: Using a Set

```python
def findDisappearedNumbers(nums):
    num_set = set(nums)  # Remove duplicates
    result = []
    
    # Check each number from 1 to n
    for i in range(1, len(nums) + 1):
        if i not in num_set:
            result.append(i)
    
    return result
```

**Analysis**:
- **Time**: O(n) - Creating set is O(n), checking n numbers is O(n)
- **Space**: O(n) - Set stores up to n elements

**Walkthrough for [4, 3, 2, 7, 8, 2, 3, 1]**:
```
num_set = {1, 2, 3, 4, 7, 8}  # Duplicates removed
n = 8

Check 1: in set ✓
Check 2: in set ✓
Check 3: in set ✓
Check 4: in set ✓
Check 5: NOT in set → add to result
Check 6: NOT in set → add to result
Check 7: in set ✓
Check 8: in set ✓

Result: [5, 6]
```

#### Approach 2: In-Place Marking (Optimal Space) ⭐

**The Clever Trick**:
Use the array itself as a "hash table" by marking indices as "seen" using negative numbers.

**How it works**:
- Values are 1 to n, indices are 0 to n-1
- For each value `v`, mark the index `v-1` as negative
- After marking, positive values indicate missing numbers

```python
def findDisappearedNumbers(nums):
    # Mark seen numbers
    for num in nums:
        index = abs(num) - 1  # Convert value to index (1→0, 2→1, etc.)
        nums[index] = -abs(nums[index])  # Make negative (mark as seen)
    
    # Find indices with positive values (not seen)
    result = []
    for i in range(len(nums)):
        if nums[i] > 0:
            result.append(i + 1)  # Convert index back to value
    
    return result
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(1) - No extra storage (modifying input)

**Detailed Walkthrough for [4, 3, 2, 7, 8, 2, 3, 1]**:

```
Initial array (indices above):
Index:  0   1   2   3   4   5   6   7
Value:  4   3   2   7   8   2   3   1

Process each number:

num = 4 (at index 0):
  index = |4| - 1 = 3
  Mark index 3 as negative
  Array: [4, 3, 2, -7, 8, 2, 3, 1]
                 ^

num = 3 (at index 1):
  index = |3| - 1 = 2
  Mark index 2 as negative
  Array: [4, 3, -2, -7, 8, 2, 3, 1]
              ^

num = -2 (at index 2):
  index = |-2| - 1 = 1
  Mark index 1 as negative
  Array: [4, -3, -2, -7, 8, 2, 3, 1]
             ^

num = -7 (at index 3):
  index = |-7| - 1 = 6
  Mark index 6 as negative
  Array: [4, -3, -2, -7, 8, 2, -3, 1]
                               ^

num = 8 (at index 4):
  index = |8| - 1 = 7
  Mark index 7 as negative
  Array: [4, -3, -2, -7, 8, 2, -3, -1]
                                   ^

num = 2 (at index 5):
  index = |2| - 1 = 1
  Index 1 already negative, keep it negative
  Array: [4, -3, -2, -7, 8, 2, -3, -1]

num = -3 (at index 6):
  index = |-3| - 1 = 2
  Index 2 already negative, keep it negative
  Array: [4, -3, -2, -7, 8, 2, -3, -1]

num = -1 (at index 7):
  index = |-1| - 1 = 0
  Mark index 0 as negative
  Array: [-4, -3, -2, -7, 8, 2, -3, -1]
          ^

Final array:
Index:  0    1    2    3   4  5    6    7
Value: -4   -3   -2   -7   8  2   -3   -1

Positive values at indices 4 and 5!
Convert to values: 4+1=5, 5+1=6

Result: [5, 6] ✓
```

**Why use `abs()`?**
Because values might become negative after marking. We need the original value to calculate the index.

---

### 3.4 Two Sum

📌 **LeetCode Link**: [1. Two Sum](https://leetcode.com/problems/two-sum/)

This is THE most famous LeetCode problem. You will see variations of it everywhere!

#### Problem Statement

Given an array of integers `nums` and an integer `target`, return **indices** of the two numbers such that they add up to `target`.

**Assumptions**:
- Exactly one solution exists
- Can't use the same element twice
- Can return answer in any order

#### Examples

```
Example 1:
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
Explanation: nums[0] + nums[1] = 2 + 7 = 9

Example 2:
Input: nums = [3, 2, 4], target = 6
Output: [1, 2]
Explanation: nums[1] + nums[2] = 2 + 4 = 6

Example 3:
Input: nums = [3, 3], target = 6
Output: [0, 1]
Explanation: nums[0] + nums[1] = 3 + 3 = 6
```

#### Approach 1: Brute Force (Don't use in interviews!)

```python
def twoSum(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []  # No solution
```

**Analysis**:
- **Time**: O(n²) - Nested loops
- **Space**: O(1)

#### Approach 2: Hash Map (Optimal) ⭐

**What is a Hash Map (Dictionary)?**

A **hash map** (called `dict` in Python) stores key-value pairs with O(1) lookup:

```python
# Dictionary basics
my_dict = {}                    # Empty dictionary
my_dict = {"apple": 1, "banana": 2}  # With values

my_dict["cherry"] = 3           # Add/update - O(1)
print(my_dict["apple"])         # Access - O(1)
print("apple" in my_dict)       # Check if key exists - O(1)
```

**The Key Insight**:

For each number, there's exactly ONE other number that would work:
```
target - current_number = the_number_we_need
```

So for each number, we check: "Have I seen the number I need before?"

```python
def twoSum(nums, target):
    seen = {}  # value -> index
    
    for i, num in enumerate(nums):
        complement = target - num  # The number we need
        
        if complement in seen:
            return [seen[complement], i]
        
        seen[num] = i  # Store current number and its index
    
    return []
```

**Analysis**:
- **Time**: O(n) - Single pass through array
- **Space**: O(n) - Hash map stores up to n elements

**Why build the hash map while iterating (not before)?**

To avoid using the same element twice!

```python
# If we pre-built: nums = [3], target = 6
# seen = {3: 0}
# For num=3: complement = 3, which IS in seen... but it's the same element!

# By building while iterating:
# seen = {}
# For num=3: complement = 3, not in seen yet, add 3
# We correctly find no solution with single element
```

**Detailed Walkthrough for [2, 7, 11, 15], target = 9**:

```
seen = {}

i=0, num=2:
  complement = 9 - 2 = 7
  Is 7 in seen? No (seen is empty)
  Add to seen: seen = {2: 0}

i=1, num=7:
  complement = 9 - 7 = 2
  Is 2 in seen? YES! At index 0
  Return [0, 1] ✓
```

**Detailed Walkthrough for [3, 2, 4], target = 6**:

```
seen = {}

i=0, num=3:
  complement = 6 - 3 = 3
  Is 3 in seen? No
  seen = {3: 0}

i=1, num=2:
  complement = 6 - 2 = 4
  Is 4 in seen? No
  seen = {3: 0, 2: 1}

i=2, num=4:
  complement = 6 - 4 = 2
  Is 2 in seen? YES! At index 1
  Return [1, 2] ✓
```

---

## 4. Array Techniques

Now we'll explore more advanced techniques used in harder array problems.

---

### 4.1 How Many Numbers Are Smaller Than the Current Number

📌 **LeetCode Link**: [1365. How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/)

#### Problem Statement

Given the array `nums`, for each `nums[i]` find out how many numbers in the array are smaller than it. Return the answer in an array.

#### Examples

```
Example 1:
Input: nums = [8, 1, 2, 2, 3]
Output: [4, 0, 1, 1, 3]

Explanation:
- For nums[0]=8: 4 numbers are smaller (1, 2, 2, 3)
- For nums[1]=1: 0 numbers are smaller
- For nums[2]=2: 1 number is smaller (1)
- For nums[3]=2: 1 number is smaller (1)
- For nums[4]=3: 3 numbers are smaller (1, 2, 2)

Example 2:
Input: nums = [6, 5, 4, 8]
Output: [2, 1, 0, 3]
```

#### Approach: Sort + Dictionary

**The Key Insight**:
If we sort the array, each number's **position (index) in the sorted array tells us exactly how many numbers are smaller than it!**

```
Original: [8, 1, 2, 2, 3]
Sorted:   [1, 2, 2, 3, 8]
Index:     0  1  2  3  4

- 1 is at index 0 → 0 numbers smaller
- 2 is at index 1 → 1 number smaller (but we have duplicate 2s!)
- 3 is at index 3 → 3 numbers smaller
- 8 is at index 4 → 4 numbers smaller
```

**Handling Duplicates**: For duplicates, we only use the FIRST occurrence's index.

```python
def smallerNumbersThanCurrent(nums):
    # Step 1: Create sorted version
    sorted_nums = sorted(nums)
    
    # Step 2: Build dictionary (number -> count of smaller numbers)
    count_dict = {}
    for i, num in enumerate(sorted_nums):
        if num not in count_dict:  # Only first occurrence!
            count_dict[num] = i
    
    # Step 3: Build result using dictionary
    return [count_dict[num] for num in nums]
```

**What is List Comprehension?**

The last line uses **list comprehension**, a shortcut for building lists:

```python
# These two are equivalent:

# Regular way:
result = []
for num in nums:
    result.append(count_dict[num])

# List comprehension (shorter):
result = [count_dict[num] for num in nums]
```

**Analysis**:
- **Time**: O(n log n) - Sorting dominates
- **Space**: O(n) - Sorted array and dictionary

**Detailed Walkthrough for [8, 1, 2, 2, 3]**:

```
Step 1: Sort
sorted_nums = [1, 2, 2, 3, 8]

Step 2: Build dictionary (only first occurrences)
i=0, num=1: 1 not in dict → count_dict = {1: 0}
i=1, num=2: 2 not in dict → count_dict = {1: 0, 2: 1}
i=2, num=2: 2 already in dict → SKIP
i=3, num=3: 3 not in dict → count_dict = {1: 0, 2: 1, 3: 3}
i=4, num=8: 8 not in dict → count_dict = {1: 0, 2: 1, 3: 3, 8: 4}

Step 3: Build result for original [8, 1, 2, 2, 3]
- 8 → count_dict[8] = 4
- 1 → count_dict[1] = 0
- 2 → count_dict[2] = 1
- 2 → count_dict[2] = 1
- 3 → count_dict[3] = 3

Result: [4, 0, 1, 1, 3] ✓
```

---

### 4.2 Minimum Time Visiting All Points

📌 **LeetCode Link**: [1266. Minimum Time Visiting All Points](https://leetcode.com/problems/minimum-time-visiting-all-points/)

#### Problem Statement

On a 2D plane, there are `n` points. You start at the first point and visit points in order. In one second, you can move:
- Horizontally by 1 unit (left/right)
- Vertically by 1 unit (up/down)  
- Diagonally by 1 unit (combines horizontal + vertical)

Return the minimum time to visit all points.

#### Understanding the Grid

```
        y
        ↑
    4   |       · (3,4)
    3   |     ·
    2   |   ·
    1   | · (1,1)
    0   +----------→ x
        0 1 2 3 4

Moving from (1,1) to (3,4):
- Diagonal: (1,1)→(2,2)→(3,3) = 2 moves
- Then vertical: (3,3)→(3,4) = 1 move
- Total: 3 moves
```

#### The Key Insight

**Diagonal moves are FREE bonus moves!** When you move diagonally, you cover both X and Y distance at the same time.

The minimum time between two points is simply:
```
max(|x2 - x1|, |y2 - y1|)
```

**Why?**
- Move diagonally as much as possible (covers both X and Y)
- Then move straight (horizontal or vertical) for remaining distance
- The larger difference determines total time

```
From (0,0) to (3,5):
- X difference: |3-0| = 3
- Y difference: |5-0| = 5
- Move diagonal 3 times: (0,0)→(1,1)→(2,2)→(3,3)
- Move vertical 2 times: (3,3)→(3,4)→(3,5)
- Total: max(3, 5) = 5 ✓
```

```python
def minTimeToVisitAllPoints(points):
    total_time = 0
    
    for i in range(1, len(points)):
        x1, y1 = points[i - 1]  # Previous point
        x2, y2 = points[i]      # Current point
        
        # Time = maximum of X and Y differences
        time = max(abs(x2 - x1), abs(y2 - y1))
        total_time += time
    
    return total_time
```

**What is `abs()`?**

`abs()` returns the **absolute value** (always positive):
```python
abs(5)   # 5
abs(-5)  # 5
abs(0)   # 0
```

**Analysis**:
- **Time**: O(n) - Visit each point once
- **Space**: O(1) - Only storing total_time

**Walkthrough for [[1,1], [3,4], [-1,0]]**:

```
Point 1 to Point 2: (1,1) to (3,4)
  X diff = |3-1| = 2
  Y diff = |4-1| = 3
  Time = max(2, 3) = 3

Point 2 to Point 3: (3,4) to (-1,0)
  X diff = |-1-3| = 4
  Y diff = |0-4| = 4
  Time = max(4, 4) = 4

Total: 3 + 4 = 7 ✓
```

---

### 4.3 Spiral Matrix (Medium)

📌 **LeetCode Link**: [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

#### Problem Statement

Given an `m x n` matrix, return all elements in **spiral order** (clockwise from outside to inside).

#### Visual Understanding

```
Input matrix:
[1,  2,  3 ]
[4,  5,  6 ]
[7,  8,  9 ]

Spiral path:
→ → →
    ↓
← ← ↓
↑   ↓
↑ ← ←

Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

#### The Pattern

We peel off layers like an onion:
1. **Top row**: Go right →
2. **Right column**: Go down ↓
3. **Bottom row**: Go left ←
4. **Left column**: Go up ↑
5. Repeat for inner layers

#### Approach: Pop Rows and Columns

**What is `pop()`?**

`pop()` removes and returns an element:
```python
arr = [1, 2, 3]
arr.pop()    # Returns 3, arr is now [1, 2]
arr.pop(0)   # Returns 1, arr is now [2]
```

For a matrix (list of lists):
```python
matrix = [[1,2,3], [4,5,6], [7,8,9]]
matrix.pop(0)  # Returns [1,2,3], removes first row
```

```python
def spiralOrder(matrix):
    result = []
    
    while matrix:
        # Step 1: Take entire top row (left to right)
        result += matrix.pop(0)
        
        # Step 2: Take last element of each remaining row (top to bottom)
        if matrix and matrix[0]:
            for row in matrix:
                result.append(row.pop())
        
        # Step 3: Take entire bottom row reversed (right to left)
        if matrix:
            result += matrix.pop()[::-1]
        
        # Step 4: Take first element of each remaining row (bottom to top)
        if matrix and matrix[0]:
            for row in matrix[::-1]:  # Reverse order of rows
                result.append(row.pop(0))
    
    return result
```

**What does `[::-1]` mean?**

It creates a **reversed copy**:
```python
arr = [1, 2, 3]
arr[::-1]  # [3, 2, 1] - original unchanged
```

**What does `+=` do with lists?**

It extends the list:
```python
result = [1, 2]
result += [3, 4]  # result is now [1, 2, 3, 4]
```

**Analysis**:
- **Time**: O(m × n) - Visit each element once
- **Space**: O(1) - Excluding output (we modify input)

**Detailed Walkthrough**:

```
Initial:
[[1, 2, 3],
 [4, 5, 6],
 [7, 8, 9]]

--- Iteration 1 ---

Step 1: Pop top row
result += [1, 2, 3]
Matrix: [[4, 5, 6], [7, 8, 9]]
result: [1, 2, 3]

Step 2: Pop last of each row
Pop 6 from [4, 5, 6] → [4, 5]
Pop 9 from [7, 8, 9] → [7, 8]
Matrix: [[4, 5], [7, 8]]
result: [1, 2, 3, 6, 9]

Step 3: Pop bottom row reversed
Pop [7, 8], reverse to [8, 7]
result += [8, 7]
Matrix: [[4, 5]]
result: [1, 2, 3, 6, 9, 8, 7]

Step 4: Pop first of each row (bottom to top)
Only one row: pop 4 from [4, 5] → [5]
Matrix: [[5]]
result: [1, 2, 3, 6, 9, 8, 7, 4]

--- Iteration 2 ---

Step 1: Pop top row
result += [5]
Matrix: []
result: [1, 2, 3, 6, 9, 8, 7, 4, 5]

Matrix is empty, loop ends.

Final: [1, 2, 3, 6, 9, 8, 7, 4, 5] ✓
```

---

### 4.4 Number of Islands (Medium)

📌 **LeetCode Link**: [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

#### Problem Statement

Given an `m x n` 2D grid of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and formed by connecting adjacent lands **horizontally or vertically** (not diagonally).

#### Examples

```
Example 1:
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
(All the 1s are connected)

Example 2:
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
(Three separate groups of 1s)
```

#### Understanding the Problem

Think of it like a map where:
- `'1'` = land
- `'0'` = water

Islands are groups of `'1'`s that touch each other horizontally or vertically.

```
Example 2 visualized:
🟢🟢⬜⬜⬜     Island 1: top-left 🟢s
🟢🟢⬜⬜⬜
⬜⬜🔵⬜⬜     Island 2: middle 🔵
⬜⬜⬜🔴🔴     Island 3: bottom-right 🔴s
```

#### The Approach: Flood Fill

**The Idea**:
1. Scan the grid cell by cell
2. When we find a `'1'` (land), it's a new island!
3. Use BFS or DFS to "flood" the entire island, marking all connected land as visited
4. Continue scanning for more islands

**What is BFS (Breadth-First Search)?**

BFS explores all neighbors at the current level before going deeper. It uses a **queue** (first-in, first-out).

```
Start at A, BFS order: A → B → C → D → E → F

    A
   / \
  B   C
 / \   \
D   E   F
```

**What is DFS (Depth-First Search)?**

DFS goes as deep as possible before backtracking. It uses a **stack** (last-in, first-out) or recursion.

```
Start at A, DFS order: A → B → D → E → C → F

    A
   / \
  B   C
 / \   \
D   E   F
```

**What is a Queue?**

A queue is like a line at a store - first person in line gets served first (FIFO: First In, First Out).

```python
from collections import deque

queue = deque()       # Create empty queue
queue.append(1)       # Add to back: [1]
queue.append(2)       # Add to back: [1, 2]
queue.append(3)       # Add to back: [1, 2, 3]
front = queue.popleft()  # Remove from front: returns 1, queue is [2, 3]
```

#### BFS Solution

```python
from collections import deque

def numIslands(grid):
    if not grid:
        return 0
    
    rows = len(grid)
    cols = len(grid[0])
    visited = set()
    islands = 0
    
    def bfs(r, c):
        """Flood fill the island starting from (r, c)"""
        queue = deque()
        queue.append((r, c))
        visited.add((r, c))
        
        # 4 directions: down, up, right, left
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        
        while queue:
            row, col = queue.popleft()
            
            for dr, dc in directions:
                new_r = row + dr
                new_c = col + dc
                
                # Check: in bounds, is land, not visited
                if (0 <= new_r < rows and 
                    0 <= new_c < cols and 
                    grid[new_r][new_c] == "1" and 
                    (new_r, new_c) not in visited):
                    
                    queue.append((new_r, new_c))
                    visited.add((new_r, new_c))
    
    # Scan entire grid
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1" and (r, c) not in visited:
                bfs(r, c)      # Flood fill this island
                islands += 1   # Count it
    
    return islands
```

**What is a Tuple?**

A **tuple** is like a list but **immutable** (can't change after creation):
```python
point = (3, 4)       # Create tuple
x, y = point         # Unpack: x=3, y=4
point[0]             # Access: 3
# point[0] = 5       # ERROR! Can't modify
```

Tuples can be added to sets (lists cannot):
```python
my_set = set()
my_set.add((1, 2))   # OK - tuple
# my_set.add([1, 2]) # ERROR - list
```

**Analysis**:
- **Time**: O(m × n) - Visit each cell once
- **Space**: O(m × n) - For visited set

**Detailed Walkthrough for Example 2**:

```
Grid:
["1","1","0","0","0"]
["1","1","0","0","0"]
["0","0","1","0","0"]
["0","0","0","1","1"]

Scanning...

(0,0) is "1" and not visited:
  → BFS from (0,0)
  → Queue: [(0,0)]
  → Visit (0,0), check neighbors
  → Add (1,0) and (0,1) to queue
  → Process (1,0), add (1,1)
  → Process (0,1), (1,1) already queued
  → Process (1,1), neighbors are visited or water
  → BFS done, visited = {(0,0), (0,1), (1,0), (1,1)}
  → islands = 1

(0,1), (1,0), (1,1) already visited, skip

(2,2) is "1" and not visited:
  → BFS from (2,2)
  → Only (2,2) is land in this area
  → visited adds (2,2)
  → islands = 2

(3,3) is "1" and not visited:
  → BFS from (3,3)
  → Process (3,3), add (3,4)
  → Process (3,4), no new land neighbors
  → visited adds (3,3), (3,4)
  → islands = 3

Final: 3 ✓
```

#### DFS Solution (Alternative)

```python
def numIslands(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    islands = 0
    
    def dfs(r, c):
        # Base cases: out of bounds or water
        if r < 0 or r >= rows or c < 0 or c >= cols:
            return
        if grid[r][c] != "1":
            return
        
        # Mark as visited by changing to "0"
        grid[r][c] = "0"
        
        # Explore all 4 directions
        dfs(r + 1, c)  # down
        dfs(r - 1, c)  # up
        dfs(r, c + 1)  # right
        dfs(r, c - 1)  # left
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1":
                dfs(r, c)
                islands += 1
    
    return islands
```

**Key Difference**: DFS modifies the grid directly (changing `"1"` to `"0"`) instead of using a visited set. This saves space but modifies input.

---

## 5. Two Pointers Pattern

The **two pointer** technique uses two index variables that move through data, typically:
- From opposite ends moving inward
- At different speeds (slow/fast)
- From the same starting point

### When to Use Two Pointers

- Array is **sorted** (or should be sorted)
- Looking for **pairs** that satisfy a condition
- Need to **compare** elements from different positions
- Trying to **reduce nested loops** from O(n²) to O(n)

---

### 5.1 Best Time to Buy and Sell Stock

📌 **LeetCode Link**: [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

#### Problem Statement

Given an array `prices` where `prices[i]` is the stock price on day `i`, find the maximum profit from ONE buy and ONE sell. You must buy before you sell.

Return 0 if no profit is possible.

#### Examples

```
Example 1:
Input: prices = [7, 1, 5, 3, 6, 4]
Output: 5
Explanation: Buy on day 2 (price=1), sell on day 5 (price=6)
             Profit = 6 - 1 = 5

Example 2:
Input: prices = [7, 6, 4, 3, 1]
Output: 0
Explanation: Prices only decrease, no profit possible
```

#### Understanding the Problem

- We want to BUY LOW, SELL HIGH
- Buy must happen BEFORE sell (can't time travel!)
- We can only make ONE transaction

#### Brute Force Approach (Don't use!)

```python
def maxProfit(prices):
    max_profit = 0
    for i in range(len(prices)):         # Buy day
        for j in range(i + 1, len(prices)):  # Sell day (must be after buy)
            profit = prices[j] - prices[i]
            max_profit = max(max_profit, profit)
    return max_profit
```
- **Time**: O(n²) - Too slow!

#### Two Pointer Approach (Optimal) ⭐

**The Idea**: 
- Keep track of the minimum price seen so far (potential buy point)
- At each day, calculate profit if we sold today
- Track maximum profit

```python
def maxProfit(prices):
    left = 0   # Buy pointer (points to minimum price so far)
    right = 1  # Sell pointer (current day)
    max_profit = 0
    
    while right < len(prices):
        # Is this a profitable trade?
        if prices[left] < prices[right]:
            profit = prices[right] - prices[left]
            max_profit = max(max_profit, profit)
        else:
            # Found new minimum! Move buy pointer here
            left = right
        
        right += 1
    
    return max_profit
```

**Alternative (Single Variable)**:

```python
def maxProfit(prices):
    min_price = float('inf')  # Infinity - any price will be smaller
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)        # Track lowest price
        profit = price - min_price                # Profit if sold today
        max_profit = max(max_profit, profit)     # Track best profit
    
    return max_profit
```

**What is `float('inf')`?**

It represents **infinity** in Python:
```python
float('inf')   # Positive infinity
float('-inf')  # Negative infinity

# Any number is less than infinity
5 < float('inf')   # True
-1000 < float('inf')  # True
```

**Analysis**:
- **Time**: O(n) - Single pass
- **Space**: O(1) - Few variables

**Detailed Walkthrough for [7, 1, 5, 3, 6, 4]**:

```
left=0 (price 7), right=1 (price 1)
  7 > 1 (not profitable to buy at 7, sell at 1)
  Found new minimum! Move left to 1
  left=1

right=2 (price 5)
  prices[1]=1 < prices[2]=5 ✓
  profit = 5 - 1 = 4
  max_profit = 4

right=3 (price 3)
  1 < 3 ✓
  profit = 3 - 1 = 2
  max_profit stays 4

right=4 (price 6)
  1 < 6 ✓
  profit = 6 - 1 = 5
  max_profit = 5 ✓

right=5 (price 4)
  1 < 4 ✓
  profit = 4 - 1 = 3
  max_profit stays 5

Final: 5 ✓
```

---

### 5.2 Squares of a Sorted Array

📌 **LeetCode Link**: [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

#### Problem Statement

Given an integer array `nums` sorted in **non-decreasing order**, return an array of the squares of each number sorted in non-decreasing order.

#### Examples

```
Example 1:
Input: nums = [-4, -1, 0, 3, 10]
Output: [0, 1, 9, 16, 100]

Example 2:
Input: nums = [-7, -3, 2, 3, 11]
Output: [4, 9, 9, 49, 121]
```

#### The Challenge

Squaring negative numbers makes them positive:
```
(-4)² = 16
(-1)² = 1
3² = 9
```

So after squaring, the order changes! `[-4, -1, 0, 3, 10]` becomes `[16, 1, 0, 9, 100]` which isn't sorted.

#### Approach 1: Square and Sort

```python
def sortedSquares(nums):
    return sorted([x * x for x in nums])
```

- **Time**: O(n log n) - Sorting
- **Space**: O(n)

#### Approach 2: Two Pointers (Optimal) ⭐

**The Key Insight**:

After squaring, the **largest** values are at the **ends** of the array (because of absolute values).

```
Original: [-4, -1, 0, 3, 10]
           ↑              ↑
         left          right
         
(-4)² = 16    vs    10² = 100
100 is larger, so it goes at the END of result
```

**Strategy**: 
- Use two pointers at both ends
- Compare squared values
- Put the larger one at the END of result
- Move the pointer that had the larger value

```python
def sortedSquares(nums):
    n = len(nums)
    result = [0] * n       # Pre-allocate result array
    left = 0               # Start of array
    right = n - 1          # End of array
    position = n - 1       # Fill result from the end
    
    while left <= right:
        left_sq = nums[left] ** 2
        right_sq = nums[right] ** 2
        
        if left_sq > right_sq:
            result[position] = left_sq
            left += 1      # Move left pointer right
        else:
            result[position] = right_sq
            right -= 1     # Move right pointer left
        
        position -= 1      # Move to next position (going backward)
    
    return result
```

**What is `**`?**

It's the **exponent** (power) operator:
```python
2 ** 3    # 2³ = 8
5 ** 2    # 5² = 25
nums[i] ** 2  # Square of nums[i]
```

**Analysis**:
- **Time**: O(n) - Single pass
- **Space**: O(n) - Result array

**Detailed Walkthrough for [-4, -1, 0, 3, 10]**:

```
result = [_, _, _, _, _]
left=0 (-4), right=4 (10), position=4

Iteration 1:
  left_sq = 16, right_sq = 100
  100 > 16, place 100 at position 4
  result = [_, _, _, _, 100]
  right = 3, position = 3

Iteration 2:
  left=0 (-4), right=3 (3)
  left_sq = 16, right_sq = 9
  16 > 9, place 16 at position 3
  result = [_, _, _, 16, 100]
  left = 1, position = 2

Iteration 3:
  left=1 (-1), right=3 (3)
  left_sq = 1, right_sq = 9
  9 > 1, place 9 at position 2
  result = [_, _, 9, 16, 100]
  right = 2, position = 1

Iteration 4:
  left=1 (-1), right=2 (0)
  left_sq = 1, right_sq = 0
  1 > 0, place 1 at position 1
  result = [_, 1, 9, 16, 100]
  left = 2, position = 0

Iteration 5:
  left=2 (0), right=2 (0)
  left_sq = 0, right_sq = 0
  0 >= 0 (else branch), place 0 at position 0
  result = [0, 1, 9, 16, 100]
  right = 1

left > right, loop ends

Final: [0, 1, 9, 16, 100] ✓
```

---

### 5.3 Three Sum (Medium)

📌 **LeetCode Link**: [15. 3Sum](https://leetcode.com/problems/3sum/)

#### Problem Statement

Given an integer array `nums`, return all the **unique triplets** `[nums[i], nums[j], nums[k]]` such that:
- `i != j != k` (different indices)
- `nums[i] + nums[j] + nums[k] == 0`

The solution set must not contain duplicate triplets.

#### Examples

```
Example 1:
Input: nums = [-1, 0, 1, 2, -1, -4]
Output: [[-1, -1, 2], [-1, 0, 1]]
Explanation:
  nums[0] + nums[1] + nums[2] = -1 + 0 + 1 = 0
  nums[1] + nums[2] + nums[4] = 0 + 1 + -1 = 0
  nums[0] + nums[3] + nums[4] = -1 + 2 + -1 = 0
  The distinct triplets are [-1,0,1] and [-1,-1,2]

Example 2:
Input: nums = [0, 1, 1]
Output: []
No triplet sums to 0

Example 3:
Input: nums = [0, 0, 0]
Output: [[0, 0, 0]]
```

#### Why This is Harder Than Two Sum

1. Three numbers instead of two
2. Must avoid **duplicate** triplets
3. Return actual values, not indices

#### The Strategy

1. **Sort the array** first (enables two-pointer technique + duplicate handling)
2. **Fix one number** (loop through array)
3. Use **two pointers** to find pairs that sum to the negative of the fixed number
4. **Skip duplicates** to avoid duplicate triplets

```python
def threeSum(nums):
    nums.sort()  # Sort first!
    result = []
    
    for i in range(len(nums) - 2):  # -2 because we need room for j and k
        # Skip duplicates for i
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        # Early termination: if first number is positive, no solution
        # (can't sum three positive numbers to get zero)
        if nums[i] > 0:
            break
        
        # Two pointer search
        left = i + 1
        right = len(nums) - 1
        target = -nums[i]  # We need nums[left] + nums[right] = target
        
        ```python
        while left < right:
            current_sum = nums[left] + nums[right]
            
            if current_sum < target:
                # Sum too small, need larger numbers
                left += 1
            elif current_sum > target:
                # Sum too large, need smaller numbers
                right -= 1
            else:
                # Found a triplet!
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates for left pointer
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                # Skip duplicates for right pointer
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                
                # Move both pointers
                left += 1
                right -= 1
        
    return result
```

**What is `continue`?**

`continue` skips the rest of the current loop iteration and moves to the next:
```python
for i in range(5):
    if i == 2:
        continue  # Skip printing 2
    print(i)
# Output: 0, 1, 3, 4
```

**What is `break`?**

`break` exits the loop entirely:
```python
for i in range(5):
    if i == 3:
        break  # Stop the loop
    print(i)
# Output: 0, 1, 2
```

**Analysis**:
- **Time**: O(n²) - Outer loop O(n), inner two-pointer O(n)
- **Space**: O(1) or O(n) depending on sort implementation

**Why Sorting Helps with Duplicates**:

After sorting, duplicate values are next to each other:
```
Before: [-1, 0, 1, 2, -1, -4]
After:  [-4, -1, -1, 0, 1, 2]
```

Now we can easily skip duplicates with:
```python
if i > 0 and nums[i] == nums[i - 1]:
    continue  # Same as previous, skip!
```

**Detailed Walkthrough for [-1, 0, 1, 2, -1, -4]**:

```
After sorting: [-4, -1, -1, 0, 1, 2]
                 0    1    2   3  4  5  (indices)

=== i = 0, nums[i] = -4 ===
target = -(-4) = 4
left = 1, right = 5

  nums[1] + nums[5] = -1 + 2 = 1 < 4
  left = 2
  
  nums[2] + nums[5] = -1 + 2 = 1 < 4
  left = 3
  
  nums[3] + nums[5] = 0 + 2 = 2 < 4
  left = 4
  
  nums[4] + nums[5] = 1 + 2 = 3 < 4
  left = 5
  
  left >= right, exit while loop

No triplets with -4.

=== i = 1, nums[i] = -1 ===
target = -(-1) = 1
left = 2, right = 5

  nums[2] + nums[5] = -1 + 2 = 1 == 1 ✓
  Found triplet: [-1, -1, 2]
  result = [[-1, -1, 2]]
  
  Skip duplicate lefts: nums[2] == nums[3]? -1 == 0? No
  Skip duplicate rights: nums[5] == nums[4]? 2 == 1? No
  
  left = 3, right = 4
  
  nums[3] + nums[4] = 0 + 1 = 1 == 1 ✓
  Found triplet: [-1, 0, 1]
  result = [[-1, -1, 2], [-1, 0, 1]]
  
  left = 4, right = 3
  left >= right, exit while loop

=== i = 2, nums[i] = -1 ===
nums[2] == nums[1]? -1 == -1? YES!
Skip (duplicate of previous i)

=== i = 3, nums[i] = 0 ===
target = 0
left = 4, right = 5

  nums[4] + nums[5] = 1 + 2 = 3 > 0
  right = 4
  
  left >= right, exit while loop

No triplets with 0.

=== i = 4 ===
i >= len(nums) - 2, loop ends

Final: [[-1, -1, 2], [-1, 0, 1]] ✓
```

---

### 5.4 Longest Mountain in Array

📌 **LeetCode Link**: [845. Longest Mountain in Array](https://leetcode.com/problems/longest-mountain-in-array/)

#### Problem Statement

You may recall that an array `arr` is a **mountain array** if and only if:
- `arr.length >= 3`
- There exists some index `i` (0-indexed) with `0 < i < arr.length - 1` such that:
  - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]` (strictly increasing)
  - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]` (strictly decreasing)

Given an integer array `arr`, return the length of the longest subarray which is a mountain. Return 0 if there is no mountain subarray.

#### Visual Understanding

```
A mountain looks like this:

        /\
       /  \
      /    \
     /      \
    
It goes UP to a peak, then DOWN.
```

#### Examples

```
Example 1:
Input: arr = [2, 1, 4, 7, 3, 2, 5]
Output: 5
Explanation: The longest mountain is [1, 4, 7, 3, 2] with length 5

        7
       / \
      4   3
     /     \
    1       2

Example 2:
Input: arr = [2, 2, 2]
Output: 0
Explanation: No mountain (no strictly increasing then decreasing)
```

#### The Strategy

1. Find each **peak** (element greater than both neighbors)
2. From each peak, expand **left** (while increasing) and **right** (while decreasing)
3. Calculate mountain length and track the maximum

```python
def longestMountain(arr):
    n = len(arr)
    if n < 3:
        return 0
    
    longest = 0
    
    for i in range(1, n - 1):  # Peaks can't be at edges
        # Check if current position is a peak
        if arr[i] > arr[i - 1] and arr[i] > arr[i + 1]:
            # Expand left (go down the increasing slope)
            left = i
            while left > 0 and arr[left] > arr[left - 1]:
                left -= 1
            
            # Expand right (go down the decreasing slope)
            right = i
            while right < n - 1 and arr[right] > arr[right + 1]:
                right += 1
            
            # Calculate length
            length = right - left + 1
            longest = max(longest, length)
    
    return longest
```

**Analysis**:
- **Time**: O(n) on average (each element visited at most twice)
- **Space**: O(1)

**Detailed Walkthrough for [2, 1, 4, 7, 3, 2, 5]**:

```
Array: [2, 1, 4, 7, 3, 2, 5]
Index:  0  1  2  3  4  5  6

=== i = 1, arr[1] = 1 ===
Is 1 > 2 (left)? No
Not a peak, skip.

=== i = 2, arr[2] = 4 ===
Is 4 > 1 (left)? Yes
Is 4 > 7 (right)? No
Not a peak, skip.

=== i = 3, arr[3] = 7 ===
Is 7 > 4 (left)? Yes
Is 7 > 3 (right)? Yes
IT'S A PEAK! 🏔️

Expand left from i=3:
  left = 3
  arr[3]=7 > arr[2]=4? Yes, left = 2
  arr[2]=4 > arr[1]=1? Yes, left = 1
  arr[1]=1 > arr[0]=2? No, stop
  Final left = 1

Expand right from i=3:
  right = 3
  arr[3]=7 > arr[4]=3? Yes, right = 4
  arr[4]=3 > arr[5]=2? Yes, right = 5
  arr[5]=2 > arr[6]=5? No, stop
  Final right = 5

Length = 5 - 1 + 1 = 5
longest = 5

=== i = 4, arr[4] = 3 ===
Is 3 > 7 (left)? No
Not a peak, skip.

=== i = 5, arr[5] = 2 ===
Is 2 > 3 (left)? No
Not a peak, skip.

Final: 5 ✓
```

---

## 6. Sliding Window Pattern

The **sliding window** technique maintains a "window" (a contiguous subarray) that slides through the data. It's perfect for:
- Finding subarrays with specific properties
- Maximum/minimum values in subarrays
- Substring problems

### Two Types of Sliding Windows

| Type | Window Size | When to Use |
|------|-------------|-------------|
| **Fixed** | Constant size k | "Find max sum of k elements" |
| **Dynamic** | Changes based on condition | "Find shortest subarray with sum ≥ target" |

---

### 6.1 Contains Duplicate II

📌 **LeetCode Link**: [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

#### Problem Statement

Given an integer array `nums` and an integer `k`, return `true` if there are two **distinct indices** `i` and `j` in the array such that:
- `nums[i] == nums[j]` (same value)
- `|i - j| <= k` (indices are at most k apart)

#### Examples

```
Example 1:
Input: nums = [1, 2, 3, 1], k = 3
Output: true
Explanation: nums[0] = nums[3] = 1, and |0 - 3| = 3 <= 3

Example 2:
Input: nums = [1, 0, 1, 1], k = 1
Output: true
Explanation: nums[2] = nums[3] = 1, and |2 - 3| = 1 <= 1

Example 3:
Input: nums = [1, 2, 3, 1, 2, 3], k = 2
Output: false
Explanation: The only duplicates are 3 apart (indices 0&3, 1&4, 2&5)
```

#### The Strategy

Maintain a **sliding window of size k** using a set:
- If we find a duplicate within the window → return true
- If window exceeds size k → remove the oldest element

```python
def containsNearbyDuplicate(nums, k):
    window = set()  # Current window of elements
    
    for i, num in enumerate(nums):
        # Check if num already exists in window
        if num in window:
            return True
        
        # Add current number to window
        window.add(num)
        
        # If window is too big, remove oldest element
        if len(window) > k:
            window.remove(nums[i - k])
    
    return False
```

**Analysis**:
- **Time**: O(n) - Single pass
- **Space**: O(k) - Window stores at most k elements

**Detailed Walkthrough for [1, 2, 3, 1], k = 3**:

```
Window can hold at most k = 3 elements

i=0, num=1:
  Is 1 in window? No (window is empty)
  Add 1: window = {1}
  len(1) <= 3, no removal needed

i=1, num=2:
  Is 2 in window? No
  Add 2: window = {1, 2}
  len(2) <= 3, no removal needed

i=2, num=3:
  Is 3 in window? No
  Add 3: window = {1, 2, 3}
  len(3) <= 3, no removal needed

i=3, num=1:
  Is 1 in window? YES! ✓
  Return true

The 1 at index 3 found the 1 at index 0 within the window.
|3 - 0| = 3 <= k = 3 ✓
```

**Walkthrough for [1, 2, 3, 1, 2, 3], k = 2**:

```
i=0, num=1: window = {1}
i=1, num=2: window = {1, 2}
i=2, num=3: 
  window = {1, 2, 3}
  len(3) > k=2, remove nums[2-2] = nums[0] = 1
  window = {2, 3}

i=3, num=1:
  Is 1 in {2, 3}? No
  window = {2, 3, 1}
  len(3) > k=2, remove nums[3-2] = nums[1] = 2
  window = {3, 1}

i=4, num=2:
  Is 2 in {3, 1}? No
  window = {3, 1, 2}
  len(3) > k=2, remove nums[4-2] = nums[2] = 3
  window = {1, 2}

i=5, num=3:
  Is 3 in {1, 2}? No
  window = {1, 2, 3}
  len(3) > k=2, remove nums[5-2] = nums[3] = 1
  window = {2, 3}

Loop ends, no duplicate found within k distance.
Return false ✓
```

---

### 6.2 Minimum Absolute Difference

📌 **LeetCode Link**: [1200. Minimum Absolute Difference](https://leetcode.com/problems/minimum-absolute-difference/)

#### Problem Statement

Given an array of **distinct** integers `arr`, find all pairs of elements with the **minimum absolute difference** of any two elements.

Return a list of pairs in ascending order (with respect to pairs), each pair `[a, b]` follows:
- `a, b` are from arr
- `a < b`
- `b - a` equals the minimum absolute difference of any two elements in arr

#### Examples

```
Example 1:
Input: arr = [4, 2, 1, 3]
Output: [[1, 2], [2, 3], [3, 4]]
Explanation: Minimum difference is 1. All consecutive pairs have difference 1.

Example 2:
Input: arr = [1, 3, 6, 10, 15]
Output: [[1, 3]]
Explanation: Minimum difference is 2 (between 1 and 3).

Example 3:
Input: arr = [3, 8, -10, 23, 19, -4, -14, 27]
Output: [[-14, -10], [19, 23], [23, 27]]
Explanation: Minimum difference is 4.
```

#### The Key Insight

After sorting, the minimum difference MUST be between **adjacent elements**!

Why? If `a < b < c`, then:
- `c - a = (c - b) + (b - a)`
- So `c - a` is always greater than `b - a`

```python
def minimumAbsDifference(arr):
    arr.sort()
    
    min_diff = float('inf')
    result = []
    
    for i in range(1, len(arr)):
        diff = arr[i] - arr[i - 1]
        
        if diff < min_diff:
            # Found new minimum, reset result
            min_diff = diff
            result = [[arr[i - 1], arr[i]]]
        elif diff == min_diff:
            # Same minimum, add to result
            result.append([arr[i - 1], arr[i]])
    
    return result
```

**Analysis**:
- **Time**: O(n log n) - Sorting dominates
- **Space**: O(n) - For sorted array and result

**Detailed Walkthrough for [4, 2, 1, 3]**:

```
After sorting: [1, 2, 3, 4]

min_diff = infinity
result = []

i=1:
  diff = 2 - 1 = 1
  1 < infinity
  min_diff = 1
  result = [[1, 2]]

i=2:
  diff = 3 - 2 = 1
  1 == 1 (equal to min_diff)
  result = [[1, 2], [2, 3]]

i=3:
  diff = 4 - 3 = 1
  1 == 1 (equal to min_diff)
  result = [[1, 2], [2, 3], [3, 4]]

Final: [[1, 2], [2, 3], [3, 4]] ✓
```

---

### 6.3 Minimum Size Subarray Sum (Medium)

📌 **LeetCode Link**: [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

#### Problem Statement

Given an array of **positive integers** `nums` and a positive integer `target`, return the **minimal length** of a subarray whose sum is **greater than or equal to** `target`. If there is no such subarray, return 0.

#### Examples

```
Example 1:
Input: target = 7, nums = [2, 3, 1, 2, 4, 3]
Output: 2
Explanation: [4, 3] has sum 7 with minimum length 2

Example 2:
Input: target = 4, nums = [1, 4, 4]
Output: 1
Explanation: [4] has sum 4 with minimum length 1

Example 3:
Input: target = 11, nums = [1, 1, 1, 1, 1, 1, 1, 1]
Output: 0
Explanation: No subarray sums to 11 or more
```

#### Understanding "Subarray"

A **subarray** is a contiguous (connected) portion of an array:
```
Array: [2, 3, 1, 2, 4, 3]

Valid subarrays: [2], [2,3], [3,1,2], [1,2,4,3], etc.
Invalid (not contiguous): [2, 1], [3, 4] (elements must be next to each other)
```

#### The Strategy: Dynamic Sliding Window

1. **Expand** the window by moving the right pointer (add elements)
2. When sum >= target, **shrink** the window from the left (to find minimum)
3. Track the minimum length found

```python
def minSubArrayLen(target, nums):
    min_length = float('inf')  # Track minimum length found
    left = 0                    # Left edge of window
    current_sum = 0            # Sum of current window
    
    for right in range(len(nums)):
        # Expand window: add right element
        current_sum += nums[right]
        
        # Shrink window while sum meets target
        while current_sum >= target:
            # Update minimum length
            window_length = right - left + 1
            min_length = min(min_length, window_length)
            
            # Shrink: remove left element
            current_sum -= nums[left]
            left += 1
    
    # Return 0 if no valid subarray found
    return min_length if min_length != float('inf') else 0
```

**Analysis**:
- **Time**: O(n) - Each element is added once and removed at most once
- **Space**: O(1)

**Why is this O(n) and not O(n²)?**

Even though there's a `while` loop inside the `for` loop, the `left` pointer only moves **forward** (never backward). Each element is processed at most twice:
- Once when `right` reaches it (added to sum)
- Once when `left` reaches it (removed from sum)

Total operations: at most 2n = O(n)

**Detailed Walkthrough for target=7, nums=[2, 3, 1, 2, 4, 3]**:

```
min_length = infinity
left = 0
current_sum = 0

=== right = 0, nums[0] = 2 ===
current_sum = 0 + 2 = 2
2 < 7, don't shrink
Window: [2], sum=2

=== right = 1, nums[1] = 3 ===
current_sum = 2 + 3 = 5
5 < 7, don't shrink
Window: [2, 3], sum=5

=== right = 2, nums[2] = 1 ===
current_sum = 5 + 1 = 6
6 < 7, don't shrink
Window: [2, 3, 1], sum=6

=== right = 3, nums[3] = 2 ===
current_sum = 6 + 2 = 8
8 >= 7, START SHRINKING!

  Window: [2, 3, 1, 2], length = 3-0+1 = 4
  min_length = min(inf, 4) = 4
  Remove nums[0]=2: current_sum = 8-2 = 6
  left = 1
  
  6 < 7, stop shrinking
Window: [3, 1, 2], sum=6

=== right = 4, nums[4] = 4 ===
current_sum = 6 + 4 = 10
10 >= 7, SHRINK!

  Window: [3, 1, 2, 4], length = 4-1+1 = 4
  min_length = min(4, 4) = 4
  Remove nums[1]=3: current_sum = 10-3 = 7
  left = 2
  
  7 >= 7, SHRINK AGAIN!
  
  Window: [1, 2, 4], length = 4-2+1 = 3
  min_length = min(4, 3) = 3
  Remove nums[2]=1: current_sum = 7-1 = 6
  left = 3
  
  6 < 7, stop shrinking
Window: [2, 4], sum=6

=== right = 5, nums[5] = 3 ===
current_sum = 6 + 3 = 9
9 >= 7, SHRINK!

  Window: [2, 4, 3], length = 5-3+1 = 3
  min_length = min(3, 3) = 3
  Remove nums[3]=2: current_sum = 9-2 = 7
  left = 4
  
  7 >= 7, SHRINK AGAIN!
  
  Window: [4, 3], length = 5-4+1 = 2
  min_length = min(3, 2) = 2 ✓
  Remove nums[4]=4: current_sum = 7-4 = 3
  left = 5
  
  3 < 7, stop shrinking
Window: [3], sum=3

Loop ends.

Final: min_length = 2 ✓
```

---

## 7. Summary: Array Patterns Cheat Sheet

| Pattern | When to Use | Key Technique | Time |
|---------|-------------|---------------|------|
| **Hash Set** | Find duplicates, check existence | O(1) lookup | O(n) |
| **Hash Map** | Store value-index pairs, counting | O(1) lookup | O(n) |
| **Sorting** | Order matters, find pairs | Sort first, then scan | O(n log n) |
| **Two Pointers (Opposite)** | Sorted array, find pairs | Move from both ends | O(n) |
| **Two Pointers (Same Dir)** | Find peak, mountain | Expand from point | O(n) |
| **Sliding Window (Fixed)** | K-size subarray problems | Maintain window of size k | O(n) |
| **Sliding Window (Dynamic)** | Min/max subarray with condition | Expand right, shrink left | O(n) |
| **In-Place Marking** | Find missing in range [1,n] | Use array as hash table | O(n) |

---

## 8. Practice Checklist

### Easy Problems
- [ ] [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
- [ ] [268. Missing Number](https://leetcode.com/problems/missing-number/)
- [ ] [448. Find All Numbers Disappeared](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
- [ ] [1. Two Sum](https://leetcode.com/problems/two-sum/)
- [ ] [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
- [ ] [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)
- [ ] [1365. How Many Numbers Are Smaller](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/)
- [ ] [1266. Minimum Time Visiting All Points](https://leetcode.com/problems/minimum-time-visiting-all-points/)
- [ ] [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)
- [ ] [1200. Minimum Absolute Difference](https://leetcode.com/problems/minimum-absolute-difference/)

### Medium Problems
- [ ] [15. 3Sum](https://leetcode.com/problems/3sum/)
- [ ] [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
- [ ] [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [ ] [845. Longest Mountain in Array](https://leetcode.com/problems/longest-mountain-in-array/)
- [ ] [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

---

## 9. Key Takeaways

1. **Master the basics first**: Contains Duplicate, Two Sum, Missing Number
2. **Sets and dictionaries are your best friends**: Trade space for time
3. **Sorting unlocks many optimizations**: Three Sum, Minimum Absolute Difference
4. **Two pointers work on sorted data**: Saves O(n) compared to brute force
5. **Sliding window for contiguous subarrays**: Fixed or dynamic based on problem
6. **Draw it out**: Especially for Spiral Matrix, Number of Islands
7. **Think about edge cases**: Empty arrays, single elements, all same values
8. **Practice, practice, practice**: Understanding comes from doing, not just reading
