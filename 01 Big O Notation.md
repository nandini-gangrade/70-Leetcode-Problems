# Big O Notation & Complexity Analysis - Complete Deep Dive

This is the foundational knowledge you **must** master before tackling any coding interview or LeetCode problem. Understanding complexity analysis allows you to evaluate your solutions, compare different approaches, and communicate effectively with interviewers.

---

## 1. What is Big O Notation?

### 1.1 Definition

Big O notation is a mathematical notation that describes the **limiting behavior of a function** when the argument tends towards a particular value or infinity. In programming, we use it to describe how the performance of an algorithm changes as the input size grows.

**Key Point**: We're not measuring actual time in seconds or minutes—we're creating a mathematical generalization of the time or space required.

### 1.2 Why We Use Big O

Big O helps us:
- **Compare algorithms** objectively without running them
- **Predict scalability** - how will this perform with 1 million inputs vs 100?
- **Communicate efficiency** in interviews and code reviews
- **Identify bottlenecks** in our solutions

### 1.3 The Two Types of Complexity

| Type | What It Measures | Priority |
|------|------------------|----------|
| **Time Complexity** | How many operations as input grows | Higher priority |
| **Space Complexity** | How much memory as input grows | Lower priority |

**Why time is prioritized**: Space (memory) is relatively cheap and readily available. Users feel time delays more directly than memory usage. However, both matter in production systems.

---

## 2. Understanding Time Complexity

### 2.1 The Core Concept

Time complexity answers: **"If I double my input size, how much longer will this take?"**

Let's use a concrete example from the video:

**Problem**: Given a list of n numbers, find if the number 2 is in the list.

```python
def find_two(nums):
    for num in nums:
        if num == 2:
            return True
    return False
```

**Analysis**:
- If the list has 10 items, we might check up to 10 items
- If the list has 1,000 items, we might check up to 1,000 items
- If the list has n items, we check up to n items

This is **O(n)** - linear time. The time grows proportionally with the input.

### 2.2 Worst-Case Analysis

**Critical concept**: Big O focuses on the **worst-case scenario**.

In our example:
- **Best case**: 2 is at the first position → O(1) constant time
- **Worst case**: 2 is at the last position OR not in the list → O(n) linear time

We report O(n) because we must account for the worst possibility.

### 2.3 Dropping Constants and Lower-Order Terms

Big O simplifies by removing constants and keeping only the dominant term:

| Actual Complexity | Big O Notation | Why |
|-------------------|----------------|-----|
| O(2n) | O(n) | Constants don't matter at scale |
| O(n + 100) | O(n) | +100 is insignificant when n is huge |
| O(n² + n) | O(n²) | n² dominates when n is large |
| O(500) | O(1) | Any constant is O(1) |

**Why?** When n = 1,000,000:
- 2n = 2,000,000
- n² = 1,000,000,000,000

The n² term completely overwhelms everything else.

---

## 3. The Seven Essential Time Complexities

Here are the complexities you **must know**, ordered from fastest to slowest:

### 3.1 O(1) - Constant Time

**Definition**: The time taken remains the same regardless of input size.

**Examples**:
```python
# Accessing array element by index
arr[5]  # Same speed whether array has 10 or 10 million elements

# Hash table lookup
dictionary["key"]  # Average O(1)

# Push/pop from stack
stack.append(x)
stack.pop()

# Basic arithmetic
x + y
```

**Visual**: A flat horizontal line on a graph.

**Real-world analogy**: Looking up a word in a dictionary by page number (not searching).

---

### 3.2 O(log n) - Logarithmic Time

**Definition**: Time increases logarithmically as input grows. Operations typically halve the search space at each step.

**Key insight**: log₂(n) asks "how many times can I divide n by 2 before reaching 1?"

| n | log₂(n) |
|---|---------|
| 8 | 3 |
| 16 | 4 |
| 1,024 | 10 |
| 1,000,000 | ~20 |
| 1,000,000,000 | ~30 |

**Examples**:
```python
# Binary search
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Finding element in balanced BST
# Each comparison eliminates half the tree
```

**Visual**: A curve that rises quickly at first but flattens out.

**Real-world analogy**: Finding a name in a phone book by repeatedly opening to the middle and eliminating half.

---

### 3.3 O(n) - Linear Time

**Definition**: Time increases proportionally to input size. If you double the input, you double the time.

**Examples**:
```python
# Simple iteration
for item in array:
    print(item)

# Finding maximum value
def find_max(arr):
    max_val = arr[0]
    for num in arr:
        if num > max_val:
            max_val = num
    return max_val

# Linear search
# Counting elements
# Summing all elements
```

**Multiple O(n) operations**:
```python
for item in array:    # O(n)
    process(item)

for item in array:    # O(n)
    another_process(item)

# Total: O(n) + O(n) = O(2n) = O(n)
```

**Visual**: A straight diagonal line.

