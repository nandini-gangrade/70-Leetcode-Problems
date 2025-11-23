# Bit Manipulation - Complete Beginner's Deep Dive

Bit manipulation might seem like a low-level, obscure topic, but it's actually tested in coding interviews at top tech companies. The video creator specifically mentions that **this was one of the questions they faced in their software engineering interview**!

---

## Table of Contents
1. [What Are Bits? (The Foundation)](#1-what-are-bits-the-foundation)
2. [Binary Number System](#2-binary-number-system)
3. [Bitwise Operators](#3-bitwise-operators)
4. [The XOR Operator (The Star of This Section)](#4-the-xor-operator-the-star-of-this-section)
5. [Single Number Problem](#5-single-number-problem)
6. [More Bit Manipulation Problems](#6-more-bit-manipulation-problems)
7. [Bit Manipulation Tricks and Patterns](#7-bit-manipulation-tricks-and-patterns)
8. [Practice Problems](#8-practice-problems)

---

## 1. What Are Bits? (The Foundation)

### 1.1 The Simplest Unit of Data

A **bit** (short for **binary digit**) is the most basic unit of information in computing. It can only have one of two values:
- **0** (off, false, no)
- **1** (on, true, yes)

Think of it like a light switch - it's either ON or OFF. There's no in-between.

```
A single bit:
┌───┐     ┌───┐
│ 0 │  or │ 1 │
└───┘     └───┘
 OFF       ON
```

### 1.2 Why Computers Use Bits

Computers are electronic devices that work with electrical signals. It's easiest to distinguish between two states:
- **High voltage** → 1
- **Low voltage** → 0

Everything in your computer - numbers, text, images, videos, programs - is ultimately represented as sequences of 0s and 1s.

### 1.3 Grouping Bits

Single bits aren't very useful alone, so we group them:

| Unit | # of Bits | Possible Values |
|------|-----------|-----------------|
| 1 bit | 1 | 2 (0 or 1) |
| 1 nibble | 4 | 16 (0-15) |
| 1 byte | 8 | 256 (0-255) |
| 1 word | 16, 32, or 64 | Varies |

```
A byte (8 bits):
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 1 │ 0 │ 1 │ 0 │ 0 │ 1 │ 1 │
└───┴───┴───┴───┴───┴───┴───┴───┘
```

---

## 2. Binary Number System

### 2.1 Decimal vs Binary

We humans use the **decimal** (base-10) system with digits 0-9.

Computers use the **binary** (base-2) system with only digits 0 and 1.

### 2.2 How Decimal Works (What You Already Know)

In decimal, each position represents a power of 10:

```
Number: 352

Position:    2      1      0
Power of 10: 10²    10¹    10⁰
Value:       100    10     1

352 = 3×100 + 5×10 + 2×1
    = 300   + 50   + 2
    = 352
```

### 2.3 How Binary Works (Same Concept, Different Base)

In binary, each position represents a power of 2:

```
Binary: 1011

Position:    3      2      1      0
Power of 2:  2³     2²     2¹     2⁰
Value:       8      4      2      1

1011 = 1×8 + 0×4 + 1×2 + 1×1
     = 8   + 0   + 2   + 1
     = 11 (in decimal)
```

### 2.4 Binary to Decimal Conversion Table

| Binary | Decimal | How to Calculate |
|--------|---------|------------------|
| 0 | 0 | 0 |
| 1 | 1 | 1 |
| 10 | 2 | 1×2 + 0×1 |
| 11 | 3 | 1×2 + 1×1 |
| 100 | 4 | 1×4 + 0×2 + 0×1 |
| 101 | 5 | 1×4 + 0×2 + 1×1 |
| 110 | 6 | 1×4 + 1×2 + 0×1 |
| 111 | 7 | 1×4 + 1×2 + 1×1 |
| 1000 | 8 | 1×8 |
| 1001 | 9 | 1×8 + 1×1 |
| 1010 | 10 | 1×8 + 1×2 |

### 2.5 Python Binary Functions

```python
# Decimal to binary
bin(5)      # '0b101' (0b prefix means binary)
bin(10)     # '0b1010'
bin(255)    # '0b11111111'

# Binary to decimal
int('101', 2)    # 5 (the 2 means base-2)
int('1010', 2)   # 10
int('11111111', 2)  # 255

# You can also write binary literals directly
x = 0b101   # x = 5
y = 0b1010  # y = 10
```

### 2.6 Visualizing How Numbers Look in Binary

```
Decimal 1:  00000001
Decimal 2:  00000010   (notice the 1 shifted left)
Decimal 3:  00000011
Decimal 4:  00000100   (1 shifted left again)
Decimal 5:  00000101
Decimal 6:  00000110
Decimal 7:  00000111
Decimal 8:  00001000   (1 shifted left again)
```

**Pattern**: Powers of 2 (1, 2, 4, 8, 16...) always have exactly one '1' bit!

---

## 3. Bitwise Operators

### 3.1 What is an Operator?

An **operator** is a symbol that tells the computer to perform a specific operation.

You already know arithmetic operators:
- `+` (addition): `5 + 3 = 8`
- `-` (subtraction): `5 - 3 = 2`
- `*` (multiplication): `5 * 3 = 15`
- `/` (division): `5 / 3 = 1.666...`

**Bitwise operators** work on the **individual bits** of numbers.

### 3.2 The Six Bitwise Operators

| Operator | Symbol | Name | Description |
|----------|--------|------|-------------|
| AND | `&` | Bitwise AND | 1 if BOTH bits are 1 |
| OR | `\|` | Bitwise OR | 1 if EITHER bit is 1 |
| XOR | `^` | Bitwise XOR | 1 if bits are DIFFERENT |
| NOT | `~` | Bitwise NOT | Flips all bits |
| Left Shift | `<<` | Left Shift | Shifts bits left |
| Right Shift | `>>` | Right Shift | Shifts bits right |

Let's explore each one:

### 3.3 AND Operator (`&`)

Returns 1 only if **BOTH** bits are 1.

**Truth Table**:
```
A | B | A & B
--|---|------
0 | 0 |   0
0 | 1 |   0
1 | 0 |   0
1 | 1 |   1     ← Only case where result is 1
```

**Example**:
```
    5 = 0101
    3 = 0011
    --------
5 & 3 = 0001 = 1

Step by step:
Position 3: 0 & 0 = 0
Position 2: 1 & 0 = 0
Position 1: 0 & 1 = 0
Position 0: 1 & 1 = 1
```

```python
print(5 & 3)   # Output: 1
print(7 & 3)   # 0111 & 0011 = 0011 = 3
print(12 & 10) # 1100 & 1010 = 1000 = 8
```

**Use Cases**:
- Check if a number is even/odd: `n & 1` (0 if even, 1 if odd)
- Extract specific bits (masking)
- Clear certain bits

```python
# Check even/odd
def is_even(n):
    return (n & 1) == 0

print(is_even(4))  # True  (4 = 100, 100 & 001 = 000)
print(is_even(7))  # False (7 = 111, 111 & 001 = 001)
```

### 3.4 OR Operator (`|`)

Returns 1 if **EITHER** (or both) bits are 1.

**Truth Table**:
```
A | B | A | B
--|---|------
0 | 0 |   0     ← Only case where result is 0
0 | 1 |   1
1 | 0 |   1
1 | 1 |   1
```

**Example**:
```
    5 = 0101
    3 = 0011
    --------
5 | 3 = 0111 = 7

Step by step:
Position 3: 0 | 0 = 0
Position 2: 1 | 0 = 1
Position 1: 0 | 1 = 1
Position 0: 1 | 1 = 1
```

```python
print(5 | 3)   # Output: 7
print(4 | 2)   # 0100 | 0010 = 0110 = 6
```

**Use Cases**:
- Set specific bits to 1
- Combine flags/permissions

### 3.5 XOR Operator (`^`) - THE MOST IMPORTANT FOR INTERVIEWS!

Returns 1 if bits are **DIFFERENT**.

**Truth Table**:
```
A | B | A ^ B
--|---|------
0 | 0 |   0     ← Same, so 0
0 | 1 |   1     ← Different, so 1
1 | 0 |   1     ← Different, so 1
1 | 1 |   0     ← Same, so 0
```

**Example**:
```
    5 = 0101
    3 = 0011
    --------
5 ^ 3 = 0110 = 6

Step by step:
Position 3: 0 ^ 0 = 0 (same)
Position 2: 1 ^ 0 = 1 (different)
Position 1: 0 ^ 1 = 1 (different)
Position 0: 1 ^ 1 = 0 (same)
```

```python
print(5 ^ 3)   # Output: 6
print(7 ^ 7)   # Output: 0 (same numbers cancel!)
print(7 ^ 0)   # Output: 7 (XOR with 0 gives same number)
```

### 3.6 NOT Operator (`~`)

Flips all bits (0 becomes 1, 1 becomes 0).

**Note**: In Python, this also considers the sign, which can be confusing.

```python
print(~5)   # Output: -6
# Because ~x = -(x+1) in Python due to how negative numbers work
```

For most interview problems, you won't need NOT.

### 3.7 Left Shift (`<<`)

Shifts all bits to the LEFT, filling with 0s on the right.

**Each left shift DOUBLES the number!**

```
5 = 00000101
5 << 1 = 00001010 = 10  (5 × 2)
5 << 2 = 00010100 = 20  (5 × 4)
5 << 3 = 00101000 = 40  (5 × 8)
```

```python
print(5 << 1)   # 10 (5 × 2)
print(5 << 2)   # 20 (5 × 4)
print(1 << 4)   # 16 (1 × 16 = 2⁴)
```

**Formula**: `x << n` equals `x × 2ⁿ`

### 3.8 Right Shift (`>>`)

Shifts all bits to the RIGHT.

**Each right shift HALVES the number (integer division)!**

```
20 = 00010100
20 >> 1 = 00001010 = 10  (20 ÷ 2)
20 >> 2 = 00000101 = 5   (20 ÷ 4)
```

```python
print(20 >> 1)  # 10 (20 ÷ 2)
print(20 >> 2)  # 5  (20 ÷ 4)
print(7 >> 1)   # 3  (7 ÷ 2 = 3.5, but integer division gives 3)
```

**Formula**: `x >> n` equals `x ÷ 2ⁿ` (integer division)

---

## 4. The XOR Operator (The Star of This Section)

XOR is the most important bitwise operator for coding interviews. Let's understand it deeply.

### 4.1 XOR Properties (MEMORIZE THESE!)

| Property | Formula | Example |
|----------|---------|---------|
| **Self-inverse** | `a ^ a = 0` | `5 ^ 5 = 0` |
| **Identity** | `a ^ 0 = a` | `5 ^ 0 = 5` |
| **Commutative** | `a ^ b = b ^ a` | `3 ^ 5 = 5 ^ 3` |
| **Associative** | `(a ^ b) ^ c = a ^ (b ^ c)` | Order doesn't matter |

### 4.2 The Magic Property: Self-Cancellation

This is the KEY insight for interview problems:

**When you XOR a number with itself, it becomes 0!**

```python
# Any number XOR itself = 0
print(5 ^ 5)     # 0
print(100 ^ 100) # 0
print(999 ^ 999) # 0

# Proof with bits:
#   5 = 0101
#   5 = 0101
#   --------
# 5^5 = 0000 = 0  (all positions are "same", so all 0s)
```

### 4.3 Chaining XOR Operations

When you XOR multiple numbers together, **pairs cancel out**:

```python
# Example: 1 ^ 2 ^ 1
# The two 1s cancel each other!
print(1 ^ 2 ^ 1)  # Output: 2

# Example: 4 ^ 1 ^ 2 ^ 1 ^ 2
# 1 ^ 1 = 0, 2 ^ 2 = 0, leaving just 4
print(4 ^ 1 ^ 2 ^ 1 ^ 2)  # Output: 4

# Example: 3 ^ 5 ^ 3 ^ 5 ^ 7
# 3 ^ 3 = 0, 5 ^ 5 = 0, leaving just 7
print(3 ^ 5 ^ 3 ^ 5 ^ 7)  # Output: 7
```

### 4.4 Why Order Doesn't Matter

XOR is both **commutative** (order doesn't matter) and **associative** (grouping doesn't matter):

```python
# All of these give the same result:
print(1 ^ 2 ^ 3)        # 0
print(3 ^ 1 ^ 2)        # 0
print(2 ^ 3 ^ 1)        # 0
print((1 ^ 2) ^ 3)      # 0
print(1 ^ (2 ^ 3))      # 0
```

This property is CRUCIAL for the Single Number problem!

---

## 5. Single Number Problem

📌 **LeetCode Link**: [136. Single Number](https://leetcode.com/problems/single-number/)

### 5.1 Problem Statement

Given a **non-empty** array of integers `nums`, every element appears **twice** except for one. Find that single one.

**Constraints**:
- You must implement a solution with **O(n)** time complexity
- You must use only **O(1)** extra space (constant space)

### 5.2 Examples

```
Example 1:
Input: nums = [2, 2, 1]
Output: 1
Explanation: 1 appears once, 2 appears twice

Example 2:
Input: nums = [4, 1, 2, 1, 2]
Output: 4
Explanation: 4 appears once, 1 and 2 appear twice each

Example 3:
Input: nums = [1]
Output: 1
Explanation: Only one element, it's the single one
```

### 5.3 Approaches That DON'T Meet the Requirements

Before we see the optimal solution, let's understand why other approaches fail the constraints:

#### Approach 1: Brute Force
```python
def singleNumber_brute(nums):
    for i in range(len(nums)):
        count = 0
        for j in range(len(nums)):
            if nums[i] == nums[j]:
                count += 1
        if count == 1:
            return nums[i]
```
- **Time**: O(n²) ❌ (doesn't meet O(n) requirement)
- **Space**: O(1) ✓

#### Approach 2: Sorting
```python
def singleNumber_sort(nums):
    nums.sort()
    i = 0
    while i < len(nums) - 1:
        if nums[i] == nums[i + 1]:
            i += 2  # Skip both
        else:
            return nums[i]
    return nums[-1]  # Last element is single
```
- **Time**: O(n log n) ❌ (doesn't meet O(n) requirement)
- **Space**: O(1) or O(n) depending on sort

#### Approach 3: Hash Map (Dictionary)
```python
def singleNumber_hash(nums):
    count = {}
    for num in nums:
        count[num] = count.get(num, 0) + 1
    
    for num, c in count.items():
        if c == 1:
            return num
```
- **Time**: O(n) ✓
- **Space**: O(n) ❌ (doesn't meet O(1) requirement)

#### Approach 4: Set Math
```python
def singleNumber_set(nums):
    # 2 * (sum of unique numbers) - (sum of all numbers) = single number
    return 2 * sum(set(nums)) - sum(nums)
```
- **Time**: O(n) ✓
- **Space**: O(n) ❌ (set takes O(n) space)

### 5.4 The XOR Solution (Optimal) ⭐

```python
def singleNumber(nums):
    result = 0
    for num in nums:
        result ^= num  # XOR each number with result
    return result
```

**That's it! Just 4 lines!**

- **Time**: O(n) ✓
- **Space**: O(1) ✓ (only one variable!)

### 5.5 Why XOR Works - Detailed Explanation

Let's trace through `nums = [4, 1, 2, 1, 2]`:

```
Initial: result = 0

Step 1: result = 0 ^ 4 = 4
  Binary: 0000 ^ 0100 = 0100

Step 2: result = 4 ^ 1 = 5
  Binary: 0100 ^ 0001 = 0101

Step 3: result = 5 ^ 2 = 7
  Binary: 0101 ^ 0010 = 0111

Step 4: result = 7 ^ 1 = 6
  Binary: 0111 ^ 0001 = 0110

Step 5: result = 6 ^ 2 = 4
  Binary: 0110 ^ 0010 = 0100

Final: result = 4 ✓
```

**Why does this work?**

Because XOR has these properties:
1. `a ^ a = 0` (pairs cancel out)
2. `a ^ 0 = a` (XOR with 0 gives same number)
3. Order doesn't matter (commutative and associative)

So we can mentally rearrange:
```
4 ^ 1 ^ 2 ^ 1 ^ 2
= 4 ^ (1 ^ 1) ^ (2 ^ 2)   ← Group pairs
= 4 ^ 0 ^ 0               ← Pairs become 0
= 4                        ← XOR with 0 gives same number
```

### 5.6 Visual Bit-by-Bit Explanation

Let's look at it column by column:

```
Array: [4, 1, 2, 1, 2]

Binary representations:
4 = 100
1 = 001
2 = 010
1 = 001
2 = 010

XOR column by column (rightmost first):

Column 0 (1's place):
0 ^ 1 ^ 0 ^ 1 ^ 0 = 0
(The 1s cancel: 1 ^ 1 = 0)

Column 1 (2's place):
0 ^ 0 ^ 1 ^ 0 ^ 1 = 0
(The 1s cancel: 1 ^ 1 = 0)

Column 2 (4's place):
1 ^ 0 ^ 0 ^ 0 ^ 0 = 1
(Only one 1, so it stays)

Result: 100 = 4 ✓
```

### 5.7 Edge Cases

```python
# Single element
nums = [1]
# result = 0 ^ 1 = 1 ✓

# Negative numbers work too!
nums = [-1, -1, -2]
# result = 0 ^ -1 ^ -1 ^ -2 = -2 ✓

# Large numbers
nums = [999999, 1, 999999]
# result = 1 ✓
```

---

## 6. More Bit Manipulation Problems

Now let's explore more problems that use bit manipulation techniques.

---

### 6.1 Single Number II

📌 **LeetCode Link**: [137. Single Number II](https://leetcode.com/problems/single-number-ii/)

#### Problem Statement

Given an integer array `nums` where every element appears **three times** except for one, which appears **exactly once**. Find the single element.

**You must implement a solution with O(n) time and O(1) space.**

#### Examples

```
Example 1:
Input: nums = [2, 2, 3, 2]
Output: 3

Example 2:
Input: nums = [0, 1, 0, 1, 0, 1, 99]
Output: 99
```

#### Why XOR Alone Doesn't Work

XOR only cancels pairs (`a ^ a = 0`). With triples, we need a different approach.

#### Solution: Count Bits

**Idea**: For each bit position, count how many numbers have a 1 there. If count % 3 != 0, that bit belongs to the single number.

```python
def singleNumber(nums):
    result = 0
    
    # Check each bit position (32 bits for integers)
    for i in range(32):
        bit_sum = 0
        
        # Count how many numbers have a 1 at position i
        for num in nums:
            # Check if bit i is set
            if (num >> i) & 1:
                bit_sum += 1
        
        # If count % 3 != 0, this bit belongs to single number
        if bit_sum % 3 != 0:
            result |= (1 << i)
    
    # Handle negative numbers in Python
    if result >= 2**31:
        result -= 2**32
    
    return result
```

**Analysis**:
- **Time**: O(32n) = O(n)
- **Space**: O(1)

#### Walkthrough for [2, 2, 3, 2]

```
Binary representations:
2 = 010
2 = 010
3 = 011
2 = 010

Bit position 0 (rightmost):
  Count of 1s: 0 + 0 + 1 + 0 = 1
  1 % 3 = 1 ≠ 0 → This bit is in result

Bit position 1:
  Count of 1s: 1 + 1 + 1 + 1 = 4
  4 % 3 = 1 ≠ 0 → This bit is in result

Bit position 2:
  Count of 1s: 0 + 0 + 0 + 0 = 0
  0 % 3 = 0 → This bit is NOT in result

Result: 011 = 3 ✓
```

---

### 6.2 Single Number III

📌 **LeetCode Link**: [260. Single Number III](https://leetcode.com/problems/single-number-iii/)

#### Problem Statement

Given an integer array `nums`, where exactly **two elements appear only once** and all other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in any order.

**You must implement a solution with O(n) time and O(1) space.**

#### Examples

```
Example 1:
Input: nums = [1, 2, 1, 3, 2, 5]
Output: [3, 5]
Explanation: [5, 3] is also valid

Example 2:
Input: nums = [-1, 0]
Output: [-1, 0]
```

#### The Approach

**Step 1**: XOR all numbers. Result = a ^ b (where a and b are the two single numbers)

**Step 2**: Find any bit that's different between a and b (this bit is 1 in XOR result)

**Step 3**: Divide numbers into two groups based on that bit and XOR each group

```python
def singleNumber(nums):
    # Step 1: XOR all numbers to get a ^ b
    xor = 0
    for num in nums:
        xor ^= num
    
    # Step 2: Find rightmost set bit (where a and b differ)
    # This is a common bit trick: x & (-x) gives rightmost 1 bit
    diff_bit = xor & (-xor)
    
    # Step 3: Divide into two groups and XOR each
    a = 0
    b = 0
    for num in nums:
        if num & diff_bit:
            a ^= num   # Group where this bit is 1
        else:
            b ^= num   # Group where this bit is 0
    
    return [a, b]
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(1)

#### Why This Works

```
nums = [1, 2, 1, 3, 2, 5]

Step 1: XOR all
1 ^ 2 ^ 1 ^ 3 ^ 2 ^ 5
= (1^1) ^ (2^2) ^ 3 ^ 5
= 0 ^ 0 ^ 3 ^ 5
= 3 ^ 5
= 011 ^ 101
= 110 = 6

Step 2: Find rightmost 1 bit
6 = 110
-6 = ...11111010 (two's complement)
6 & (-6) = 010 = 2

This means 3 and 5 differ in bit position 1 (the 2's place):
3 = 011 (bit 1 is 1)
5 = 101 (bit 1 is 0)

Step 3: Divide and XOR
Group where bit 1 is 1: 1, 2, 1, 3, 2 (wait, let me recalculate)

Actually, let's check each number:
1 = 001, bit 1 is 0 → Group B
2 = 010, bit 1 is 1 → Group A
1 = 001, bit 1 is 0 → Group B
3 = 011, bit 1 is 1 → Group A
2 = 010, bit 1 is 1 → Group A
5 = 101, bit 1 is 0 → Group B

Group A: 2, 3, 2 → XOR = 3
Group B: 1, 1, 5 → XOR = 5

Result: [3, 5] ✓
```

---

### 6.3 Number of 1 Bits (Hamming Weight)

📌 **LeetCode Link**: [191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

#### Problem Statement

Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

#### Examples

```
Example 1:
Input: n = 11 (binary: 1011)
Output: 3
Explanation: 1011 has three '1' bits

Example 2:
Input: n = 128 (binary: 10000000)
Output: 1

Example 3:
Input: n = 2147483645 (binary: 1111111111111111111111111111101)
Output: 30
```

#### Solution 1: Check Each Bit

```python
def hammingWeight(n):
    count = 0
    while n:
        count += n & 1  # Add 1 if last bit is 1
        n >>= 1         # Shift right (remove last bit)
    return count
```

#### Solution 2: Brian Kernighan's Algorithm (Optimal)

**Trick**: `n & (n - 1)` removes the rightmost 1 bit!

```
n = 12     = 1100
n - 1 = 11 = 1011
n & (n-1)  = 1000 = 8  (rightmost 1 is gone!)

n = 8      = 1000
n - 1 = 7  = 0111
n & (n-1)  = 0000 = 0  (rightmost 1 is gone!)
```

```python
def hammingWeight(n):
    count = 0
    while n:
        n &= (n - 1)  # Remove rightmost 1 bit
        count += 1
    return count
```

**Analysis**:
- **Time**: O(k) where k is the number of 1 bits
- **Space**: O(1)

---

### 6.4 Counting Bits

📌 **LeetCode Link**: [338. Counting Bits](https://leetcode.com/problems/counting-bits/)

This problem was covered in detail in the transcript!

#### Problem Statement

Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i` (0 <= i <= n), `ans[i]` is the **number of 1's** in the binary representation of `i`.

#### Examples

```
Example 1:
Input: n = 2
Output: [0, 1, 1]
Explanation:
0 = 0 (zero 1's)
1 = 1 (one 1)
2 = 10 (one 1)

Example 2:
Input: n = 5
Output: [0, 1, 1, 2, 1, 2]
Explanation:
0 = 0    (zero 1's)
1 = 1    (one 1)
2 = 10   (one 1)
3 = 11   (two 1's)
4 = 100  (one 1)
5 = 101  (two 1's)
```

#### Approach 1: Brute Force - Count Each Number Individually

**Why would I think of this?**
The most natural first thought is: "I need to count 1 bits for each number from 0 to n. Let me just do that for each number separately."

```python
def countBits_brute(n):
    result = []
    for i in range(n + 1):
        # Count 1 bits in i
        count = 0
        num = i
        while num:
            count += num & 1
            num >>= 1
        result.append(count)
    return result
```

**Why does this work?**
It correctly counts 1 bits for each number using the bit-checking technique we learned.

**Why is this not optimal?**
- **Time**: O(n log n) - For each of n numbers, we check up to log(n) bits
- **Space**: O(1) extra (excluding output)

The problem hints at finding an O(n) solution, so we need to think smarter.

#### Approach 2: Using Brian Kernighan's Algorithm

**Why would I think of this?**
"I know `n & (n-1)` removes the rightmost 1 bit. Maybe I can use that to count faster?"

```python
def countBits_kernighan(n):
    result = []
    for i in range(n + 1):
        count = 0
        num = i
        while num:
            num &= (num - 1)  # Remove rightmost 1
            count += 1
        result.append(count)
    return result
```

**Why does this work?**
Each `n & (n-1)` operation removes exactly one 1 bit, so we count how many times we can do this.

**Why is this still not optimal?**
- **Time**: O(n × k) where k is average number of 1 bits - still not O(n)
- Better than brute force, but we can do even better!

#### Approach 3: Dynamic Programming - Use Previous Results

**Why would I think of this?**
"Wait, I'm calculating things from scratch each time. But `countBits[5]` is related to `countBits[4]` or `countBits[2]`... Can I reuse previous calculations?"

Let's look for patterns:

```
Number | Binary | # of 1s | Observation
-------|--------|---------|-------------
0      | 0000   | 0       |
1      | 0001   | 1       | = countBits[0] + 1
2      | 0010   | 1       | = countBits[0] + 1 (new bit position)
3      | 0011   | 2       | = countBits[1] + 1
4      | 0100   | 1       | = countBits[0] + 1 (new bit position)
5      | 0101   | 2       | = countBits[1] + 1
6      | 0110   | 2       | = countBits[2] + 1
7      | 0111   | 3       | = countBits[3] + 1
8      | 1000   | 1       | = countBits[0] + 1 (new bit position)
```

**Pattern discovered!**
- At each power of 2 (1, 2, 4, 8...), there's exactly one 1 bit
- Numbers between powers of 2 build on previous patterns

**Key insight**: `countBits[i] = countBits[i - offset] + 1` where offset is the most recent power of 2 ≤ i.

```python
def countBits(n):
    result = [0] * (n + 1)
    offset = 1  # Most recent power of 2
    
    for i in range(1, n + 1):
        # When we hit a new power of 2, update offset
        if offset * 2 == i:
            offset = i
        
        # Current count = count at (i - offset) + 1
        result[i] = result[i - offset] + 1
    
    return result
```

**Let's trace through n = 5**:

```
result = [0, 0, 0, 0, 0, 0]
offset = 1

i = 1:
  offset * 2 = 2 ≠ 1, so offset stays 1
  result[1] = result[1-1] + 1 = result[0] + 1 = 0 + 1 = 1
  result = [0, 1, 0, 0, 0, 0]

i = 2:
  offset * 2 = 2 == 2, so offset = 2
  result[2] = result[2-2] + 1 = result[0] + 1 = 0 + 1 = 1
  result = [0, 1, 1, 0, 0, 0]

i = 3:
  offset * 2 = 4 ≠ 3, so offset stays 2
  result[3] = result[3-2] + 1 = result[1] + 1 = 1 + 1 = 2
  result = [0, 1, 1, 2, 0, 0]

i = 4:
  offset * 2 = 4 == 4, so offset = 4
  result[4] = result[4-4] + 1 = result[0] + 1 = 0 + 1 = 1
  result = [0, 1, 1, 2, 1, 0]

i = 5:
  offset * 2 = 8 ≠ 5, so offset stays 4
  result[5] = result[5-4] + 1 = result[1] + 1 = 1 + 1 = 2
  result = [0, 1, 1, 2, 1, 2]

Final: [0, 1, 1, 2, 1, 2] ✓
```

**Analysis**:
- **Time**: O(n) ✓
- **Space**: O(1) extra (excluding output) ✓

#### Approach 4: Using i & (i-1) (Alternative DP)

**Why would I think of this?**
"I know `i & (i-1)` removes one 1 bit. So `countBits[i]` must be `countBits[i & (i-1)] + 1`!"

```python
def countBits(n):
    result = [0] * (n + 1)
    for i in range(1, n + 1):
        result[i] = result[i & (i - 1)] + 1
    return result
```

**Why does this work?**
- `i & (i-1)` is always less than `i` (we've removed a 1 bit)
- So we already calculated `result[i & (i-1)]` earlier
- Adding 1 accounts for the bit we removed

**Let's trace through n = 5**:

```
result = [0, 0, 0, 0, 0, 0]

i = 1:
  1 & 0 = 0
  result[1] = result[0] + 1 = 1
  result = [0, 1, 0, 0, 0, 0]

i = 2:
  2 & 1 = 10 & 01 = 00 = 0
  result[2] = result[0] + 1 = 1
  result = [0, 1, 1, 0, 0, 0]

i = 3:
  3 & 2 = 11 & 10 = 10 = 2
  result[3] = result[2] + 1 = 2
  result = [0, 1, 1, 2, 0, 0]

i = 4:
  4 & 3 = 100 & 011 = 000 = 0
  result[4] = result[0] + 1 = 1
  result = [0, 1, 1, 2, 1, 0]

i = 5:
  5 & 4 = 101 & 100 = 100 = 4
  result[5] = result[4] + 1 = 2
  result = [0, 1, 1, 2, 1, 2]

Final: [0, 1, 1, 2, 1, 2] ✓
```

This is the cleanest solution! Just one line in the loop.

---

### 6.5 Reverse Bits

📌 **LeetCode Link**: [190. Reverse Bits](https://leetcode.com/problems/reverse-bits/)

#### Problem Statement

Reverse bits of a given 32 bits unsigned integer.

#### Examples

```
Example 1:
Input: n = 43261596 
       (binary: 00000010100101000001111010011100)
Output: 964176192 
       (binary: 00111001011110000010100101000000)

Example 2:
Input: n = 4294967293
       (binary: 11111111111111111111111111111101)
Output: 3221225471
       (binary: 10111111111111111111111111111111)
```

#### Approach 1: Simple Bit-by-Bit Reversal

**Why would I think of this?**
"To reverse bits, I need to take each bit from the right side of the original and place it on the left side of the result. I'll process 32 bits one by one."

```python
def reverseBits(n):
    result = 0
    for i in range(32):
        # Get the rightmost bit of n
        bit = n & 1
        
        # Shift result left and add the bit
        result = (result << 1) | bit
        
        # Shift n right to get next bit
        n >>= 1
    
    return result
```

**Step-by-step explanation**:

1. `n & 1` extracts the rightmost bit (0 or 1)
2. `result << 1` shifts result left, making room for new bit
3. `result | bit` adds the new bit to the rightmost position
4. `n >>= 1` removes the bit we just processed from n

**Let's trace through a simple 4-bit example**:

```
Reverse bits of 5 (0101) to get 10 (1010)

Initial: n = 0101, result = 0000

i = 0:
  bit = 0101 & 1 = 1
  result = (0000 << 1) | 1 = 0000 | 1 = 0001
  n = 0101 >> 1 = 0010

i = 1:
  bit = 0010 & 1 = 0
  result = (0001 << 1) | 0 = 0010 | 0 = 0010
  n = 0010 >> 1 = 0001

i = 2:
  bit = 0001 & 1 = 1
  result = (0010 << 1) | 1 = 0100 | 1 = 0101
  n = 0001 >> 1 = 0000

i = 3:
  bit = 0000 & 1 = 0
  result = (0101 << 1) | 0 = 1010 | 0 = 1010
  n = 0000 >> 1 = 0000

Final: result = 1010 = 10 ✓
```

**Analysis**:
- **Time**: O(32) = O(1) - Always 32 iterations
- **Space**: O(1)

#### Why other approaches might fail or be overcomplicated:

**String conversion approach**:
```python
def reverseBits_string(n):
    binary = bin(n)[2:].zfill(32)  # Convert to 32-bit binary string
    reversed_binary = binary[::-1]  # Reverse string
    return int(reversed_binary, 2)  # Convert back to int
```

**Why I might think of this**: "I know how to reverse strings! Let me convert to string, reverse, convert back."

**Why this is not ideal**:
- Creates extra strings (space overhead)
- String operations are slower than bit operations
- Not demonstrating bit manipulation skills in interview

---

### 6.6 Missing Number (Using XOR)

📌 **LeetCode Link**: [268. Missing Number](https://leetcode.com/problems/missing-number/)

We covered this in the Arrays section, but let's revisit the XOR solution!

#### Problem Statement

Given an array `nums` containing n distinct numbers in the range `[0, n]`, return the only number missing.

#### XOR Solution

**Why would I think of XOR?**
"I have numbers 0 to n, but one is missing. If I XOR all the numbers I have with all the numbers I should have, pairs will cancel, leaving the missing one!"

```python
def missingNumber(nums):
    result = len(nums)  # Start with n (the largest expected number)
    
    for i, num in enumerate(nums):
        result ^= i ^ num
    
    return result
```

**Why does this work?**

```
nums = [3, 0, 1], n = 3
Expected: 0, 1, 2, 3
Have: 3, 0, 1
Missing: 2

XOR all indices: 0 ^ 1 ^ 2 = 3
XOR all values: 3 ^ 0 ^ 1 = 2
XOR together with n=3: 3 ^ 3 ^ 2 = 2

More systematically:
result = 3 (n)
i=0, num=3: result = 3 ^ 0 ^ 3 = 0
i=1, num=0: result = 0 ^ 1 ^ 0 = 1
i=2, num=1: result = 1 ^ 2 ^ 1 = 2

Final: 2 ✓
```

**Analysis**:
- **Time**: O(n)
- **Space**: O(1)

---

### 6.7 Power of Two

📌 **LeetCode Link**: [231. Power of Two](https://leetcode.com/problems/power-of-two/)

#### Problem Statement

Given an integer `n`, return `true` if it is a power of two. Otherwise, return `false`.

#### Examples

```
Example 1:
Input: n = 1
Output: true (2⁰ = 1)

Example 2:
Input: n = 16
Output: true (2⁴ = 16)

Example 3:
Input: n = 3
Output: false
```

#### Approach 1: Loop Division

**Why would I think of this?**
"A power of 2 means I can keep dividing by 2 until I get 1. If I get a non-integer along the way, it's not a power of 2."

```python
def isPowerOfTwo_loop(n):
    if n <= 0:
        return False
    
    while n > 1:
        if n % 2 != 0:  # Not divisible by 2
            return False
        n //= 2
    
    return True
```

**Why does this work?**
Powers of 2: 1, 2, 4, 8, 16... Each is the previous × 2. So dividing by 2 repeatedly should give us 1.

**Why is this not optimal?**
- **Time**: O(log n) - Need log₂(n) divisions
- We can do O(1) with bit manipulation!

#### Approach 2: Bit Counting

**Why would I think of this?**
"Powers of 2 in binary have exactly ONE 1 bit!"

```
1  = 00001
2  = 00010
4  = 00100
8  = 01000
16 = 10000
```

```python
def isPowerOfTwo_count(n):
    if n <= 0:
        return False
    
    count = 0
    while n:
        count += n & 1
        n >>= 1
    
    return count == 1
```

**Why does this work?**
Powers of 2 have exactly one 1 bit, so if count == 1, it's a power of 2.

**Why is this still not the best?**
- **Time**: O(log n) - Still need to check all bits

#### Approach 3: n & (n-1) Trick (Optimal!)

**Why would I think of this?**
"I know `n & (n-1)` removes the rightmost 1 bit. If n is a power of 2, it has only ONE 1 bit. So after removing it, the result should be 0!"

```
n = 8  = 1000
n-1 = 7 = 0111
n & (n-1) = 0000 = 0 ✓ (power of 2)

n = 6  = 0110
n-1 = 5 = 0101
n & (n-1) = 0100 = 4 ≠ 0 ✗ (not power of 2)
```

```python
def isPowerOfTwo(n):
    return n > 0 and (n & (n - 1)) == 0
```

**Analysis**:
- **Time**: O(1) ✓
- **Space**: O(1) ✓

**Edge cases**:
- `n = 0`: 0 is not a power of 2, and `0 & (-1)` would give wrong result, so we check `n > 0`
- `n < 0`: Negative numbers are not powers of 2

---

### 6.8 Power of Four

📌 **LeetCode Link**: [342. Power of Four](https://leetcode.com/problems/power-of-four/)

#### Problem Statement

Given an integer `n`, return `true` if it is a power of four. Otherwise, return `false`.

#### Examples

```
Example 1:
Input: n = 16
Output: true (4² = 16)

Example 2:
Input: n = 5
Output: false

Example 3:
Input: n = 1
Output: true (4⁰ = 1)
```

#### Approach 1: Loop Division

**Why would I think of this?**
"Same as power of 2, but divide by 4 instead!"

```python
def isPowerOfFour_loop(n):
    if n <= 0:
        return False
    
    while n > 1:
        if n % 4 != 0:
            return False
        n //= 4
    
    return True
```

**Analysis**: O(log₄ n) time

#### Approach 2: Power of 2 + Position Check

**Why would I think of this?**
"Powers of 4 are also powers of 2, but the 1 bit must be at an EVEN position (0, 2, 4, 6...)!"

```
1  = 0001  (bit at position 0) ✓
4  = 0100  (bit at position 2) ✓
16 = 10000 (bit at position 4) ✓

2  = 0010  (bit at position 1) ✗
8  = 1000  (bit at position 3) ✗
```

**How to check for even position?**
Use a mask with 1s at all even positions: `0x55555555` = `01010101010101010101010101010101` in binary.

```python
def isPowerOfFour(n):
    # Check: positive, power of 2, and 1-bit at even position
    return n > 0 and (n & (n - 1)) == 0 and (n & 0x55555555) != 0
```

**Breaking it down**:
1. `n > 0`: Must be positive
2. `(n & (n - 1)) == 0`: Must be power of 2 (single 1 bit)
3. `(n & 0x55555555) != 0`: That 1 bit must be at even position

**Analysis**:
- **Time**: O(1) ✓
- **Space**: O(1) ✓

---

### 6.9 Bitwise AND of Numbers Range

📌 **LeetCode Link**: [201. Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

#### Problem Statement

Given two integers `left` and `right` that represent the range `[left, right]`, return the bitwise AND of all numbers in this range, inclusive.

#### Examples

```
Example 1:
Input: left = 5, right = 7
Output: 4
Explanation: 5 & 6 & 7 = 4
  5 = 101
  6 = 110
  7 = 111
  -------
  AND = 100 = 4

Example 2:
Input: left = 0, right = 0
Output: 0

Example 3:
Input: left = 1, right = 2147483647
Output: 0
```

#### Approach 1: Brute Force (Don't use!)

**Why would I think of this?**
"Just AND all numbers in the range!"

```python
def rangeBitwiseAnd_brute(left, right):
    result = left
    for i in range(left + 1, right + 1):
        result &= i
    return result
```

**Why does this fail?**
- **Time**: O(right - left) - Could be billions of numbers!
- Will time out on large ranges

#### Approach 2: Find Common Prefix

**Why would I think of this?**
"When I AND numbers, any bit that changes in the range will become 0. Only bits that stay the same throughout the range will remain 1. So I need to find the common prefix!"

```
5 = 101
6 = 110
7 = 111

The rightmost bits keep changing, only the leftmost '1' survives as... wait, that's not quite right.

Actually, let's look more carefully:
5 = 101
6 = 110  <- bit 0 and 1 change
7 = 111

Result should be: bits that are 1 in ALL numbers
Bit 2: 1, 1, 1 → 1
Bit 1: 0, 1, 1 → 0
Bit 0: 1, 0, 1 → 0

Result = 100 = 4 ✓
```

**Key insight**: We need to find the common PREFIX of left and right (the bits that don't change). All other bits will eventually flip in the range.

```python
def rangeBitwiseAnd(left, right):
    shift = 0
    
    # Find common prefix by shifting until left == right
    while left < right:
        left >>= 1
        right >>= 1
        shift += 1
    
    # Shift back to original position
    return left << shift
```

**Let's trace through left=5, right=7**:

```
Initial: left=5 (101), right=7 (111), shift=0

Iteration 1:
  left < right (5 < 7)? Yes
  left = 5 >> 1 = 2 (10)
  right = 7 >> 1 = 3 (11)
  shift = 1

Iteration 2:
  left < right (2 < 3)? Yes
  left = 2 >> 1 = 1 (1)
  right = 3 >> 1 = 1 (1)
  shift = 2

Iteration 3:
  left < right (1 < 1)? No
  Exit loop

Result = 1 << 2 = 4 (100) ✓
```

**Why does this work?**
- When we shift both numbers right until they're equal, we've found their common prefix
- All the bits we shifted away are bits that differ somewhere in the range
- Those differing bits will become 0 when ANDed
- Shifting back puts the common prefix in the right position

**Analysis**:
- **Time**: O(log n) - At most 32 shifts
- **Space**: O(1)

---

## 7. Bit Manipulation Tricks and Patterns

Here are essential bit manipulation tricks that appear frequently in interviews:

### 7.1 Check if Number is Even or Odd

```python
def is_even(n):
    return (n & 1) == 0

def is_odd(n):
    return (n & 1) == 1
```

**Why it works**: The last bit determines odd/even:
- Even numbers end in 0: 2=10, 4=100, 6=110
- Odd numbers end in 1: 1=1, 3=11, 5=101

### 7.2 Multiply/Divide by Powers of 2

```python
# Multiply by 2^k
n << k  # n * (2^k)

# Divide by 2^k (integer division)
n >> k  # n // (2^k)

# Examples:
5 << 1   # 5 * 2 = 10
5 << 3   # 5 * 8 = 40
20 >> 2  # 20 // 4 = 5
```

### 7.3 Get the Rightmost 1 Bit

```python
rightmost_one = n & (-n)

# Example:
# n = 12 = 1100
# -n = ...11110100 (two's complement)
# n & (-n) = 0100 = 4
```

### 7.4 Remove the Rightmost 1 Bit

```python
n = n & (n - 1)

# Example:
# n = 12 = 1100
# n-1 = 11 = 1011
# n & (n-1) = 1000 = 8
```

### 7.5 Check if Number is Power of 2

```python
is_power_of_2 = n > 0 and (n & (n - 1)) == 0
```

### 7.6 Set a Specific Bit to 1

```python
# Set bit at position k to 1
n = n | (1 << k)

# Example: Set bit 2 of n=5
# 5 = 0101
# 1 << 2 = 0100
# 0101 | 0100 = 0101 (already 1)

# Set bit 1 of n=5
# 5 = 0101
# 1 << 1 = 0010
# 0101 | 0010 = 0111 = 7
```

### 7.7 Clear a Specific Bit (Set to 0)

```python
# Clear bit at position k
n = n & ~(1 << k)

# Example: Clear bit 2 of n=7
# 7 = 0111
# 1 << 2 = 0100
# ~0100 = 1011
# 0111 & 1011 = 0011 = 3
```

### 7.8 Toggle a Specific Bit (Flip)

```python
# Toggle bit at position k
n = n ^ (1 << k)

# Example: Toggle bit 1 of n=5
# 5 = 0101
# 1 << 1 = 0010
# 0101 ^ 0010 = 0111 = 7
```

### 7.9 Check if a Specific Bit is Set

```python
# Check if bit at position k is 1
is_set = (n >> k) & 1 == 1
# or
is_set = (n & (1 << k)) != 0

# Example: Is bit 2 of n=5 set?
# 5 = 0101
# (5 >> 2) & 1 = 01 & 1 = 1 → Yes
```

### 7.10 Swap Two Numbers Without Temp Variable

```python
def swap(a, b):
    a = a ^ b
    b = a ^ b  # b = (a^b)^b = a
    a = a ^ b  # a = (a^b)^a = b
    return a, b

# Example:
a, b = 5, 3
a = 5 ^ 3 = 6
b = 6 ^ 3 = 5
a = 6 ^ 5 = 3
# Now a=3, b=5
```

---

## 8. Practice Problems

### Easy Problems

| Problem | Link | Key Technique |
|---------|------|---------------|
| Single Number | [LeetCode 136](https://leetcode.com/problems/single-number/) | XOR all numbers |
| Number of 1 Bits | [LeetCode 191](https://leetcode.com/problems/number-of-1-bits/) | n & (n-1) |
| Counting Bits | [LeetCode 338](https://leetcode.com/problems/counting-bits/) | DP with bits |
| Reverse Bits | [LeetCode 190](https://leetcode.com/problems/reverse-bits/) | Bit shifting |
| Missing Number | [LeetCode 268](https://leetcode.com/problems/missing-number/) | XOR |
| Power of Two | [LeetCode 231](https://leetcode.com/problems/power-of-two/) | n & (n-1) |
| Power of Four | [LeetCode 342](https://leetcode.com/problems/power-of-four/) | Bit mask |
| Add Binary | [LeetCode 67](https://leetcode.com/problems/add-binary/) | Bit addition |

### Medium Problems

| Problem | Link | Key Technique |
|---------|------|---------------|
| Single Number II | [LeetCode 137](https://leetcode.com/problems/single-number-ii/) | Count bits mod 3 |
| Single Number III | [LeetCode 260](https://leetcode.com/problems/single-number-iii/) | XOR + grouping |
| Bitwise AND of Range | [LeetCode 201](https://leetcode.com/problems/bitwise-and-of-numbers-range/) | Common prefix |
| Sum of Two Integers | [LeetCode 371](https://leetcode.com/problems/sum-of-two-integers/) | XOR + AND |
| Subsets | [LeetCode 78](https://leetcode.com/problems/subsets/) | Bit masking |
| UTF-8 Validation | [LeetCode 393](https://leetcode.com/problems/utf-8-validation/) | Bit pattern matching |
