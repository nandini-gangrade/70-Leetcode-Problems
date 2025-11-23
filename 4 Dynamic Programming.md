# Dynamic Programming - Complete Beginner's Deep Dive

Dynamic Programming (DP) is one of the most important topics for coding interviews at top tech companies. It might seem intimidating at first, but once you understand the core concepts and patterns, it becomes much more approachable.

As mentioned in the video: **"Dynamic programming is a technique used to solve complex problems by breaking them down into simpler subproblems. It works by solving each subproblem only once, storing the solution to that subproblem so that when the solution is needed again, it can be looked up instead of computing the answer all over again."**

---

## Table of Contents
1. [What is Dynamic Programming?](#1-what-is-dynamic-programming)
2. [When to Use Dynamic Programming](#2-when-to-use-dynamic-programming)
3. [Two Approaches: Top-Down vs Bottom-Up](#3-two-approaches-top-down-vs-bottom-up)
4. [Climbing Stairs](#4-climbing-stairs)
5. [Coin Change](#5-coin-change)
6. [Maximum Subarray (Kadane's Algorithm)](#6-maximum-subarray-kadanes-algorithm)
7. [Counting Bits](#7-counting-bits)
8. [Range Sum Query - Immutable](#8-range-sum-query---immutable)
9. [More Essential DP Problems](#9-more-essential-dp-problems)
10. [DP Patterns Summary](#10-dp-patterns-summary)
11. [Practice Problems](#11-practice-problems)

---

## 1. What is Dynamic Programming?

### 1.1 The Simple Definition

**Dynamic Programming (DP)** is a problem-solving technique where you:
1. Break a big problem into smaller **subproblems**
2. Solve each subproblem **only once**
3. **Store** the solution so you don't recalculate it
4. Use stored solutions to build up to the final answer

### 1.2 A Real-Life Analogy

Imagine you're asked: **"What's 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1?"**

You count: "That's 8."

Now someone asks: **"What's 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1?"** (one more 1)

Do you count from scratch? No! You remember the previous answer (8) and just add 1 to get 9.

**That's dynamic programming!** You stored (memorized) the previous result and reused it.

### 1.3 The Two Key Properties

A problem can be solved with DP if it has:

#### Property 1: Optimal Substructure

The optimal solution to the problem can be constructed from optimal solutions of its subproblems.

**Example**: The shortest path from A to C through B = (shortest path A to B) + (shortest path B to C)

#### Property 2: Overlapping Subproblems

The same subproblems are solved multiple times.

**Example**: Computing Fibonacci(5) requires Fibonacci(4) and Fibonacci(3). Computing Fibonacci(4) also requires Fibonacci(3). We're computing Fibonacci(3) twice!

```
                    fib(5)
                   /      \
              fib(4)      fib(3)
             /    \       /    \
         fib(3)  fib(2) fib(2) fib(1)
         /   \
     fib(2) fib(1)
```

See how `fib(3)` and `fib(2)` appear multiple times? That's overlapping subproblems!

### 1.4 What is Memoization?

**Memoization** is just a fancy word for "storing results so we don't recalculate them."

The term comes from "memo" - like writing yourself a note to remember something.

```python
# Without memoization - recalculates same values many times
def fib_slow(n):
    if n <= 1:
        return n
    return fib_slow(n-1) + fib_slow(n-2)

# With memoization - stores results in a dictionary
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]  # Return stored result
    if n <= 1:
        return n
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]
```

---

## 2. When to Use Dynamic Programming

### 2.1 Keywords That Hint at DP

Look for these words in problem statements:

| Keyword | Why It Suggests DP |
|---------|-------------------|
| "Minimum" / "Maximum" | Optimization problems |
| "Count the number of ways" | Counting problems |
| "Is it possible" | Decision problems |
| "Longest" / "Shortest" | Sequence problems |
| "All possible" | May need to explore all options |

### 2.2 Problem Types That Often Use DP

1. **Optimization**: Find min/max value (Coin Change, Knapsack)
2. **Counting**: Count number of ways (Climbing Stairs, Unique Paths)
3. **Existence**: Is something possible? (Word Break)
4. **Sequence**: Longest/shortest sequence (LCS, LIS)

### 2.3 How to Recognize a DP Problem

Ask yourself:
1. "Can I break this into smaller similar problems?"
2. "Will I solve the same smaller problem multiple times?"
3. "Can I build the answer from answers to smaller problems?"

If yes to all three → likely a DP problem!

---

## 3. Two Approaches: Top-Down vs Bottom-Up

### 3.1 Top-Down (Memoization)

**Approach**: Start with the big problem, recursively break it down, store results.

**Think of it as**: "I need to solve the big problem. To do that, I need to solve smaller problems first. Let me remember solutions as I go."

```python
def solve_top_down(n, memo={}):
    # Base case
    if n == 0:
        return base_value
    
    # Check if already solved
    if n in memo:
        return memo[n]
    
    # Solve and store
    memo[n] = solve_top_down(n-1, memo) + something
    return memo[n]
```

**Pros**:
- More intuitive (follows recursive thinking)
- Only solves subproblems that are needed

**Cons**:
- Recursion overhead (function call stack)
- Risk of stack overflow for very large inputs

### 3.2 Bottom-Up (Tabulation)

**Approach**: Start with smallest subproblems, build up to the big problem.

**Think of it as**: "Let me solve all the small problems first, then use those solutions to solve bigger and bigger problems until I reach my answer."

```python
def solve_bottom_up(n):
    # Create table to store solutions
    dp = [0] * (n + 1)
    
    # Base case
    dp[0] = base_value
    
    # Fill table from small to large
    for i in range(1, n + 1):
        dp[i] = dp[i-1] + something
    
    return dp[n]
```

**Pros**:
- No recursion overhead
- Often faster in practice
- No stack overflow risk

**Cons**:
- May solve subproblems we don't need
- Sometimes harder to think about

### 3.3 Which Should You Use?

For interviews, the video creator recommends **bottom-up** because:
- "I typically don't like recursive algorithms - they're harder to read and debug"
- "They can take up a lot of space in memory which can cause stack overflow"

However, for learning, top-down is often easier to understand first.

---

## 4. Climbing Stairs

📌 **LeetCode Link**: [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

### 4.1 Problem Statement

You are climbing a staircase. It takes `n` steps to reach the top. Each time you can either climb **1 step** or **2 steps**. In how many distinct ways can you climb to the top?

### 4.2 Examples

```
Example 1:
Input: n = 2
Output: 2
Explanation: Two ways to climb:
1. 1 step + 1 step
2. 2 steps

Example 2:
Input: n = 3
Output: 3
Explanation: Three ways to climb:
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

### 4.3 Understanding the Problem

Let's visualize for n=4:

```
Step 4: ████  (Goal)
Step 3: ███
Step 2: ██
Step 1: █
Step 0: (Start)

To reach Step 4, I can:
- Be at Step 3 and take 1 step, OR
- Be at Step 2 and take 2 steps

So: ways(4) = ways(3) + ways(2)
```

### 4.4 Approach 1: Brute Force Recursion

**Why would I think of this?**
"To reach step n, I must have come from step n-1 (took 1 step) or step n-2 (took 2 steps). So total ways = ways to reach n-1 + ways to reach n-2."

```python
def climbStairs_brute(n):
    if n <= 2:
        return n
    return climbStairs_brute(n-1) + climbStairs_brute(n-2)
```

**Why does this work?**
- Base cases: 1 way to climb 1 step, 2 ways to climb 2 steps
- For any n > 2: total ways = ways(n-1) + ways(n-2)

**Why is this terrible?**
- **Time**: O(2^n) - Exponential! Each call spawns two more calls
- For n=40, this takes forever

Let's see why with a call tree:

```
                    climb(5)
                   /        \
            climb(4)        climb(3)
           /       \        /       \
      climb(3)  climb(2) climb(2) climb(1)
      /     \
  climb(2) climb(1)
```

We're computing `climb(3)` and `climb(2)` multiple times! That's waste.

### 4.5 Approach 2: Top-Down DP (Memoization)

**Why would I think of this?**
"The brute force recalculates the same values. What if I store results as I compute them?"

```python
def climbStairs_memo(n, memo={}):
    if n <= 2:
        return n
    
    if n in memo:
        return memo[n]  # Already computed!
    
    memo[n] = climbStairs_memo(n-1, memo) + climbStairs_memo(n-2, memo)
    return memo[n]
```

**Analysis**:
- **Time**: O(n) - Each subproblem computed only once
- **Space**: O(n) - Memo dictionary + recursion stack

### 4.6 Approach 3: Bottom-Up DP (Recommended)

**Why would I think of this?**
"Instead of starting from n and going down, let me start from 0 and build up. I'll store solutions in an array."

```python
def climbStairs(n):
    if n <= 2:
        return n
    
    # dp[i] = number of ways to reach step i
    dp = [0] * (n + 1)
    
    # Base cases
    dp[1] = 1  # 1 way to reach step 1
    dp[2] = 2  # 2 ways to reach step 2
    
    # Fill table
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(n)

**Walkthrough for n = 5**:

```
dp = [0, 0, 0, 0, 0, 0]  (size n+1 = 6)

Base cases:
dp[1] = 1
dp[2] = 2
dp = [0, 1, 2, 0, 0, 0]

i = 3:
dp[3] = dp[2] + dp[1] = 2 + 1 = 3
dp = [0, 1, 2, 3, 0, 0]

i = 4:
dp[4] = dp[3] + dp[2] = 3 + 2 = 5
dp = [0, 1, 2, 3, 5, 0]

i = 5:
dp[5] = dp[4] + dp[3] = 5 + 3 = 8
dp = [0, 1, 2, 3, 5, 8]

Return dp[5] = 8
```

### 4.7 Approach 4: Space Optimized

**Why would I think of this?**
"Looking at my formula `dp[i] = dp[i-1] + dp[i-2]`, I only need the previous TWO values. Why store the entire array?"

```python
def climbStairs_optimized(n):
    if n <= 2:
        return n
    
    prev2 = 1  # dp[i-2]
    prev1 = 2  # dp[i-1]
    
    for i in range(3, n + 1):
        current = prev1 + prev2
        prev2 = prev1
        prev1 = current
    
    return prev1
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(1) ✓

**Walkthrough for n = 5**:

```
prev2 = 1 (ways to reach step 1)
prev1 = 2 (ways to reach step 2)

i = 3:
  current = 2 + 1 = 3
  prev2 = 2, prev1 = 3

i = 4:
  current = 3 + 2 = 5
  prev2 = 3, prev1 = 5

i = 5:
  current = 5 + 3 = 8
  prev2 = 5, prev1 = 8

Return prev1 = 8 ✓
```

This is actually the **Fibonacci sequence**! (1, 1, 2, 3, 5, 8, 13...)

---

## 5. Coin Change

📌 **LeetCode Link**: [322. Coin Change](https://leetcode.com/problems/coin-change/)

This is one of the classic DP problems that the video covers in detail!

### 5.1 Problem Statement

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the **fewest number of coins** that you need to make up that amount. If that amount cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an **infinite number** of each kind of coin.

### 5.2 Examples

```
Example 1:
Input: coins = [1, 2, 5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1 (three coins)

Example 2:
Input: coins = [2], amount = 3
Output: -1
Explanation: Can't make 3 with only 2-cent coins

Example 3:
Input: coins = [1], amount = 0
Output: 0
Explanation: 0 coins needed for amount 0
```

### 5.3 Understanding the Problem

We want the MINIMUM number of coins, not just any valid combination.

For amount = 11 with coins [1, 2, 5]:
- 11 ones = 11 coins ❌ (works but not minimum)
- 5 + 5 + 1 = 3 coins ✓ (minimum!)

### 5.4 Approach 1: Greedy (DOESN'T ALWAYS WORK!)

**Why would I think of this?**
"To minimize coins, always use the biggest coin possible!"

```python
def coinChange_greedy(coins, amount):
    coins.sort(reverse=True)  # Largest first
    count = 0
    
    for coin in coins:
        count += amount // coin  # Use as many as possible
        amount %= coin           # Remaining amount
    
    return count if amount == 0 else -1
```

**Why does this FAIL?**

Consider coins = [1, 3, 4] and amount = 6:
- Greedy: 4 + 1 + 1 = 3 coins
- Optimal: 3 + 3 = 2 coins ✓

The greedy approach doesn't consider all possibilities!

### 5.5 Approach 2: Brute Force DFS

**Why would I think of this?**
"Let me try ALL possible combinations and find the minimum."

```python
def coinChange_dfs(coins, amount):
    def dfs(remaining):
        if remaining == 0:
            return 0
        if remaining < 0:
            return float('inf')
        
        min_coins = float('inf')
        for coin in coins:
            result = dfs(remaining - coin)
            min_coins = min(min_coins, result + 1)
        
        return min_coins
    
    result = dfs(amount)
    return result if result != float('inf') else -1
```

**Why does this work?**
For each amount, we try every coin and recursively solve for the remaining amount.

**Why is this too slow?**
- **Time**: O(S^n) where S = amount, n = number of coins
- Same subproblems are solved many times!

```
For coins=[1,2,5], amount=11:

              dfs(11)
             /   |   \
        dfs(10) dfs(9) dfs(6)
        /|\      /|\     /|\
       ... ... ... ...  ... ...
       
Many paths lead to the same amounts (like dfs(5), dfs(4), etc.)
```

### 5.6 Approach 3: Top-Down DP (Memoization)

**Why would I think of this?**
"The brute force recalculates dfs(5), dfs(4), etc. multiple times. Let me store results!"

```python
def coinChange_memo(coins, amount):
    memo = {}
    
    def dfs(remaining):
        if remaining == 0:
            return 0
        if remaining < 0:
            return float('inf')
        if remaining in memo:
            return memo[remaining]
        
        min_coins = float('inf')
        for coin in coins:
            result = dfs(remaining - coin)
            min_coins = min(min_coins, result + 1)
        
        memo[remaining] = min_coins
        return min_coins
    
    result = dfs(amount)
    return result if result != float('inf') else -1
```

**Analysis**:
- **Time**: O(amount × len(coins))
- **Space**: O(amount) for memo

### 5.7 Approach 4: Bottom-Up DP (Recommended) ⭐

**Why would I think of this?**
"Instead of starting from `amount` and going down, let me solve for amounts 0, 1, 2, ... up to `amount`."

From the video: "What we're going to do is start with the smallest target possible: 0, 1, 2, 3, all the way up to 11, and check the minimum number of coins for each target along the way."

```python
def coinChange(coins, amount):
    # dp[i] = minimum coins needed for amount i
    # Initialize with amount + 1 (impossible value, acts like infinity)
    dp = [amount + 1] * (amount + 1)
    
    # Base case: 0 coins needed for amount 0
    dp[0] = 0
    
    # For each amount from 1 to target
    for i in range(1, amount + 1):
        # Try each coin
        for coin in coins:
            if coin <= i:  # Can use this coin
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != amount + 1 else -1
```

**What is `amount + 1`?**
It's our "impossible" value (like infinity). Since we can never need more than `amount` coins (using all 1s), `amount + 1` is impossible. We use this instead of `float('inf')` because it works better with integer arrays.

**Analysis**:
- **Time**: O(amount × len(coins))
- **Space**: O(amount)

### 5.8 Detailed Walkthrough

Let's trace through coins = [1, 2, 5], amount = 11:

```
Initialize:
dp = [12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12]
     [0   1   2   3   4   5   6   7   8   9  10  11]  ← indices (amounts)

dp[0] = 0 (base case)
dp = [0, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12]

=== i = 1 (amount = 1) ===
Try coin 1: 1 <= 1 ✓
  dp[1] = min(dp[1], dp[1-1] + 1) = min(12, dp[0] + 1) = min(12, 1) = 1
Try coin 2: 2 <= 1 ✗ (coin too big)
Try coin 5: 5 <= 1 ✗ (coin too big)
dp = [0, 1, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12]

=== i = 2 (amount = 2) ===
Try coin 1: dp[2] = min(12, dp[1] + 1) = min(12, 2) = 2
Try coin 2: dp[2] = min(2, dp[0] + 1) = min(2, 1) = 1
Try coin 5: 5 <= 2 ✗
dp = [0, 1, 1, 12, 12, 12, 12, 12, 12, 12, 12, 12]

=== i = 3 ===
Try coin 1: dp[3] = min(12, dp[2] + 1) = min(12, 2) = 2
Try coin 2: dp[3] = min(2, dp[1] + 1) = min(2, 2) = 2
Try coin 5: ✗
dp = [0, 1, 1, 2, 12, 12, 12, 12, 12, 12, 12, 12]

=== i = 4 ===
Try coin 1: dp[4] = min(12, dp[3] + 1) = min(12, 3) = 3
Try coin 2: dp[4] = min(3, dp[2] + 1) = min(3, 2) = 2
Try coin 5: ✗
dp = [0, 1, 1, 2, 2, 12, 12, 12, 12, 12, 12, 12]

=== i = 5 ===
Try coin 1: dp[5] = min(12, dp[4] + 1) = 3
Try coin 2: dp[5] = min(3, dp[3] + 1) = 3
Try coin 5: dp[5] = min(3, dp[0] + 1) = min(3, 1) = 1  ← Just one 5-coin!
dp = [0, 1, 1, 2, 2, 1, 12, 12, 12, 12, 12, 12]

=== i = 6 ===
Try coin 1: dp[6] = min(12, dp[5] + 1) = 2
Try coin 2: dp[6] = min(2, dp[4] + 1) = min(2, 3) = 2
Try coin 5: dp[6] = min(2, dp[1] + 1) = 2
dp = [0, 1, 1, 2, 2, 1, 2, 12, 12, 12, 12, 12]

=== i = 7 ===
dp[7] = min(dp[6]+1, dp[5]+1, dp[2]+1) = min(3, 2, 2) = 2
dp = [0, 1, 1, 2, 2, 1, 2, 2, 12, 12, 12, 12]

=== i = 8 ===
dp[8] = min(dp[7]+1, dp[6]+1, dp[3]+1) = min(3, 3, 3) = 3
dp = [0, 1, 1, 2, 2, 1, 2, 2, 3, 12, 12, 12]

=== i = 9 ===
dp[9] = min(dp[8]+1, dp[7]+1, dp[4]+1) = min(4, 3, 3) = 3
dp = [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 12, 12]

=== i = 10 ===
dp[10] = min(dp[9]+1, dp[8]+1, dp[5]+1) = min(4, 4, 2) = 2
dp = [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 12]

=== i = 11 ===
dp[11] = min(dp[10]+1, dp[9]+1, dp[6]+1) = min(3, 4, 3) = 3
dp = [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3]

Final answer: dp[11] = 3 ✓
(That's 5 + 5 + 1 = 11 using 3 coins)
```

### 5.9 The Key Insight (From the Video)

The video explains it beautifully:

"For each number, our target minus the current number gives us the only value that works."

When we're at amount `i` and considering coin `c`:
- If we use coin `c`, we need to solve for amount `i - c`
- We already solved `dp[i - c]` earlier!
- So `dp[i] = dp[i - c] + 1` (use one coin `c` plus whatever we needed for `i - c`)

---

## 6. Maximum Subarray (Kadane's Algorithm)

📌 **LeetCode Link**: [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

### 6.1 Problem Statement

Given an integer array `nums`, find the **subarray** with the largest sum, and return its sum.

**What is a subarray?**
A subarray is a contiguous (connected) part of the array.

### 6.2 Examples

```
Example 1:
Input: nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: 6
Explanation: The subarray [4, -1, 2, 1] has the largest sum = 6

Example 2:
Input: nums = [1]
Output: 1

Example 3:
Input: nums = [5, 4, -1, 7, 8]
Output: 23
Explanation: The entire array [5, 4, -1, 7, 8] has sum 23
```

### 6.3 Approach 1: Brute Force

**Why would I think of this?**
"Let me try ALL possible subarrays and find the maximum sum."

```python
def maxSubArray_brute(nums):
    max_sum = float('-inf')
    n = len(nums)
    
    for i in range(n):  # Start of subarray
        for j in range(i, n):  # End of subarray
            current_sum = sum(nums[i:j+1])
            max_sum = max(max_sum, current_sum)
    
    return max_sum
```

**Why does this work?**
We check every possible subarray (defined by start and end indices).

**Why is this terrible?**
- **Time**: O(n³) - Two nested loops × sum calculation
- For n = 10,000, that's 10^12 operations!

### 6.4 Approach 2: Optimized Brute Force

**Why would I think of this?**
"Instead of recalculating sum each time, I can add incrementally."

```python
def maxSubArray_brute2(nums):
    max_sum = float('-inf')
    n = len(nums)
    
    for i in range(n):
        current_sum = 0
        for j in range(i, n):
            current_sum += nums[j]  # Add next element
            max_sum = max(max_sum, current_sum)
    
    return max_sum
```

**Analysis**:
- **Time**: O(n²) - Better, but still slow

### 6.5 Approach 3: Dynamic Programming (Kadane's Algorithm) ⭐

**Why would I think of this?**
"At each position, I have a choice: start a new subarray here, or extend the previous subarray. I should pick whichever gives the larger sum!"

**The Key Insight**:

For each position `i`, the maximum subarray ending at `i` is either:
1. Just `nums[i]` (start fresh)
2. `nums[i]` + maximum subarray ending at `i-1` (extend previous)

We pick whichever is larger!

```python
def maxSubArray(nums):
    # dp[i] = maximum subarray sum ending at index i
    n = len(nums)
    dp = [0] * n
    
    dp[0] = nums[0]  # Base case
    
    for i in range(1, n):
        # Either extend previous subarray or start new
        dp[i] = max(nums[i], dp[i-1] + nums[i])
    
    return max(dp)  # Return maximum of all endings
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(n)

**Walkthrough for [-2, 1, -3, 4, -1, 2, 1, -5, 4]**:

```
dp = [0, 0, 0, 0, 0, 0, 0, 0, 0]

dp[0] = -2 (base case)
dp = [-2, 0, 0, 0, 0, 0, 0, 0, 0]

i = 1, nums[1] = 1:
  dp[1] = max(1, dp[0] + 1) = max(1, -2 + 1) = max(1, -1) = 1
  "Start fresh with 1, because extending (-2 + 1 = -1) is worse"
dp = [-2, 1, 0, 0, 0, 0, 0, 0, 0]

i = 2, nums[2] = -3:
  dp[2] = max(-3, dp[1] + (-3)) = max(-3, 1 - 3) = max(-3, -2) = -2
  "Extend, because -2 is better than starting fresh with -3"
```
dp = [-2, 1, -2, 0, 0, 0, 0, 0, 0]

i = 3, nums[3] = 4:
  dp[3] = max(4, dp[2] + 4) = max(4, -2 + 4) = max(4, 2) = 4
  "Start fresh with 4, because extending (-2 + 4 = 2) is worse"
dp = [-2, 1, -2, 4, 0, 0, 0, 0, 0]

i = 4, nums[4] = -1:
  dp[4] = max(-1, dp[3] + (-1)) = max(-1, 4 - 1) = max(-1, 3) = 3
  "Extend, because 3 is better than starting fresh with -1"
dp = [-2, 1, -2, 4, 3, 0, 0, 0, 0]

i = 5, nums[5] = 2:
  dp[5] = max(2, dp[4] + 2) = max(2, 3 + 2) = max(2, 5) = 5
  "Extend"
dp = [-2, 1, -2, 4, 3, 5, 0, 0, 0]

i = 6, nums[6] = 1:
  dp[6] = max(1, dp[5] + 1) = max(1, 5 + 1) = max(1, 6) = 6
  "Extend"
dp = [-2, 1, -2, 4, 3, 5, 6, 0, 0]

i = 7, nums[7] = -5:
  dp[7] = max(-5, dp[6] + (-5)) = max(-5, 6 - 5) = max(-5, 1) = 1
  "Extend"
dp = [-2, 1, -2, 4, 3, 5, 6, 1, 0]

i = 8, nums[8] = 4:
  dp[8] = max(4, dp[7] + 4) = max(4, 1 + 4) = max(4, 5) = 5
  "Extend"
dp = [-2, 1, -2, 4, 3, 5, 6, 1, 5]

Maximum of dp array = 6 ✓
(This corresponds to subarray [4, -1, 2, 1])
```

### 6.6 Approach 4: Space Optimized Kadane's

**Why would I think of this?**
"Looking at my formula, I only need the previous dp value, not the entire array!"

```python
def maxSubArray_optimized(nums):
    max_sum = nums[0]      # Best sum found so far
    current_sum = nums[0]  # Best sum ending at current position
    
    for i in range(1, len(nums)):
        # Either extend or start fresh
        current_sum = max(nums[i], current_sum + nums[i])
        # Update global maximum
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(1) ✓

### 6.7 Understanding Why Kadane's Works

The brilliance is in recognizing:

1. **Local decision**: At each position, should I extend the previous subarray or start fresh?
   - If `current_sum + nums[i] > nums[i]`, extend (previous sum is positive contribution)
   - Otherwise, start fresh (previous sum is negative, would only hurt us)

2. **Global tracking**: We separately track the best we've seen anywhere.

**Visual intuition**:
```
Array: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

At index 3 (value 4):
  - Previous best ending: -2 (from subarray [1, -3])
  - Extending: -2 + 4 = 2
  - Starting fresh: 4
  - Decision: Start fresh! (4 > 2)

At index 6 (value 1):
  - Previous best ending: 5 (from subarray [4, -1, 2])
  - Extending: 5 + 1 = 6
  - Starting fresh: 1
  - Decision: Extend! (6 > 1)
```

---

## 7. Counting Bits

📌 **LeetCode Link**: [338. Counting Bits](https://leetcode.com/problems/counting-bits/)

We covered this in the Bit Manipulation section, but let's see it through the DP lens!

### 7.1 Problem Statement

Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i` (0 <= i <= n), `ans[i]` is the number of 1's in the binary representation of `i`.

### 7.2 Examples

```
Example 1:
Input: n = 2
Output: [0, 1, 1]
Explanation:
0 → 0    → 0 ones
1 → 1    → 1 one
2 → 10   → 1 one

Example 2:
Input: n = 5
Output: [0, 1, 1, 2, 1, 2]
Explanation:
0 → 0    → 0 ones
1 → 1    → 1 one
2 → 10   → 1 one
3 → 11   → 2 ones
4 → 100  → 1 one
5 → 101  → 2 ones
```

### 7.3 Approach 1: Count Each Number Individually

**Why would I think of this?**
"For each number 0 to n, count its 1 bits."

```python
def countBits_brute(n):
    result = []
    for i in range(n + 1):
        count = bin(i).count('1')  # Convert to binary string and count
        result.append(count)
    return result
```

**Alternative without string conversion**:
```python
def countBits_brute2(n):
    result = []
    for i in range(n + 1):
        count = 0
        num = i
        while num:
            count += num & 1
            num >>= 1
        result.append(count)
    return result
```

**Why is this not optimal?**
- **Time**: O(n log n) - For each of n numbers, we check up to log(n) bits
- The problem asks for O(n)!

### 7.4 Approach 2: DP Using Most Significant Bit (From the Video)

**Why would I think of this?**

As explained in the video, let's look for patterns:

```
Number | Binary | # of 1s | Pattern
-------|--------|---------|--------
0      | 0000   | 0       | Base case
1      | 0001   | 1       | = dp[0] + 1
2      | 0010   | 1       | = dp[0] + 1 (new power of 2)
3      | 0011   | 2       | = dp[1] + 1
4      | 0100   | 1       | = dp[0] + 1 (new power of 2)
5      | 0101   | 2       | = dp[1] + 1
6      | 0110   | 2       | = dp[2] + 1
7      | 0111   | 3       | = dp[3] + 1
8      | 1000   | 1       | = dp[0] + 1 (new power of 2)
```

**The Pattern**:
- At each power of 2 (1, 2, 4, 8...), there's exactly one 1 bit
- Numbers between powers of 2 follow: `dp[i] = dp[i - offset] + 1`
- Where `offset` is the most recent power of 2 ≤ i

```python
def countBits(n):
    dp = [0] * (n + 1)
    offset = 1  # Most recent power of 2
    
    for i in range(1, n + 1):
        # When we hit a new power of 2, update offset
        if offset * 2 == i:
            offset = i
        
        # Current count = count at (i - offset) + 1
        dp[i] = dp[i - offset] + 1
    
    return dp
```

**Analysis**:
- **Time**: O(n) ✓
- **Space**: O(1) extra (excluding output)

**Walkthrough for n = 5**:

```
dp = [0, 0, 0, 0, 0, 0]
offset = 1

i = 1:
  offset * 2 = 2 ≠ 1, offset stays 1
  dp[1] = dp[1 - 1] + 1 = dp[0] + 1 = 0 + 1 = 1
dp = [0, 1, 0, 0, 0, 0]

i = 2:
  offset * 2 = 2 == 2 ✓, offset = 2
  dp[2] = dp[2 - 2] + 1 = dp[0] + 1 = 0 + 1 = 1
dp = [0, 1, 1, 0, 0, 0]

i = 3:
  offset * 2 = 4 ≠ 3, offset stays 2
  dp[3] = dp[3 - 2] + 1 = dp[1] + 1 = 1 + 1 = 2
dp = [0, 1, 1, 2, 0, 0]

i = 4:
  offset * 2 = 4 == 4 ✓, offset = 4
  dp[4] = dp[4 - 4] + 1 = dp[0] + 1 = 0 + 1 = 1
dp = [0, 1, 1, 2, 1, 0]

i = 5:
  offset * 2 = 8 ≠ 5, offset stays 4
  dp[5] = dp[5 - 4] + 1 = dp[1] + 1 = 1 + 1 = 2
dp = [0, 1, 1, 2, 1, 2]

Final: [0, 1, 1, 2, 1, 2] ✓
```

### 7.5 Approach 3: DP Using i & (i-1)

**Why would I think of this?**
"I know `i & (i-1)` removes one 1 bit. So `dp[i]` must equal `dp[i & (i-1)] + 1`!"

```python
def countBits_and(n):
    dp = [0] * (n + 1)
    
    for i in range(1, n + 1):
        dp[i] = dp[i & (i - 1)] + 1
    
    return dp
```

**Why does this work?**
- `i & (i-1)` removes the rightmost 1 bit
- `i & (i-1)` is always less than `i`, so we've already computed it
- We just add 1 for the bit we "removed"

**Walkthrough**:
```
i = 5 (binary 101):
  5 & 4 = 101 & 100 = 100 = 4
  dp[5] = dp[4] + 1 = 1 + 1 = 2 ✓

i = 7 (binary 111):
  7 & 6 = 111 & 110 = 110 = 6
  dp[7] = dp[6] + 1 = 2 + 1 = 3 ✓
```

This is the cleanest solution!

---

## 8. Range Sum Query - Immutable

📌 **LeetCode Link**: [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

This problem introduces **Prefix Sums**, a crucial DP technique!

### 8.1 Problem Statement

Given an integer array `nums`, handle multiple queries of the following type:

Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive**.

Implement the `NumArray` class:
- `NumArray(int[] nums)` - Initializes the object with array `nums`
- `int sumRange(int left, int right)` - Returns the sum of elements between `left` and `right`

### 8.2 Examples

```
Input:
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]

Output: [null, 1, -1, -3]

Explanation:
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

### 8.3 Why This is Important

**Real-world use**: Database queries, financial data analysis, game score tracking

**Interview frequency**: Very common! Prefix sums appear in many problems.

### 8.4 Approach 1: Brute Force

**Why would I think of this?**
"For each query, just sum the elements from left to right."

```python
class NumArray_Brute:
    def __init__(self, nums):
        self.nums = nums
    
    def sumRange(self, left, right):
        return sum(self.nums[left:right+1])
```

**Why is this not optimal?**
- **Init Time**: O(1)
- **Query Time**: O(n) for each query
- If we have Q queries, total time is O(Q × n)

For millions of queries, this is too slow!

### 8.5 Approach 2: Prefix Sum (DP) ⭐

**Why would I think of this?**
"What if I precompute cumulative sums? Then I can answer any query in O(1)!"

**What is a Prefix Sum?**

A prefix sum array stores the cumulative sum up to each index:

```
Original:    [-2,  0,  3, -5,  2, -1]
Index:         0   1   2   3   4   5

Prefix sum:  [-2, -2,  1, -4, -2, -3]
              ↑   ↑   ↑   ↑   ↑   ↑
             -2  -2+0 -2+0+3 ...

prefix[i] = sum of nums[0] through nums[i]
```

**The Magic Formula**:

```
sum(left, right) = prefix[right] - prefix[left-1]
```

**Why does this work?**
```
prefix[right] = nums[0] + nums[1] + ... + nums[left-1] + nums[left] + ... + nums[right]
prefix[left-1] = nums[0] + nums[1] + ... + nums[left-1]

prefix[right] - prefix[left-1] = nums[left] + ... + nums[right] ✓
```

**Edge case**: When `left = 0`, there's no `prefix[left-1]`. Solution: Add a leading 0!

```python
class NumArray:
    def __init__(self, nums):
        # prefix[i] = sum of nums[0...i-1]
        # prefix[0] = 0 (empty sum)
        # prefix[1] = nums[0]
        # prefix[2] = nums[0] + nums[1]
        # etc.
        
        self.prefix = [0] * (len(nums) + 1)
        
        for i in range(len(nums)):
            self.prefix[i + 1] = self.prefix[i] + nums[i]
    
    def sumRange(self, left, right):
        # sum(left, right) = prefix[right+1] - prefix[left]
        return self.prefix[right + 1] - self.prefix[left]
```

**Analysis**:
- **Init Time**: O(n) - Build prefix array once
- **Init Space**: O(n) - Store prefix array
- **Query Time**: O(1) ✓

### 8.6 Detailed Walkthrough

```
nums = [-2, 0, 3, -5, 2, -1]

Building prefix array:
prefix = [0, 0, 0, 0, 0, 0, 0]  (size = len(nums) + 1)

i = 0: prefix[1] = prefix[0] + nums[0] = 0 + (-2) = -2
i = 1: prefix[2] = prefix[1] + nums[1] = -2 + 0 = -2
i = 2: prefix[3] = prefix[2] + nums[2] = -2 + 3 = 1
i = 3: prefix[4] = prefix[3] + nums[3] = 1 + (-5) = -4
i = 4: prefix[5] = prefix[4] + nums[4] = -4 + 2 = -2
i = 5: prefix[6] = prefix[5] + nums[5] = -2 + (-1) = -3

prefix = [0, -2, -2, 1, -4, -2, -3]

Query: sumRange(0, 2)
  = prefix[2+1] - prefix[0]
  = prefix[3] - prefix[0]
  = 1 - 0
  = 1 ✓

Query: sumRange(2, 5)
  = prefix[5+1] - prefix[2]
  = prefix[6] - prefix[2]
  = -3 - (-2)
  = -1 ✓

Query: sumRange(0, 5)
  = prefix[5+1] - prefix[0]
  = prefix[6] - prefix[0]
  = -3 - 0
  = -3 ✓
```

### 8.7 Visual Understanding

```
Index:    0    1    2    3    4    5
nums:   [-2,   0,   3,  -5,   2,  -1]

prefix: [0,  -2,  -2,   1,  -4,  -2,  -3]
         ↑
         padding (sum of nothing = 0)

sumRange(2, 4):
  We want: nums[2] + nums[3] + nums[4] = 3 + (-5) + 2 = 0

  prefix[5] = sum of nums[0..4] = -2 + 0 + 3 - 5 + 2 = -2
  prefix[2] = sum of nums[0..1] = -2 + 0 = -2
  
  Difference = -2 - (-2) = 0 ✓
```

---

## 9. More Essential DP Problems

Let's cover more important DP problems for interview preparation!

---

### 9.1 House Robber

📌 **LeetCode Link**: [198. House Robber](https://leetcode.com/problems/house-robber/)

#### Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. The only constraint stopping you from robbing each of them is that **adjacent houses have security systems connected** - robbing two adjacent houses will trigger an alarm.

Given an integer array `nums` representing money at each house, return the **maximum amount** you can rob without alerting the police.

#### Examples

```
Example 1:
Input: nums = [1, 2, 3, 1]
Output: 4
Explanation: Rob house 0 (money = 1) and house 2 (money = 3)
             Total = 1 + 3 = 4

Example 2:
Input: nums = [2, 7, 9, 3, 1]
Output: 12
Explanation: Rob house 0 (2), house 2 (9), house 4 (1)
             Total = 2 + 9 + 1 = 12
```

#### Approach 1: Brute Force (Try All Combinations)

**Why would I think of this?**
"Let me try all possible combinations of non-adjacent houses."

```python
def rob_brute(nums):
    def helper(index):
        if index >= len(nums):
            return 0
        
        # Choice 1: Rob this house, skip next
        rob = nums[index] + helper(index + 2)
        # Choice 2: Skip this house, check next
        skip = helper(index + 1)
        
        return max(rob, skip)
    
    return helper(0)
```

**Why is this terrible?**
- **Time**: O(2^n) - Each house has 2 choices
- Same subproblems computed many times!

#### Approach 2: DP Solution

**Why would I think of this?**
"At each house, I decide: rob it (plus best from 2 houses ago) or skip it (take best from previous house)."

**The Recurrence**:
```
dp[i] = max(dp[i-1], dp[i-2] + nums[i])
        ↑           ↑
      Skip       Rob this house
```

```python
def rob(nums):
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    
    n = len(nums)
    dp = [0] * n
    
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    
    for i in range(2, n):
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    
    return dp[n-1]
```

**Walkthrough for [2, 7, 9, 3, 1]**:

```
dp[0] = 2 (only option is house 0)
dp[1] = max(2, 7) = 7 (best of house 0 or 1)

i = 2:
  dp[2] = max(dp[1], dp[0] + nums[2])
        = max(7, 2 + 9)
        = max(7, 11)
        = 11

i = 3:
  dp[3] = max(dp[2], dp[1] + nums[3])
        = max(11, 7 + 3)
        = max(11, 10)
        = 11

i = 4:
  dp[4] = max(dp[3], dp[2] + nums[4])
        = max(11, 11 + 1)
        = max(11, 12)
        = 12

Final: 12 ✓
```

#### Space Optimized

```python
def rob_optimized(nums):
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    
    prev2 = nums[0]
    prev1 = max(nums[0], nums[1])
    
    for i in range(2, len(nums)):
        current = max(prev1, prev2 + nums[i])
        prev2 = prev1
        prev1 = current
    
    return prev1
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(1)

---

### 9.2 Unique Paths

📌 **LeetCode Link**: [62. Unique Paths](https://leetcode.com/problems/unique-paths/)

#### Problem Statement

There is a robot on an `m x n` grid. The robot starts at the top-left corner (0, 0) and wants to reach the bottom-right corner (m-1, n-1).

The robot can only move **down** or **right** at any point. Return the number of unique paths.

#### Examples

```
Example 1:
Input: m = 3, n = 7
Output: 28

Example 2:
Input: m = 3, n = 2
Output: 3
Explanation: From top-left to bottom-right:
1. Right → Down → Down
2. Down → Right → Down
3. Down → Down → Right
```

#### Visualizing the Problem

```
m=3, n=3 grid:

S → → 
↓   ↓
↓ → E

S = Start, E = End
```

#### Approach 1: Brute Force Recursion

**Why would I think of this?**
"From any cell, I can go right or down. Let me count all paths recursively."

```python
def uniquePaths_brute(m, n):
    def helper(row, col):
        # Reached destination
        if row == m - 1 and col == n - 1:
            return 1
        # Out of bounds
        if row >= m or col >= n:
            return 0
        
        # Go right + go down
        return helper(row, col + 1) + helper(row + 1, col)
    
    return helper(0, 0)
```

**Why is this slow?**
- **Time**: O(2^(m+n)) - Exponential!
- Many cells visited multiple times

#### Approach 2: DP Solution

**Why would I think of this?**
"The number of paths to cell (i, j) = paths from above + paths from left"

```
dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

```python
def uniquePaths(m, n):
    # dp[i][j] = number of paths to reach cell (i, j)
    dp = [[0] * n for _ in range(m)]
    
    # First row: only one way (go right)
    for j in range(n):
        dp[0][j] = 1
    
    # First column: only one way (go down)
    for i in range(m):
        dp[i][0] = 1
    
    # Fill rest of the grid
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    
    return dp[m-1][n-1]
```

**Walkthrough for m=3, n=3**:

```
Initial (first row and column filled with 1s):
dp = [
  [1, 1, 1],
  [1, 0, 0],
  [1, 0, 0]
]

i=1, j=1: dp[1][1] = dp[0][1] + dp[1][0] = 1 + 1 = 2
i=1, j=2: dp[1][2] = dp[0][2] + dp[1][1] = 1 + 2 = 3
i=2, j=1: dp[2][1] = dp[1][1] + dp[2][0] = 2 + 1 = 3
i=2, j=2: dp[2][2] = dp[1][2] + dp[2][1] = 3 + 3 = 6

Final:
dp = [
  [1, 1, 1],
  [1, 2, 3],
  [1, 3, 6]
]

Answer: 6 ✓
```

**Analysis**:
- **Time**: O(m × n)
- **Space**: O(m × n), can be optimized to O(n)

---

### 9.3 Longest Common Subsequence

📌 **LeetCode Link**: [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

This is a CLASSIC DP problem that appears frequently in interviews!

#### Problem Statement

Given two strings `text1` and `text2`, return the length of their **longest common subsequence**. If there is no common subsequence, return 0.

**What is a subsequence?**
A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

#### Examples

```
Example 1:
Input: text1 = "abcde", text2 = "ace"
Output: 3
Explanation: LCS is "ace", length 3

Example 2:
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: LCS is "abc", length 3

Example 3:
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: No common subsequence
```

#### Understanding Subsequences

```
"abcde" has subsequences: "a", "ab", "ace", "abcde", "ae", "bd", etc.
NOT subsequences: "aec" (wrong order), "f" (not in string)
```

#### Approach 1: Brute Force

**Why would I think of this?**
"Generate all subsequences of both strings, find common ones, return longest."

**Why is this terrible?**
- A string of length n has 2^n subsequences!
- **Time**: O(2^n × 2^m × min(n,m)) - Completely impractical

#### Approach 2: DP Solution

**Why would I think of this?**
"Compare characters one by one. If they match, we found part of LCS. If not, try skipping one character from either string."

**The Recurrence**:
```
If text1[i] == text2[j]:
    dp[i][j] = 1 + dp[i-1][j-1]  (characters match, extend LCS)
Else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])  (skip one char from either string)
```

```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1), len(text2)
    
    # dp[i][j] = LCS of text1[0..i-1] and text2[0..j-1]
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]
```

**Walkthrough for "abcde" and "ace"**:

```
      ""  a  c  e
  ""   0  0  0  0
  a    0  1  1  1
  b    0  1  1  1
  c    0  1  2  2
  d    0  1  2  2
  e    0  1  2  3

Answer: dp[5][3] = 3 ✓
```

**Analysis**:
- **Time**: O(m × n)
- **Space**: O(m × n)

---

### 9.4 Word Break

📌 **LeetCode Link**: [139. Word Break](https://leetcode.com/problems/word-break/)

#### Problem Statement

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note**: The same word in the dictionary may be reused multiple times.