**Real-world analogy**: Reading every page of a book to find a specific sentence.

---

### 3.4 O(n log n) - Linearithmic Time

**Definition**: Time increases in a pattern that's slightly worse than linear but much better than quadratic.

**Where it appears**: Most efficient comparison-based sorting algorithms.

**Examples**:
```python
# Merge sort
# Quick sort (average case)
# Heap sort
# Tim sort (Python's built-in sort)

sorted_array = sorted(my_array)  # O(n log n)
my_list.sort()  # O(n log n)
```

**Why n log n for sorting?**
- We need to look at each element: O(n)
- Efficient algorithms use divide-and-conquer: O(log n) divisions
- Combined: O(n log n)

**Visual**: A curve slightly steeper than linear but much flatter than quadratic.

---

### 3.5 O(n²) - Quadratic Time

**Definition**: Time increases with the square of input size. Doubling input quadruples time.

**The telltale sign**: Nested loops where both iterate over input.

**Examples**:
```python
# Nested loops - comparing every pair
for i in range(n):
    for j in range(n):
        # some operation

# Bubble sort
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(n - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

# Checking for duplicates (brute force)
def has_duplicate_slow(arr):
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            if arr[i] == arr[j]:
                return True
    return False
```

**Scale impact**:
| n | n² operations |
|---|---------------|
| 10 | 100 |
| 100 | 10,000 |
| 1,000 | 1,000,000 |
| 10,000 | 100,000,000 |

**Visual**: A steep upward curve (parabola).

**Real-world analogy**: Comparing every person in a room with every other person.

---

### 3.6 O(2ⁿ) - Exponential Time

**Definition**: Time doubles with each additional input element. Grows extremely fast.

**Examples**:
```python
# Naive recursive Fibonacci
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)

# Generating all subsets
def all_subsets(arr):
    if len(arr) == 0:
        return [[]]
    rest = all_subsets(arr[1:])
    return rest + [[arr[0]] + subset for subset in rest]

# Brute force password cracking
```

**Scale impact**:
| n | 2ⁿ |
|---|-----|
| 10 | 1,024 |
| 20 | 1,048,576 |
| 30 | 1,073,741,824 |
| 40 | 1,099,511,627,776 |

**Visual**: An almost vertical line shooting upward.

**Warning**: O(2ⁿ) algorithms are usually only feasible for n < 20-30.

---

### 3.7 O(n!) - Factorial Time

**Definition**: Time increases factorially. The slowest common complexity.

**Factorial reminder**: n! = n × (n-1) × (n-2) × ... × 2 × 1

| n | n! |
|---|-----|
| 5 | 120 |
| 10 | 3,628,800 |
| 15 | 1,307,674,368,000 |
| 20 | 2,432,902,008,176,640,000 |

**Examples**:
```python
# Generating all permutations
def permutations(arr):
    if len(arr) <= 1:
        return [arr]
    result = []
    for i in range(len(arr)):
        rest = arr[:i] + arr[i+1:]
        for p in permutations(rest):
            result.append([arr[i]] + p)
    return result

# Traveling salesman (brute force)
# Solving certain NP-complete problems
```

**Visual**: Shoots up so fast it's essentially vertical immediately.

**Warning**: O(n!) is only practical for n < 10-12.

---

## 4. Complexity Comparison Chart

```
Time ↑
     |                                           O(n!)
     |                                        O(2ⁿ)
     |                                    O(n²)
     |                               O(n log n)
     |                          O(n)
     |                   O(log n)
     |____________O(1)_________________________ Input Size →
```

**Practical boundaries for 1-second execution**:
| Complexity | Approximate Max n |
|------------|-------------------|
| O(n!) | 10-12 |
| O(2ⁿ) | 20-25 |
| O(n²) | 10,000 |
| O(n log n) | 1,000,000 |
| O(n) | 100,000,000 |
| O(log n) | 10^18 |
| O(1) | Any |

---

## 5. Space Complexity

### 5.1 What It Measures

Space complexity measures the **total memory used** by an algorithm, including:
- Input storage (sometimes excluded—called "auxiliary space")
- Variables and data structures created
- Recursion call stack

### 5.2 Common Space Complexities

**O(1) - Constant Space**:
```python
def find_max(arr):
    max_val = arr[0]  # Just one variable
    for num in arr:
        if num > max_val:
            max_val = num
    return max_val
```

**O(n) - Linear Space**:
```python
def double_all(arr):
    result = []  # New array that grows with input
    for num in arr:
        result.append(num * 2)
    return result
```

**O(n) - Recursion Stack**:
```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
# Call stack grows to depth n
```

### 5.3 Space-Time Tradeoffs

Often you can trade space for time:

**Example: Two Sum Problem**

*Slow time, low space* - O(n²) time, O(1) space:
```python
def two_sum_slow(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
```

*Fast time, more space* - O(n) time, O(n) space:
```python
def two_sum_fast(nums, target):
    seen = {}  # Extra space for hash map
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
```

