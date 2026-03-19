# Complete Backtracking Guide for Interview Preparation

## What is Backtracking?

**Backtracking** is an algorithmic technique where we explore all possible solutions by building them incrementally. When we hit a dead end (a solution that doesn't work), we "backtrack" - meaning we undo our last choice and try a different option.

### Real-World Analogy
Think of navigating a maze:
- You walk down a path
- If you hit a wall (dead end), you walk back to the last intersection
- You try a different path
- Repeat until you find the exit

### Why Backtracking Works
Backtracking is essentially **exploring a decision tree**:
- At each step, we make a choice
- We explore that choice fully
- If it doesn't lead to a solution, we undo it (backtrack)
- We try the next choice

**Example:** Solving Sudoku
- Try placing number 1 in a cell
- Move to next cell and try valid numbers
- If you get stuck, backtrack and try number 2 in the previous cell
- Continue until the entire board is filled correctly

---

## 7.1 Core Concepts

### Permutations vs Combinations

These two concepts are fundamental to understanding backtracking problems.

#### **Permutations: Order Matters**

In permutations, the **order** of elements is important. 

**Example:** For `[1, 2, 3]`:
- `[1, 2, 3]` is different from `[3, 2, 1]`
- Both are valid permutations but they're different

**Real-world example:** 
- Password combinations: "abc" ≠ "bca"
- Race finishing positions: 1st place, 2nd place, 3rd place (order matters!)

**Formula:** n! (n factorial)
- For 3 elements: 3! = 3 × 2 × 1 = 6 permutations

#### **Combinations: Order Doesn't Matter**

In combinations, the **order** doesn't matter - we only care about which elements are selected.

**Example:** For `[1, 2, 3]`, choosing 2 elements:
- `[1, 2]` is the same as `[2, 1]`
- We count this as one combination

**Real-world example:**
- Pizza toppings: Pepperoni + Mushroom = Mushroom + Pepperoni (same pizza!)
- Team selection: Choosing 3 people from 5 candidates

**Formula:** C(n, k) = n! / (k! × (n-k)!)
- For 3 elements, choosing 2: C(3,2) = 3!/(2!×1!) = 3

---

## 7.2 Problems Covered

### Problem 1: Letter Case Permutation

**[LeetCode 784 - Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)**

#### Problem Statement
Given a string `s`, return all possible strings we can get by toggling the case of each letter in the string.

**Example:**
```
Input: s = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]
```

#### Understanding the Problem

For each **letter** in the string:
- We have 2 choices: lowercase or uppercase
- For **digits**: We have only 1 choice (keep as is)

**Why this is a backtracking problem:**
- At each letter, we make a choice (uppercase or lowercase)
- We explore both choices fully
- This creates a decision tree

#### Visualization

For string "a1b":
```
                    a1b
                   /   \
              a1b       A1b
             /   \     /   \
          a1b   a1B  A1b  A1B
```

#### Approach 1: Iterative Solution

**Why start with iterative?**
- Easier to understand for beginners
- No recursion stack to think about
- Clearer step-by-step process

**How it works:**
1. Start with the original string in our result
2. For each character:
   - If it's a letter, take all current results
   - For each result, create a new version with the letter toggled
   - Add these new versions to our result

**Step-by-step example for "a1b":**

```
Initial: result = ["a1b"]

Character 'a' (index 0):
  Current results: ["a1b"]
  Toggle 'a' to 'A': "A1b"
  New result: ["a1b", "A1b"]

Character '1' (index 1):
  It's a digit, skip

Character 'b' (index 2):
  Current results: ["a1b", "A1b"]
  Toggle 'b' in "a1b" → "a1B"
  Toggle 'b' in "A1b" → "A1B"
  Final result: ["a1b", "A1b", "a1B", "A1B"]
```

**Code:**
```python
def letterCasePermutation(s: str) -> list[str]:
    result = [s]  # Start with original string
    
    # Go through each character
    for i in range(len(s)):
        # Only process letters, not digits
        if s[i].isalpha():
            # For each existing string in result
            current_size = len(result)
            for j in range(current_size):
                # Get the current string
                chars = list(result[j])
                
                # Toggle the case of character at position i
                if chars[i].islower():
                    chars[i] = chars[i].upper()
                else:
                    chars[i] = chars[i].lower()
                
                # Add this new variation
                result.append(''.join(chars))
    
    return result
```

**Why this works:**
- We build all possibilities by doubling our results at each letter
- For n letters, we get 2^n combinations
- Time: O(2^n × n) - we create 2^n strings, each takes O(n) to build

#### Approach 2: Recursive Backtracking Solution

**Why use recursion?**
- More natural for backtracking problems
- Follows the decision tree structure
- Standard pattern for interview problems

**How it works:**
1. At each position, if it's a letter, try both cases
2. Recurse to the next position
3. When we reach the end, add the current string to results

**Code:**
```python
def letterCasePermutation(s: str) -> list[str]:
    result = []
    
    def backtrack(index, path):
        # Base case: reached end of string
        if index == len(s):
            result.append(''.join(path))
            return
        
        char = s[index]
        
        if char.isalpha():
            # Choice 1: Use lowercase
            path.append(char.lower())
            backtrack(index + 1, path)
            path.pop()  # Backtrack
            
            # Choice 2: Use uppercase
            path.append(char.upper())
            backtrack(index + 1, path)
            path.pop()  # Backtrack
        else:
            # It's a digit, only one choice
            path.append(char)
            backtrack(index + 1, path)
            path.pop()  # Backtrack
    
    backtrack(0, [])
    return result
```

**Tracing through "ab":**
```
backtrack(0, [])
  → Try 'a': backtrack(1, ['a'])
      → Try 'b': backtrack(2, ['a','b'])
          → Add "ab" to result
      → Try 'B': backtrack(2, ['a','B'])
          → Add "aB" to result
  → Try 'A': backtrack(1, ['A'])
      → Try 'b': backtrack(2, ['A','b'])
          → Add "Ab" to result
      → Try 'B': backtrack(2, ['A','B'])
          → Add "AB" to result
```

**Key backtracking elements:**
- `path.append()` - Make a choice
- `backtrack()` - Explore that choice
- `path.pop()` - Undo the choice (this is the backtrack step!)

---

### Problem 2: Subsets

**[LeetCode 78 - Subsets](https://leetcode.com/problems/subsets/)**

#### Problem Statement
Given an integer array `nums` of **unique** elements, return all possible subsets (the power set).

**Example:**
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

#### Understanding the Problem

A **subset** is any combination of elements from the array, including:
- Empty set: `[]`
- Individual elements: `[1]`, `[2]`, `[3]`
- Pairs: `[1,2]`, `[1,3]`, `[2,3]`
- All elements: `[1,2,3]`

**Key insight:** For each element, we have 2 choices:
1. Include it in the subset
2. Don't include it

**Total subsets:** 2^n (where n is the length of array)
- For [1,2,3]: 2^3 = 8 subsets

#### Why This is a Combination Problem
- Order doesn't matter: [1,2] is the same as [2,1]
- We just care about which elements are selected

#### Approach 1: Backtracking (Most Important for Interviews)

**Core idea:**
- At each position, decide: include this element or not?
- Use a `start` index to avoid duplicates

**Why we need a start index:**
Without it, we'd generate duplicates:
- [1, 2] and [2, 1] would both be generated
- But in combinations, these are the same!

**How start index prevents duplicates:**
- We only consider elements from `start` onwards
- Once we've decided not to include element `i`, we never go back to it
- This ensures we only move forward in the array

**Code:**
```python
def subsets(nums: list[int]) -> list[list[int]]:
    result = []
    
    def backtrack(start, path):
        # Every path we build is a valid subset
        result.append(path[:])  # Make a copy of path
        
        # Try adding each remaining element
        for i in range(start, len(nums)):
            # Include nums[i]
            path.append(nums[i])
            
            # Recurse with next index
            backtrack(i + 1, path)
            
            # Backtrack: remove nums[i]
            path.pop()
    
    backtrack(0, [])
    return result
```

**Detailed trace for [1,2,3]:**

```
backtrack(0, [])
  → Add [] to result
  
  i=0 (num=1):
    path = [1]
    backtrack(1, [1])
      → Add [1] to result
      
      i=1 (num=2):
        path = [1,2]
        backtrack(2, [1,2])
          → Add [1,2] to result
          
          i=2 (num=3):
            path = [1,2,3]
            backtrack(3, [1,2,3])
              → Add [1,2,3] to result
            path = [1,2]  (backtracked)
        path = [1]  (backtracked)
      
      i=2 (num=3):
        path = [1,3]
        backtrack(3, [1,3])
          → Add [1,3] to result
        path = [1]  (backtracked)
    path = []  (backtracked)
  
  i=1 (num=2):
    path = [2]
    backtrack(2, [2])
      → Add [2] to result
      
      i=2 (num=3):
        path = [2,3]
        backtrack(3, [2,3])
          → Add [2,3] to result
        path = [2]  (backtracked)
    path = []  (backtracked)
  
  i=2 (num=3):
    path = [3]
    backtrack(3, [3])
      → Add [3] to result
    path = []  (backtracked)

Result: [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

**Why we copy path:** `path[:]`
- `path` is modified during recursion
- Without copying, all results would reference the same list
- By the end, all would be empty!

#### Approach 2: Iterative Solution

**Why this approach?**
- Sometimes easier to understand than recursion
- Good to know alternative methods

**How it works:**
1. Start with empty subset: `[[]]`
2. For each number in nums:
   - Take all existing subsets
   - Create new subsets by adding current number to each
   - Add these new subsets to result

**Code:**
```python
def subsets(nums: list[int]) -> list[list[int]]:
    result = [[]]  # Start with empty subset
    
    for num in nums:
        # For each existing subset, create a new one with current num
        new_subsets = []
        for subset in result:
            new_subsets.append(subset + [num])
        
        # Add all new subsets to result
        result.extend(new_subsets)
    
    return result
```

**Step-by-step for [1,2,3]:**
```
Initial: result = [[]]

Add 1:
  Existing: [[]]
  New: [[1]]
  Result: [[], [1]]

Add 2:
  Existing: [[], [1]]
  New: [[2], [1,2]]
  Result: [[], [1], [2], [1,2]]

Add 3:
  Existing: [[], [1], [2], [1,2]]
  New: [[3], [1,3], [2,3], [1,2,3]]
  Result: [[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]
```

**Time Complexity:** O(n × 2^n)
- We generate 2^n subsets
- Each subset takes O(n) time to create

---

### Problem 3: Combinations

**[LeetCode 77 - Combinations](https://leetcode.com/problems/combinations/)**

#### Problem Statement
Given two integers `n` and `k`, return all possible combinations of `k` numbers chosen from the range `[1, n]`.

**Example:**
```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
```

#### Understanding the Problem

We need to choose `k` numbers from `1` to `n`:
- Order doesn't matter: [1,2] is same as [2,1]
- We can't repeat numbers: Each number used at most once
- We need exactly `k` numbers in each combination

**Why this is different from Subsets:**
- Subsets: All possible sizes (0 to n)
- Combinations: Fixed size (exactly k)

#### Approach 1: Backtracking with Pruning

**Why pruning is important:**
- Without pruning, we'd explore paths that can never work
- Example: If we need 3 numbers but only 2 positions left, stop early!

**How to think about this:**
1. Start with an empty combination
2. Add numbers one by one (only from current position forward)
3. When we have k numbers, add to result
4. Prune: If we can't possibly get k numbers, stop

**Code:**
```python
def combine(n: int, k: int) -> list[list[int]]:
    result = []
    
    def backtrack(start, path):
        # Base case: we have k numbers
        if len(path) == k:
            result.append(path[:])
            return
        
        # Pruning: check if we have enough numbers left
        need = k - len(path)  # How many more numbers we need
        remain = n - start + 1  # How many numbers are available
        
        # Try adding each number from start to n
        for i in range(start, n + 1):
            # Pruning optimization
            if remain < need:
                break
            
            path.append(i)
            backtrack(i + 1, path)
            path.pop()
            
            remain -= 1  # One less number available
    
    backtrack(1, [])
    return result
```

**Detailed trace for n=4, k=2:**

```
backtrack(1, [])
  
  i=1:
    path = [1]
    backtrack(2, [1])
      
      i=2:
        path = [1,2]
        len(path)==k, add [1,2] to result
        path = [1]
      
      i=3:
        path = [1,3]
        len(path)==k, add [1,3] to result
        path = [1]
      
      i=4:
        path = [1,4]
        len(path)==k, add [1,4] to result
        path = [1]
    
    path = []
  
  i=2:
    path = [2]
    backtrack(3, [2])
      
      i=3:
        path = [2,3]
        len(path)==k, add [2,3] to result
        path = [2]
      
      i=4:
        path = [2,4]
        len(path)==k, add [2,4] to result
        path = [2]
    
    path = []
  
  i=3:
    path = [3]
    backtrack(4, [3])
      
      i=4:
        path = [3,4]
        len(path)==k, add [3,4] to result
        path = [3]
    
    path = []
  
  i=4:
    path = [4]
    backtrack(5, [4])
      need=1, remain=0
      Not enough numbers left, return immediately

Result: [[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
```

**Understanding the pruning:**
```
For n=4, k=3:

When start=4, path=[1,2]:
  need = 3 - 2 = 1 (need 1 more number)
  remain = 4 - 4 + 1 = 1 (only number 4 left)
  1 >= 1, so continue (can potentially work)

When start=4, path=[1]:
  need = 3 - 1 = 2 (need 2 more numbers)
  remain = 4 - 4 + 1 = 1 (only number 4 left)
  1 < 2, so stop (impossible to get 3 numbers)
```

**Why start from `i+1` in recursion:**
- Ensures we don't reuse numbers
- Ensures we don't generate duplicates
- Example: After using 1, we only consider 2, 3, 4 (not 1 again)

**Time Complexity:** O(C(n,k) × k)
- C(n,k) combinations to generate
- Each takes O(k) time to build

---

### Problem 4: Permutations

**[LeetCode 46 - Permutations](https://leetcode.com/problems/permutations/)**

#### Problem Statement
Given an array `nums` of distinct integers, return all possible permutations.

**Example:**
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

#### Understanding the Problem

**Permutations = Order matters!**
- [1,2,3] is different from [3,2,1]
- We use each element exactly once
- We need all n elements in each permutation

**Total permutations:** n! = n × (n-1) × (n-2) × ... × 1
- For [1,2,3]: 3! = 6 permutations

**Thinking process:**
- First position: n choices
- Second position: n-1 choices (one already used)
- Third position: n-2 choices
- And so on...

#### Approach 1: Backtracking with Used Array

**Why we need a "used" array:**
- Unlike combinations, we can pick any unused element (not just forward)
- We need to track which elements are already in current permutation
- Can't use `start` index like in combinations

**How it works:**
1. Try each unused number in current position
2. Mark it as used
3. Recursively build rest of permutation
4. Unmark it (backtrack) and try next number

**Code:**
```python
def permute(nums: list[int]) -> list[list[int]]:
    result = []
    used = [False] * len(nums)  # Track which elements are used
    
    def backtrack(path):
        # Base case: permutation is complete
        if len(path) == len(nums):
            result.append(path[:])
            return
        
        # Try each number
        for i in range(len(nums)):
            # Skip if already used
            if used[i]:
                continue
            
            # Choose: add nums[i] to path
            path.append(nums[i])
            used[i] = True
            
            # Explore
            backtrack(path)
            
            # Unchoose (backtrack)
            path.pop()
            used[i] = False
    
    backtrack([])
    return result
```

**Detailed trace for [1,2,3]:**

```
backtrack([])
  used = [F, F, F]
  
  i=0 (num=1):
    path = [1], used = [T, F, F]
    backtrack([1])
      
      i=0: skip (used[0]=True)
      
      i=1 (num=2):
        path = [1,2], used = [T, T, F]
        backtrack([1,2])
          
          i=0: skip
          i=1: skip
          
          i=2 (num=3):
            path = [1,2,3], used = [T, T, T]
            len(path)==3, add [1,2,3] to result
            path = [1,2], used = [T, T, F]
        
        path = [1], used = [T, F, F]
      
      i=2 (num=3):
        path = [1,3], used = [T, F, T]
        backtrack([1,3])
          
          i=0: skip
          
          i=1 (num=2):
            path = [1,3,2], used = [T, T, T]
            len(path)==3, add [1,3,2] to result
            path = [1,3], used = [T, F, T]
          
          i=2: skip
        
        path = [1], used = [T, F, F]
    
    path = [], used = [F, F, F]
  
  i=1 (num=2):
    path = [2], used = [F, T, F]
    ... (similar pattern)
    Results: [2,1,3] and [2,3,1]
  
  i=2 (num=3):
    path = [3], used = [F, F, T]
    ... (similar pattern)
    Results: [3,1,2] and [3,2,1]

Final: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Key differences from Combinations:**
- We try ALL elements at each position (not just from start onwards)
- We need `used` array to prevent reusing elements
- We explore n choices at each level (not decreasing choices)

#### Approach 2: Backtracking with Swap

**Alternative approach:**
- Instead of tracking used elements, swap them
- Build permutation in-place

**Why this works:**
- Swap current element with each element after it
- Recursively permute the rest
- Swap back to restore original array

**Code:**
```python
def permute(nums: list[int]) -> list[list[int]]:
    result = []
    
    def backtrack(start):
        # Base case: one permutation complete
        if start == len(nums):
            result.append(nums[:])
            return
        
        # Try each element in position 'start'
        for i in range(start, len(nums)):
            # Swap: put nums[i] at position start
            nums[start], nums[i] = nums[i], nums[start]
            
            # Recurse on remaining positions
            backtrack(start + 1)
            
            # Swap back (backtrack)
            nums[start], nums[i] = nums[i], nums[start]
    
    backtrack(0)
    return result
```

**Visualization for [1,2,3]:**
```
Position 0: Try each element

  Put 1 at position 0: [1,2,3]
    Position 1: Try 2 and 3
      Put 2 at pos 1: [1,2,3] → Add to result
      Put 3 at pos 1: [1,3,2] → Add to result
  
  Put 2 at position 0: [2,1,3]
    Position 1: Try 1 and 3
      Put 1 at pos 1: [2,1,3] → Add to result
      Put 3 at pos 1: [2,3,1] → Add to result
  
  Put 3 at position 0: [3,2,1]
    Position 1: Try 2 and 1
      Put 2 at pos 1: [3,2,1] → Add to result
      Put 1 at pos 1: [3,1,2] → Add to result
```

**Time Complexity:** O(n! × n)
- n! permutations to generate
- Each takes O(n) time to copy

---

## 7.3 Common Backtracking Pattern

Now that we've seen multiple problems, let's identify the **universal pattern**.

### The Template

```python
def backtrack(path, choices):
    # Base case: solution is complete
    if is_solution(path):
        add path to result
        return
    
    # Try each available choice
    for choice in choices:
        # Check if choice is valid
        if not is_valid(choice):
            continue
        
        # Make choice
        add choice to path
        mark choice as used (if needed)
        
        # Explore with this choice
        backtrack(path, remaining_choices)
        
        # Unmake choice (backtrack)
        remove choice from path
        unmark choice (if needed)
```

### Breaking Down the Pattern

#### 1. **Base Case**
When do we have a complete solution?
- **Subsets:** Always (any path is valid)
- **Combinations:** When `len(path) == k`
- **Permutations:** When `len(path) == n`

#### 2. **Choice Space**
What choices do we have at each step?
- **Subsets:** All elements from `start` onwards
- **Combinations:** Numbers from `start` to `n`
- **Permutations:** All unused elements

#### 3. **Making a Choice**
What do we do when making a choice?
- Add element to `path`
- Mark as used (if tracking used elements)
- Update any other state

#### 4. **Exploring**
- Recursively call backtrack with updated state
- This explores all possibilities with current choice

#### 5. **Undoing the Choice (Backtracking)**
- Remove element from `path`
- Unmark as used
- Restore any other state
- This allows us to try other choices

---

## Additional Important Problems

### Problem 5: Combination Sum

**[LeetCode 39 - Combination Sum](https://leetcode.com/problems/combination-sum/)**

#### Problem Statement
Given an array of distinct integers `candidates` and a target integer `target`, return all unique combinations where the chosen numbers sum to `target`. You may use the same number unlimited times.

**Example:**
```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
```

#### Understanding the Problem

**Key differences from regular Combinations:**
- We can reuse elements unlimited times
- We stop when sum equals target (not when we have k elements)
- We need to track the current sum

**Why this is harder:**
- We can pick the same element multiple times: [2,2,3]
- We need to avoid exceeding the target (pruning)
- We need to avoid duplicates: [2,3] and [3,2] are the same

#### Approach: Backtracking with Reusable Elements

**Key insight:**
- Use `start` index to avoid duplicates
- Allow reusing current element by passing `i` (not `i+1`)
- Prune when sum exceeds target

**Code:**
```python
def combinationSum(candidates: list[int], target: int) -> list[list[int]]:
    result = []
    
    def backtrack(start, path, current_sum):
        # Base case: found a valid combination
        if current_sum == target:
            result.append(path[:])
            return
        
        # Pruning: exceeded target
        if current_sum > target:
            return
        
        # Try each candidate from start onwards
        for i in range(start, len(candidates)):
            # Make choice
            path.append(candidates[i])
            
            # Explore (i, not i+1, because we can reuse)
            backtrack(i, path, current_sum + candidates[i])
            
            # Backtrack
            path.pop()
    
    backtrack(0, [], 0)
    return result
```

**Trace for candidates=[2,3], target=7:**
```
backtrack(0, [], 0)
  
  i=0 (num=2):
    path=[2], sum=2
    backtrack(0, [2], 2)
      
      i=0 (num=2):
        path=[2,2], sum=4
        backtrack(0, [2,2], 4)
          
          i=0 (num=2):
            path=[2,2,2], sum=6
            backtrack(0, [2,2,2], 6)
              
              i=0 (num=2):
                sum=8 > 7, return (pruned)
              
              i=1 (num=3):
                sum=9 > 7, return (pruned)
          
          i=1 (num=3):
            path=[2,2,3], sum=7
            sum==target, add [2,2,3] to result
      
      i=1 (num=3):
        path=[2,3], sum=5
        backtrack(1, [2,3], 5)
          
          i=1 (num=3):
            path=[2,3,3], sum=8 > 7, return (pruned)
  
  i=1 (num=3):
    path=[3], sum=3
    backtrack(1, [3], 3)
      
      i=1 (num=3):
        path=[3,3], sum=6
        backtrack(1, [3,3], 6)
          
          i=1 (num=3):
            sum=9 > 7, return (pruned)

Result: [[2,2,3]]
```

**Why we pass `i` (not `i+1`):**
- Allows reusing same element
- Example: After adding 2, we can add 2 again

**Optimization:** Sort candidates first
- Early pruning: If candidates[i] > remaining, stop
- No need to try larger numbers

---

### Problem 6: Generate Parentheses

**[LeetCode 22 - Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)**

#### Problem Statement
Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
**Example:**
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

#### Understanding the Problem

**What makes parentheses valid:**
1. Same number of opening '(' and closing ')'
2. At any point, number of ')' should not exceed number of '('
3. Total: n opening and n closing parentheses

**Why this is a backtracking problem:**
- At each step, we choose to add '(' or ')'
- We explore all valid choices
- We backtrack when we can't add more

#### Approach: Backtracking with Constraints

**Key insight:**
- Track count of '(' and ')' used
- Only add '(' if we haven't used n yet
- Only add ')' if it won't exceed '(' count

**Why these constraints work:**
- First constraint: Ensures we don't use too many '('
- Second constraint: Ensures parentheses remain balanced

**Code:**
```python
def generateParenthesis(n: int) -> list[str]:
    result = []
    
    def backtrack(path, open_count, close_count):
        # Base case: used all parentheses
        if len(path) == 2 * n:
            result.append(''.join(path))
            return
        
        # Choice 1: Add opening parenthesis
        if open_count < n:
            path.append('(')
            backtrack(path, open_count + 1, close_count)
            path.pop()
        
        # Choice 2: Add closing parenthesis
        if close_count < open_count:
            path.append(')')
            backtrack(path, open_count, close_count + 1)
            path.pop()
    
    backtrack([], 0, 0)
    return result
```

**Detailed trace for n=2:**
```
backtrack([], 0, 0)
  
  Add '(':
    path=['('], open=1, close=0
    backtrack(['('], 1, 0)
      
      Add '(':
        path=['(',(''], open=2, close=0
        backtrack(['(','('], 2, 0)
          
          Can't add '(' (open==n)
          
          Add ')':
            path=['(','(',')'], open=2, close=1
            backtrack(['(','(',')'], 2, 1)
              
              Can't add '(' (open==n)
              
              Add ')':
                path=['(','(',')' ,')'], open=2, close=2
                len==4, add "(())" to result
      
      Add ')':
        path=['(',')'], open=1, close=1
        backtrack(['(',')'], 1, 1)
          
          Add '(':
            path=['(',')','('], open=2, close=1
            backtrack(['(',')','('], 2, 1)
              
              Can't add '(' (open==n)
              
              Add ')':
                path=['(',')','(',')'], open=2, close=2
                len==4, add "()()" to result

Result: ["(())", "()()"]
```

**Why close_count < open_count:**
```
Valid: ((
  open=2, close=0
  Can add ')' because close < open

Invalid: ())
  Would have open=1, close=2
  We prevent this by checking close < open
```

---

### Problem 7: Word Search

**[LeetCode 79 - Word Search](https://leetcode.com/problems/word-search/)**

#### Problem Statement
Given a 2D board and a word, find if the word exists in the grid. The word can be constructed from letters of sequentially adjacent cells (horizontally or vertically). The same cell cannot be used twice.

**Example:**
```
board = [
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED"
Output: true
```

#### Understanding the Problem

**Rules:**
- Start from any cell
- Move to adjacent cells (up, down, left, right)
- Cannot revisit a cell in the same path
- Must match entire word

**Why this is a backtracking problem:**
- We explore paths character by character
- If path doesn't match, we backtrack
- We try all possible starting positions

#### Approach: 2D Backtracking

**Key elements:**
1. Try each cell as starting point
2. From each cell, explore 4 directions
3. Mark cells as visited (to avoid reuse)
4. Unmark when backtracking

**Code:**
```python
def exist(board: list[list[str]], word: str) -> bool:
    rows, cols = len(board), len(board[0])
    
    def backtrack(r, c, index):
        # Base case: matched entire word
        if index == len(word):
            return True
        
        # Check boundaries
        if (r < 0 or r >= rows or 
            c < 0 or c >= cols or 
            board[r][c] != word[index]):
            return False
        
        # Mark as visited
        temp = board[r][c]
        board[r][c] = '#'  # Use a marker
        
        # Explore 4 directions
        found = (backtrack(r+1, c, index+1) or  # down
                 backtrack(r-1, c, index+1) or  # up
                 backtrack(r, c+1, index+1) or  # right
                 backtrack(r, c-1, index+1))    # left
        
        # Restore cell (backtrack)
        board[r][c] = temp
        
        return found
    
    # Try each cell as starting point
    for r in range(rows):
        for c in range(cols):
            if backtrack(r, c, 0):
                return True
    
    return False
```

**Trace for finding "AB":**
```
Board:
  A B
  C D

Try (0,0) 'A':
  Matches word[0]='A'
  Mark (0,0) as visited: '#'
  
  Try down (1,0) 'C':
    Doesn't match word[1]='B', return False
  
  Try up (-1,0):
    Out of bounds, return False
  
  Try right (0,1) 'B':
    Matches word[1]='B'
    index==2==len(word), return True!

Found! Return True
```

**Why we modify the board:**
- Efficient way to mark visited cells
- No need for separate visited set
- Must restore on backtrack!

---

## Key Backtracking Concepts for Interviews

### 1. Decision Trees
Every backtracking problem can be visualized as a decision tree:
- Each node = a state (current path/choice)
- Each edge = making a choice
- Leaves = complete solutions or dead ends

### 2. Pruning
**What is pruning?**
Stopping exploration early when we know a path can't lead to a solution.

**Examples:**
- Combination Sum: Stop if sum > target
- Combinations: Stop if not enough numbers left
- Word Search: Stop if character doesn't match

**Why pruning matters:**
- Dramatically reduces time complexity
- Critical for interview performance
- Shows you understand optimization

### 3. State Management
**What state do we track?**
- Current path/solution being built
- What elements are used (used array, visited set)
- Additional constraints (sum, count, etc.)

**Critical rule:**
- Make choice → Explore → Unmake choice
- This is the essence of backtracking!

### 4. Avoiding Duplicates
**Common strategies:**
- Use `start` index (for combinations/subsets)
- Sort input + skip duplicates (for problems with duplicate elements)
- Use sets (when order doesn't matter)

---

## Practice Strategy for Interviews

### Beginner Level (Start Here)
1. **[Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)** - Easiest backtracking
2. **[Subsets](https://leetcode.com/problems/subsets/)** - Foundation for many problems
3. **[Combinations](https://leetcode.com/problems/combinations/)** - Learn pruning

### Intermediate Level
4. **[Permutations](https://leetcode.com/problems/permutations/)** - Understand state management
5. **[Combination Sum](https://leetcode.com/problems/combination-sum/)** - Reusable elements
6. **[Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)** - Constraint-based backtracking

### Advanced Level
7. **[Word Search](https://leetcode.com/problems/word-search/)** - 2D backtracking
8. **[N-Queens](https://leetcode.com/problems/n-queens/)** - Classic hard problem
9. **[Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)** - Complex constraints

---

## Time & Space Complexity Summary

| Problem | Time Complexity | Space Complexity | # Solutions |
|---------|----------------|------------------|-------------|
| Subsets | O(n × 2^n) | O(n) | 2^n |
| Combinations | O(k × C(n,k)) | O(k) | C(n,k) |
| Permutations | O(n × n!) | O(n) | n! |
| Combination Sum | O(n^target) | O(target) | Varies |
| Generate Parentheses | O(4^n / √n) | O(n) | Catalan number |

**Note:** Space complexity is for recursion stack depth, not including output space.

---

## Common Mistakes to Avoid

### 1. Forgetting to Copy Path
```python
# WRONG
result.append(path)  # All results reference same list!

# CORRECT
result.append(path[:])  # Create a copy
```

### 2. Not Backtracking Properly
```python
# WRONG
path.append(num)
backtrack(...)
# Forgot to pop!

# CORRECT
path.append(num)
backtrack(...)
path.pop()  # Must undo choice
```

### 3. Wrong Base Case
```python
# WRONG - Combinations
if start > n:  # Too late!

# CORRECT
if len(path) == k:  # Right time
```

### 4. Modifying Input Without Restoring
```python
# In Word Search
temp = board[r][c]
board[r][c] = '#'
backtrack(...)
board[r][c] = temp  # Must restore!
```

---

## Interview Tips

1. **Draw the decision tree** - Helps visualize the problem
2. **Start simple** - Begin with small example (n=2, k=1)
3. **Identify the pattern** - Is it subsets, combinations, or permutations?
4. **Think about pruning** - How can you stop early?
5. **Trace your code** - Walk through one complete path


-------
-------

# Complete Backtracking Guide with Visual Representations

## Table of Contents
1. [Introduction to Backtracking](#introduction-to-backtracking)
2. [Permutations vs Combinations](#permutations-vs-combinations)
3. [Core Backtracking Problems](#core-backtracking-problems)
4. [Linked Lists Section](#linked-lists-section)
5. [Advanced Concepts](#advanced-concepts)

---

## Introduction to Backtracking

### What is Backtracking?

**Backtracking** is like exploring a maze where you try different paths, and if you hit a dead end, you go back and try a different route.

```
Visual Representation of Backtracking:

Start → Choice 1 → Choice 1.1 → Success! ✓
     ↓
     Choice 2 → Choice 2.1 → Dead End ✗
             ↓                    ↑
             Choice 2.2 ────────┘ (Backtrack)
                    ↓
                    Success! ✓
```

### Real-World Examples

**Example 1: Solving a Sudoku Puzzle**

```
Step 1: Try placing '1' in cell
Step 2: Continue filling based on that choice
Step 3: Hit a conflict? Backtrack!
Step 4: Try '2' instead
Step 5: Continue until solved
```

**Example 2: Planning a Road Trip**

```
City A → City B → City C → City D (Destination)
      ↓
      City E → City F → Dead End!
               ↑
               Backtrack to City A
      ↓
      City G → City H → City D (Success!)
```

### Decision Tree Visualization

Every backtracking problem can be represented as a **decision tree**:

```
                    [Empty]
                   /       \
              [Add A]    [Skip A]
              /    \       /    \
         [Add B] [Skip B] ...  ...
```

**Key Terms:**
- **Node**: Represents a state (current choices made)
- **Edge**: Represents making a choice
- **Leaf**: Represents a complete solution or dead end
- **Backtracking**: Moving back up the tree to try different branches

---

## Permutations vs Combinations

### Understanding the Difference

Think of it this way:
- **Permutations**: Like arranging people in a line for a photo (order matters)
- **Combinations**: Like choosing pizza toppings (order doesn't matter)

### Visual Comparison

**For elements [1, 2, 3]:**

#### Permutations (Order Matters):
```
       Start
      /  |  \
    1    2    3
   / \  / \  / \
  2  3 1  3 1  2
  |  | |  | |  |
  3  2 3  1 2  1

Results: [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]
Total: 3! = 6 permutations
```

#### Combinations (Order Doesn't Matter):
```
Choosing 2 elements:

       Start
      /  |  \
    1    2    3
   / \   |    
  2   3  3   

Results: [1,2], [1,3], [2,3]
Total: C(3,2) = 3 combinations

Note: [1,2] and [2,1] are considered the same!
```

### Formulas

**Permutations:**
```
P(n, r) = n! / (n - r)!

Example: Arrange 2 items from [1,2,3]
P(3, 2) = 3! / (3-2)! = 6 / 1 = 6
Results: [1,2], [1,3], [2,1], [2,3], [3,1], [3,2]
```

**Combinations:**
```
C(n, r) = n! / (r! × (n - r)!)

Example: Choose 2 items from [1,2,3]
C(3, 2) = 3! / (2! × 1!) = 6 / 2 = 3
Results: [1,2], [1,3], [2,3]
```

---

## Core Backtracking Problems

### Problem 1: Letter Case Permutation

**[LeetCode 784 - Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)**

#### Understanding the Problem

For string "a1b2":
- 'a' can be: 'a' or 'A' (2 choices)
- '1' can be: '1' only (1 choice)
- 'b' can be: 'b' or 'B' (2 choices)
- '2' can be: '2' only (1 choice)

**Total possibilities**: 2 × 1 × 2 × 1 = 4

#### Decision Tree Visualization

```
                    "a1b2"
                   /      \
              "a1b2"      "A1b2"
             /    \       /    \
        "a1b2" "a1B2" "A1b2" "A1B2"
```

**Step-by-step breakdown:**
1. Start with "a1b2"
2. At 'a': Branch into lowercase and uppercase
3. Skip '1' (digit, no choice)
4. At 'b': Each existing result branches again
5. Skip '2' (digit)

#### Visual Trace for "ab"

```
Level 0:           ""
                   |
Level 1:          "a"
                 /   \
Level 2:      "ab"   "aB"
                      
Backtrack:     
                   ""
                   |
                  "A"
                 /   \
             "Ab"   "AB"

Final: ["ab", "aB", "Ab", "AB"]
```

#### Approach 1: Iterative

**Why this works:**

```
Initial: ["a1b2"]

Step 1 - Process 'a':
  Current: ["a1b2"]
  Add uppercase version: ["a1b2", "A1b2"]
  
Step 2 - Skip '1' (digit)

Step 3 - Process 'b':
  Current: ["a1b2", "A1b2"]
  For "a1b2" → add "a1B2"
  For "A1b2" → add "A1B2"
  Result: ["a1b2", "A1b2", "a1B2", "A1B2"]
```

**Code:**
```python
def letterCasePermutation(s: str) -> list[str]:
    result = [s]
    
    for i in range(len(s)):
        if s[i].isalpha():
            # Store current size to avoid infinite loop
            n = len(result)
            for j in range(n):
                # Create a list from string for modification
                chars = list(result[j])
                # Toggle the case
                chars[i] = chars[i].swapcase()
                result.append(''.join(chars))
    
    return result

# Time: O(n × 2^L) where L = number of letters
# Space: O(n × 2^L) for storing results
```

#### Approach 2: Recursive Backtracking

**Decision at each step:**

```
At index i:
  - If letter: Try both lowercase AND uppercase
  - If digit: Only one choice
  - Recurse to next index
```

**Recursion Tree for "a1":**

```
                backtrack(0, [])
                /              \
       append('a')            append('A')
       backtrack(1,['a'])     backtrack(1,['A'])
              |                      |
       append('1')            append('1')
       backtrack(2,['a','1']) backtrack(2,['A','1'])
              |                      |
       add "a1" to result     add "A1" to result
```

**Code:**
```python
def letterCasePermutation(s: str) -> list[str]:
    result = []
    
    def backtrack(index, path):
        # Base case: processed entire string
        if index == len(s):
            result.append(''.join(path))
            return
        
        char = s[index]
        
        if char.isalpha():
            # Choice 1: lowercase
            path.append(char.lower())
            backtrack(index + 1, path)
            path.pop()  # BACKTRACK
            
            # Choice 2: uppercase
            path.append(char.upper())
            backtrack(index + 1, path)
            path.pop()  # BACKTRACK
        else:
            # Digit: only one choice
            path.append(char)
            backtrack(index + 1, path)
            path.pop()  # BACKTRACK
    
    backtrack(0, [])
    return result

# Time: O(n × 2^L) where L = number of letters
# Space: O(n) for recursion stack
```

**Detailed Trace for "ab":**

```
Call Stack                           Path        Result
─────────────────────────────────────────────────────────
backtrack(0, [])                     []          []
├─ append('a')                       ['a']       []
│  └─ backtrack(1, ['a'])           ['a']       []
│     ├─ append('b')                ['a','b']   []
│     │  └─ backtrack(2,['a','b'])  ['a','b']   []
│     │     └─ len==2, add "ab"     ['a','b']   ["ab"]
│     │  └─ pop() → path = ['a']   ['a']       ["ab"]
│     ├─ append('B')                ['a','B']   ["ab"]
│     │  └─ backtrack(2,['a','B'])  ['a','B']   ["ab"]
│     │     └─ len==2, add "aB"     ['a','B']   ["ab","aB"]
│     │  └─ pop() → path = ['a']   ['a']       ["ab","aB"]
│  └─ pop() → path = []             []          ["ab","aB"]
├─ append('A')                       ['A']       ["ab","aB"]
│  └─ backtrack(1, ['A'])           ['A']       ["ab","aB"]
│     ├─ append('b')                ['A','b']   ["ab","aB"]
│     │  └─ backtrack(2,['A','b'])  ['A','b']   ["ab","aB"]
│     │     └─ len==2, add "Ab"     ['A','b']   ["ab","aB","Ab"]
│     │  └─ pop() → path = ['A']   ['A']       ["ab","aB","Ab"]
│     ├─ append('B')                ['A','B']   ["ab","aB","Ab"]
│     │  └─ backtrack(2,['A','B'])  ['A','B']   ["ab","aB","Ab"]
│     │     └─ len==2, add "AB"     ['A','B']   ["ab","aB","Ab","AB"]

Final Result: ["ab", "aB", "Ab", "AB"]
```

---

### Problem 2: Subsets

**[LeetCode 78 - Subsets](https://leetcode.com/problems/subsets/)**

#### Understanding Subsets

A **subset** is any combination of elements, including empty set.

**For [1,2,3], all subsets:**
```
[]           - Empty set
[1]          - Single elements
[2]
[3]
[1,2]        - Pairs
[1,3]
[2,3]
[1,2,3]      - All elements

Total: 2^3 = 8 subsets
```

#### Why 2^n subsets?

**Each element has 2 choices:**
```
Element 1: Include or Exclude
Element 2: Include or Exclude  
Element 3: Include or Exclude

Total combinations: 2 × 2 × 2 = 2^3 = 8
```

#### Complete Decision Tree for [1,2,3]

```
                        []
                       /  \
                Include 1  Exclude 1
                    /          \
                  [1]           []
                 /  \          /  \
            Inc 2  Exc 2   Inc 2  Exc 2
              /      \       /      \
           [1,2]    [1]    [2]      []
           /  \     /  \   /  \    /  \
        I3  E3   I3  E3 I3 E3  I3  E3
        /    \   /    \ /   \  /    \
    [1,2,3][1,2][1,3][1][2,3][2][3] []

Final: [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

#### Approach: Backtracking

**Key Insight**: Use `start` index to avoid duplicates!

**Without start index:**
```
Would generate:
[1,2] and [2,1] ← duplicates!
[1,3] and [3,1] ← duplicates!
```

**With start index:**
```
After choosing 1:
  Only consider elements from index 1 onwards: [2, 3]
After choosing 2:
  Only consider elements from index 2 onwards: [3]
```

**Visual trace for [1,2,3]:**

```
backtrack(start=0, path=[])
│
├─ Add [] to result
│
├─ i=0, num=1
│  ├─ path=[1]
│  ├─ backtrack(start=1, path=[1])
│  │  ├─ Add [1] to result
│  │  ├─ i=1, num=2
│  │  │  ├─ path=[1,2]
│  │  │  ├─ backtrack(start=2, path=[1,2])
│  │  │  │  ├─ Add [1,2] to result
│  │  │  │  ├─ i=2, num=3
│  │  │  │  │  ├─ path=[1,2,3]
│  │  │  │  │  ├─ backtrack(start=3, path=[1,2,3])
│  │  │  │  │  │  └─ Add [1,2,3] to result
│  │  │  │  │  └─ pop() → path=[1,2]
│  │  │  └─ pop() → path=[1]
│  │  ├─ i=2, num=3
│  │  │  ├─ path=[1,3]
│  │  │  ├─ backtrack(start=3, path=[1,3])
│  │  │  │  └─ Add [1,3] to result
│  │  │  └─ pop() → path=[1]
│  └─ pop() → path=[]
│
├─ i=1, num=2
│  ├─ path=[2]
│  ├─ backtrack(start=2, path=[2])
│  │  ├─ Add [2] to result
│  │  ├─ i=2, num=3
│  │  │  ├─ path=[2,3]
│  │  │  ├─ backtrack(start=3, path=[2,3])
│  │  │  │  └─ Add [2,3] to result
│  │  │  └─ pop() → path=[2]
│  └─ pop() → path=[]
│
└─ i=2, num=3
   ├─ path=[3]
   ├─ backtrack(start=3, path=[3])
   │  └─ Add [3] to result
   └─ pop() → path=[]

Result: [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

**Code:**
```python
def subsets(nums: list[int]) -> list[list[int]]:
    result = []
    
    def backtrack(start, path):
        # Every path is a valid subset
        result.append(path[:])  # Copy the path!
        
        # Try adding each remaining element
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)  # Next index
            path.pop()  # Backtrack
    
    backtrack(0, [])
    return result

# Time: O(n × 2^n) - generate 2^n subsets, each takes O(n) to copy
# Space: O(n) for recursion depth
```

**Why path[:] (copying)?**

```
Without copying:                  With copying:
─────────────────                ─────────────────
path = []                         path = []
result.append(path)               result.append(path[:])
result = [[]]                     result = [[]]

path.append(1)                    path.append(1)
result.append(path)               result.append(path[:])
result = [[], [1]]                result = [[], [1]]

path.append(2)                    path.append(2)
result.append(path)               result.append(path[:])
result = [[], [1], [1,2]]         result = [[], [1], [1,2]]

path.pop() → path = [1]           path.pop() → path = [1]
path.pop() → path = []            path.pop() → path = []

Final result:                     Final result:
[[], [], []]  ✗ WRONG!            [[], [1], [1,2]]  ✓ CORRECT!
All reference same list!          Each is independent copy!
```

---

### Problem 3: Combinations

**[LeetCode 77 - Combinations](https://leetcode.com/problems/combinations/)**

#### Understanding Combinations

**Given n=4, k=2:**

Choose 2 numbers from [1,2,3,4]:

```
Start with 1:      Start with 2:      Start with 3:
  [1,2]              [2,3]              [3,4]
  [1,3]              [2,4]
  [1,4]

Total: C(4,2) = 6 combinations
```

#### Why not [2,1], [3,1], [4,1]?

Because **order doesn't matter** in combinations!
- [1,2] is same as [2,1]
- We use `start` index to ensure we only move forward

#### Decision Tree for n=4, k=2

```
                    []
          /    |    |    \
        1      2    3     4
       /|\    /|    |
      2 3 4  3 4    4
      
Results:
From 1: [1,2], [1,3], [1,4]
From 2: [2,3], [2,4]
From 3: [3,4]
From 4: Can't form k=2 (need 2 elements, only 1 left)

Total: 6 combinations
```

#### Pruning Optimization

**Without pruning:**
```
start=4, path=[1], need 1 more
Range: [4, 5) → try 4
  Add 4 → [1,4] ✓ Valid

start=4, path=[], need 2 more
Range: [4, 5) → try 4
  Add 4 → [4], need 1 more
  But no more numbers! ✗ Wasted work
```

**With pruning:**
```
start=4, path=[], need=2, remain=1
1 < 2, so skip entirely! ✓ Saved recursion
```

**Pruning visualization:**

```
For n=5, k=3:

Current state: start=4, path=[1]
├─ need = k - len(path) = 3 - 1 = 2
├─ remain = n - start + 1 = 5 - 4 + 1 = 2
└─ remain (2) >= need (2) ✓ Continue

Current state: start=5, path=[1]
├─ need = k - len(path) = 3 - 1 = 2
├─ remain = n - start + 1 = 5 - 5 + 1 = 1
└─ remain (1) < need (2) ✗ Prune! Stop here
```

**Code:**
```python
def combine(n: int, k: int) -> list[list[int]]:
    result = []
    
    def backtrack(start, path):
        # Base case: combination complete
        if len(path) == k:
            result.append(path[:])
            return
        
        # Pruning calculation
        need = k - len(path)
        remain = n - start + 1
        
        for i in range(start, n + 1):
            # Prune if not enough numbers left
            if remain < need:
                break
            
            path.append(i)
            backtrack(i + 1, path)
            path.pop()
            
            remain -= 1  # One less number available
    
    backtrack(1, [])
    return result

# Time: O(C(n,k) × k)
# Space: O(k) for recursion depth
```

**Detailed trace for n=4, k=2:**

```
backtrack(1, [])
need=2, remain=4

├─ i=1: remain(4) >= need(2) ✓
│  path=[1], backtrack(2, [1])
│  need=1, remain=3
│  │
│  ├─ i=2: remain(3) >= need(1) ✓
│  │  path=[1,2], len==k
│  │  Add [1,2], return
│  │
│  ├─ i=3: remain(2) >= need(1) ✓
│  │  path=[1,3], len==k
│  │  Add [1,3], return
│  │
│  └─ i=4: remain(1) >= need(1) ✓
│     path=[1,4], len==k
│     Add [1,4], return

├─ i=2: remain(3) >= need(2) ✓
│  path=[2], backtrack(3, [2])
│  need=1, remain=2
│  │
│  ├─ i=3: remain(2) >= need(1) ✓
│  │  path=[2,3], len==k
│  │  Add [2,3], return
│  │
│  └─ i=4: remain(1) >= need(1) ✓
│     path=[2,4], len==k
│     Add [2,4], return

├─ i=3: remain(2) >= need(2) ✓
│  path=[3], backtrack(4, [3])
│  need=1, remain=1
│  │
│  └─ i=4: remain(1) >= need(1) ✓
│     path=[3,4], len==k
│     Add [3,4], return

└─ i=4: remain(1) < need(2) ✗ PRUNED

Result: [[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
```

---

### Problem 4: Permutations

**[LeetCode 46 - Permutations](https://leetcode.com/problems/permutations/)**

#### Understanding Permutations

**For [1,2,3]:**

```
Position 1: 3 choices (1, 2, or 3)
Position 2: 2 choices (remaining)
Position 3: 1 choice (last one)

Total: 3 × 2 × 1 = 6 permutations
```

**All 6 permutations:**
```
[1,2,3]  [1,3,2]
[2,1,3]  [2,3,1]
[3,1,2]  [3,2,1]
```

#### Why Different from Combinations?

**Combinations [1,2,3], k=2:**
- [1,2] is same as [2,1]
- Result: [1,2], [1,3], [2,3]

**Permutations [1,2,3]:**
- [1,2,3] is different from [3,2,1]
- Result: All 6 arrangements above

#### Complete Decision Tree for [1,2,3]

```
                        []
            /           |           \
          1             2             3
        /   \         /   \         /   \
      2      3      1      3      1      2
      |      |      |      |      |      |
      3      2      3      1      2      1
      
Results:
[1,2,3]  [1,3,2]  [2,1,3]  [2,3,1]  [3,1,2]  [3,2,1]
```

#### Approach 1: With Used Array

**Why we need `used` array:**

```
Without used array:
├─ Pick 1: [1]
│  ├─ Pick 1 again: [1,1] ✗ Wrong! (reusing element)
│  └─ Pick 2: [1,2] ✓

With used array:
├─ Pick 1, mark used[0]=True: [1]
│  ├─ Try 1: used[0]=True, skip ✓
│  └─ Try 2: used[1]=False, pick ✓ [1,2]
```

**Visual representation of used array:**

```
nums = [1, 2, 3]
used = [F, F, F]  ← All available initially

Step 1: Choose 1
nums = [1, 2, 3]
used = [T, F, F]  ← 1 is now used
path = [1]

Step 2: Choose 2
nums = [1, 2, 3]
used = [T, T, F]  ← 1 and 2 are used
path = [1, 2]

Step 3: Choose 3
nums = [1, 2, 3]
used = [T, T, T]  ← All used
path = [1, 2, 3]  ← Complete permutation!

Backtrack to Step 2:
nums = [1, 2, 3]
used = [T, F, F]  ← Reset to after Step 1
path = [1]        ← Try different second choice
```

**Code:**
```python
def permute(nums: list[int]) -> list[list[int]]:
    result = []
    used = [False] * len(nums)
    
    def backtrack(path):
        # Base case
        if len(path) == len(nums):
            result.append(path[:])
            return
        
        # Try each element
        for i in range(len(nums)):
            if used[i]:
                continue  # Skip used elements
            
            # Choose
            path.append(nums[i])
            used[i] = True
            
            # Explore
            backtrack(path)
            
            # Unchoose (backtrack)
            path.pop()
            used[i] = False
    
    backtrack([])
    return result

# Time: O(n! × n)
# Space: O(n) for recursion + used array
```

**Detailed execution trace:**

```
nums=[1,2,3], used=[F,F,F], path=[]

backtrack([])
├─ i=0: used[0]=F, choose 1
│  path=[1], used=[T,F,F]
│  backtrack([1])
│  │
│  ├─ i=0: used[0]=T, skip
│  ├─ i=1: used[1]=F, choose 2
│  │  path=[1,2], used=[T,T,F]
│  │  backtrack([1,2])
│  │  │
│  │  ├─ i=0: used[0]=T, skip
│  │  ├─ i=1: used[1]=T, skip
│  │  └─ i=2: used[2]=F, choose 3
│  │     path=[1,2,3], used=[T,T,T]
│  │     backtrack([1,2,3])
│  │     └─ len(path)==3, add [1,2,3] ✓
│  │     path=[1,2], used=[T,T,F] (backtracked)
│  │  path=[1], used=[T,F,F] (backtracked)
│  │
│  └─ i=2: used[2]=F, choose 3
│     path=[1,3], used=[T,F,T]
│     backtrack([1,3])
│     │
│     ├─ i=0: used[0]=T, skip
│     ├─ i=1: used[1]=F, choose 2
│     │  path=[1,3,2], used=[T,T,T]
│     │  backtrack([1,3,2])
│     │  └─ len(path)==3, add [1,3,2] ✓
│     │  path=[1,3], used=[T,F,T] (backtracked)
│     └─ i=2: used[2]=T, skip
│     path=[1], used=[T,F,F] (backtracked)
│  path=[], used=[F,F,F] (backtracked)
│
├─ i=1: used[1]=F, choose 2
│  ... (similar pattern, generates [2,1,3] and [2,3,1])
│
└─ i=2: used[2]=F, choose 3
   ... (similar pattern, generates [3,1,2] and [3,2,1])

Final: [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
```

---

## The Universal Backtracking Template

### The Pattern

```python
def backtrack(state, choices):
    """
    state: Current state (path, sum, etc.)
    choices: Available options to explore
    """
    
    # 1. BASE CASE: Check if solution is complete
    if is_solution_complete(state):
        save_solution(state)
        return
    
    # 2. PRUNING: Stop early if path cannot lead to solution
    if should_prune(state):
        return
    
    # 3. ITERATE through choices
    for choice in get_available_choices(choices):
        # 4. MAKE CHOICE: Update state
        make_choice(state, choice)
        
        # 5. RECURSE: Explore with this choice
        backtrack(updated_state, remaining_choices)
        
        # 6. UNDO CHOICE: Backtrack
        undo_choice(state, choice)
```

### Visual Representation

```
                    [Initial State]
                    /    |    \
            [Choice 1] [Choice 2] [Choice 3]
           /    \      |    |      /    \
      [C1.1] [C1.2] [C2.1] [C2.2] ...  ...
        |      |      |      |
    [Solution][X]  [Solution][X]

Legend:
[Solution] = Valid solution found
[X] = Dead end, backtrack
```

### Comparing Different Problems

| Problem | Base Case | Choices | State | Backtrack Action |
|---------|-----------|---------|-------|------------------|
| **Subsets** | Always valid | Elements from `start` | Current subset | Remove last element |6. **Test edge cases** - Empty input, n=1, k=n, etc.

Remember: Backtracking is about **trying all possibilities systematically**. When you hit a dead end, you backtrack and try something else. With practice, the pattern becomes second nature!