---

## 6. Analyzing Complex Code

### 6.1 Sequential Operations (Add)

```python
for i in range(n):      # O(n)
    operation1()

for j in range(n):      # O(n)
    operation2()

# Total: O(n) + O(n) = O(2n) = O(n)
```

### 6.2 Nested Operations (Multiply)

```python
for i in range(n):          # O(n)
    for j in range(n):      # × O(n)
        operation()

# Total: O(n) × O(n) = O(n²)
```

### 6.3 Different Variables

```python
for i in range(n):          # O(n)
    operation()

for j in range(m):          # O(m)
    operation()

# Total: O(n + m) - cannot simplify further
```

```python
for i in range(n):          # O(n)
    for j in range(m):      # × O(m)
        operation()

# Total: O(n × m)
```

### 6.4 Logarithmic Patterns

Look for **halving** or **doubling**:

```python
i = n
while i > 0:
    operation()
    i = i // 2  # Halving → O(log n)
```

```python
i = 1
while i < n:
    operation()
    i = i * 2  # Doubling → O(log n)
```

### 6.5 Multiple Complexity Sources

Identify the **dominant term**:

```python
def complex_function(arr):
    arr.sort()                      # O(n log n)
    
    for item in arr:                # O(n)
        print(item)
    
    for i in range(len(arr)):       # O(n²)
        for j in range(len(arr)):
            compare(arr[i], arr[j])

# Total: O(n log n) + O(n) + O(n²) = O(n²)
# The n² dominates
```

---

## 7. Common Patterns and Their Complexities

| Pattern | Time | Space | Example |
|---------|------|-------|---------|
| Single loop | O(n) | O(1) | Finding max |
| Two nested loops (same array) | O(n²) | O(1) | Bubble sort |
| Two nested loops (different arrays) | O(n×m) | O(1) | Matrix operations |
| Binary search | O(log n) | O(1) | Finding in sorted array |
| Sorting | O(n log n) | O(n) or O(log n) | Merge/Quick sort |
| Hash table operations | O(1) avg | O(n) | Dictionary lookups |
| BFS/DFS on tree | O(n) | O(n) | Tree traversal |
| BFS/DFS on graph | O(V+E) | O(V) | Graph traversal |
| Dynamic programming | O(n) to O(n²) | O(n) to O(n²) | Depends on problem |
| Generating all subsets | O(2ⁿ) | O(2ⁿ) | Backtracking |
| Generating all permutations | O(n!) | O(n!) | Backtracking |

---

## 8. Interview Tips for Complexity Analysis

### 8.1 Always Discuss Complexity

After presenting a solution, **always** state:
- Time complexity
- Space complexity
- Why (brief explanation)

### 8.2 Start with Brute Force

"The brute force approach would be O(n²) because of the nested loops. Can I optimize?"

### 8.3 Know Common Optimizations

| From | To | How |
|------|-----|-----|
| O(n²) | O(n log n) | Sorting first |
| O(n²) | O(n) | Hash map |
| O(2ⁿ) | O(n) or O(n²) | Dynamic programming |
| O(n) | O(log n) | Binary search (if sorted) |

### 8.4 Space vs Time Discussion

"I can solve this in O(n) time using O(n) extra space with a hash map, or O(n²) time with O(1) space. Which would you prefer?"

---

## 9. Practice Problems by Complexity

### O(n) Solutions:
- Two Sum (with hash map)
- Contains Duplicate (with set)
- Valid Anagram
- Maximum Subarray

### O(n log n) Solutions:
- Merge Intervals
- Meeting Rooms
- Sort Colors (can also be O(n))

### O(n²) Solutions (often can be optimized):
- Three Sum
- Longest Palindromic Substring

### O(log n) Solutions:
- Binary Search
- Search in Rotated Sorted Array
- Find Minimum in Rotated Sorted Array

---

## 10. Key Takeaways

1. **Big O describes growth rate**, not actual time
2. **Focus on worst case** unless asked otherwise
3. **Drop constants and lower-order terms**
4. **Time complexity usually matters more** than space
5. **Nested loops = multiply**, sequential = add
6. **Look for halving/doubling** for logarithmic patterns
7. **Always state complexity** when presenting solutions
8. **Practice identifying patterns** - it becomes intuitive

---

## 11. Quick Reference Card

```
O(1)       - Constant    - Hash lookup, array access
O(log n)   - Logarithmic - Binary search, balanced BST
O(n)       - Linear      - Single loop, linear search
O(n log n) - Linearithmic- Efficient sorting
O(n²)      - Quadratic   - Nested loops, simple sorting
O(2ⁿ)      - Exponential - Recursive subsets
O(n!)      - Factorial   - Permutations
```

**Remember**: When n is large enough, O(n) will always beat O(n²), and O(n²) will always beat O(2ⁿ), regardless of constants.
