# 70-Leetcode-Problems -> [Website (Track Your Progress 👁)](https://claude.ai/public/artifacts/c8114343-98b7-442b-a5f4-e3368246412a?fullscreen=true)
[![Watch on YouTube](https://img.youtube.com/vi/lvO88XxNAzs/maxresdefault.jpg)](https://youtu.be/lvO88XxNAzs)

70 LeetCode problems across all major data structures, serving as a complete interview preparation guide. Below is the structured breakdown:

---

## 1. Introduction & Foundation (00:00 - 05:22)

### 1.1 Why Trust This Content
- Creator solved hundreds of LeetCode problems leading to a job at a big tech (FANG) company
- Now on the hiring side, helping others get into Apple, Amazon, etc.

### 1.2 Step Zero: Focus & Deep Work
- Emphasizes the importance of working 2+ hours uninterrupted
- Discussion on how phone/social media usage rewires the brain for short-term feedback, harming productivity

### 1.3 Preparation Steps Overview
- **Step 1**: Learn a programming language (Python recommended)
- **Step 2**: Data structures and algorithms fundamentals
- **Step 3**: This video/LeetCode practice
- **Step 4**: Software engineering foundations (20-page doc mentioned)
- **Steps 5-7**: Interview-related preparation

### 1.4 Key Advice
- Don't just watch—practice by writing notes, solving problems, and doing mock interviews

---

## 2. Big O Notation & Complexity Analysis (05:22 - 12:07)

### 2.1 Time Complexity
- Describes how long an algorithm takes as input grows
- Focus on worst-case scenarios

### 2.2 Space Complexity
- Memory used by the algorithm
- Time complexity generally prioritized over space

### 2.3 Complexity Types (Fastest to Slowest)
| Complexity | Name | Example |
|------------|------|---------|
| O(1) | Constant | Array index access |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Single loop through array |
| O(n log n) | Linearithmic | Merge sort |
| O(n²) | Quadratic | Nested loops |
| O(2ⁿ) | Exponential | Recursive Fibonacci |
| O(n!) | Factorial | Permutations |

---

## 3. Problem-Solving Framework (12:07 - 14:47)

### 3.1 Step-by-Step Process
1. Read the problem twice and understand it
2. Ask clarifying questions (in interviews)
3. Think of multiple approaches (start with brute force)
4. Draw out solutions with pseudocode
5. Code the best solution
6. Review and optimize
7. Study other solutions for learning

---

## 4. Arrays Section (14:47 - 45:03)

### 4.1 Basic Array Problems
- **Contains Duplicate**: Use sets for O(n) solution
- **Missing Number**: Sum formula approach (expected sum - actual sum)
- **Find All Missing Numbers**: Set-based comparison
- **Two Sum**: Hashmap approach for O(n) solution

### 4.2 Array Techniques Covered
- **How Many Numbers Smaller**: Sorting + dictionary approach
- **Minimum Time Visiting All Points**: Key insight—maximum of x/y differences
- **Spiral Matrix (Medium)**: Layer-by-layer traversal with careful boundary handling
- **Number of Islands (Medium)**: BFS/DFS traversal on 2D grid

### 4.3 Two Pointers Pattern
- **Best Time to Buy/Sell Stock**: Greedy approach with left/right pointers
- **Squares of Sorted Array**: Three solutions covered (sort, split-merge, two-pointer)
- **Three Sum (Medium)**: Sort + two pointers, avoiding duplicates
- **Longest Mountain**: Expanding from peaks with two pointers

### 4.4 Sliding Window Pattern
- **Contains Duplicate II**: Fixed-size window with set
- **Minimum Absolute Difference**: Sort + sliding window comparison
- **Minimum Size Subarray Sum (Medium)**: Dynamic window with sum tracking

---

## 5. Bit Manipulation (45:03 - 47:30)

### 5.1 Single Number Problem
- Uses XOR operation (^) to find the unique element
- Key property: n XOR n = 0, n XOR 0 = n
- O(n) time, O(1) space solution

---

## 6. Dynamic Programming (47:30 - 56:05)

### 6.1 Core Concept
- Breaking problems into overlapping subproblems
- Storing/reusing solutions (memoization)

### 6.2 Problems Covered
- **Coin Change (Medium)**: Bottom-up DP with array storing minimum coins for each amount
- **Climbing Stairs**: Fibonacci-style DP (current = previous two sums)
- **Maximum Subarray**: Kadane's algorithm variant
- **Counting Bits**: Using most significant bit pattern recognition
- **Range Sum Query**: Prefix sums for O(1) queries after O(n) preprocessing

---

## 7. Backtracking (56:05 - 1:08:15)

### 7.1 Core Concepts
- **Permutations**: Order matters
- **Combinations**: Order doesn't matter

### 7.2 Problems Covered
- **Letter Case Permutation**: Iterative or recursive building of all case variations
- **Subsets**: Generate all possible subsets using backtracking
- **Combinations**: Choose k elements from n
- **Permutations**: Generate all orderings of array elements

### 7.3 Common Pattern
```
function backtrack(path, start):
    add path to result
    for i from start to end:
        add element to path
        backtrack(path, i+1)
        remove element from path (backtrack)
```

---

## 8. Linked Lists (1:08:15 - 1:26:33)

### 8.1 Problems Covered
- **Middle of Linked List**: Slow/fast pointer technique
- **Linked List Cycle**: Floyd's tortoise and hare algorithm
- **Reverse Linked List**: Three-pointer approach (prev, curr, next)
- **Remove Elements**: Dummy head technique for edge cases
- **Reverse Linked List II (Medium)**: Partial reversal with careful pointer management
- **Palindrome Linked List**: Find middle, reverse second half, compare
- **Merge Two Sorted Lists**: Two-pointer comparison and linking

---

## 9. Stacks (1:26:33 - 1:38:46)

### 9.1 Core Concept
- LIFO (Last In, First Out)

### 9.2 Problems Covered
- **Min Stack (Medium)**: Store (value, current_min) tuples
- **Valid Parentheses**: Match opening/closing brackets with stack
- **Evaluate Reverse Polish Notation (Medium)**: Process operands and operators
- **Stack Sorting**: Use temporary stack to sort elements

---

## 10. Queues (1:38:46 - 1:44:43)

### 10.1 Core Concept
- FIFO (First In, First Out)

### 10.2 Problems Covered
- **Implement Stack Using Queue**: Rearrange elements after each push
- **Time Needed to Buy Tickets**: Logical analysis of queue position
- **Reverse First K Elements**: Use stack to reverse, then reconstruct queue

---

## 11. Binary Trees (1:44:43 - 2:11:55)

### 11.1 Tree Types Reviewed
- Full, Complete, Perfect, Balanced, Degenerate trees
- Leaf nodes definition

### 11.2 Traversal Methods
- **BFS (Breadth-First)**: Level by level using queue
- **DFS (Depth-First)**: Uses stack (iterative) or recursion

### 11.3 Problems Covered
- **Average of Levels**: BFS with level tracking
- **Minimum/Maximum Depth**: BFS for min (early termination), DFS for max
- **Level Order Traversal (Medium)**: BFS returning arrays per level
- **Same Tree**: Simultaneous DFS comparison
- **Path Sum**: DFS with running sum tracking
- **Diameter of Binary Tree**: Recursive depth calculation
- **Invert Binary Tree**: Swap left/right children at each node
- **Lowest Common Ancestor (Medium)**: Track ancestors or use recursive logic

---

## 12. Binary Search Trees (2:11:55 - 2:26:48)

### 12.1 BST Property
- Left child < Parent < Right child

### 12.2 Problems Covered
- **Search in BST**: Binary search logic (go left if smaller, right if larger)
- **Insert into BST (Medium)**: Find correct leaf position
- **Convert Sorted Array to BST (Medium)**: Recursive middle element selection
- **Two Sum IV - BST**: BFS/DFS with set for complement checking
- **Lowest Common Ancestor**: Simplified due to BST ordering
- **Minimum Absolute Difference**: In-order traversal comparison
- **Balance a BST (Medium)**: In-order to array, then rebuild
- **Delete Node in BST (Medium)**: Three cases (no child, one child, two children)
- **Kth Smallest Element (Medium)**: In-order traversal with counter

---

## 13. Heaps (2:26:48 - 2:41:48)

### 13.1 Heap Basics
- Complete binary tree with heap property
- Min-heap: parent ≤ children; Max-heap: parent ≥ children
- Commonly implemented with arrays

### 13.2 Key Operations
- Insert: O(log n)
- Extract min/max: O(log n)
- Heapify: O(n)

### 13.3 Problems Covered (All Medium)
- **Kth Largest Element**: Min-heap of size k
- **K Closest Points to Origin**: Max-heap with distance calculations
- **Top K Frequent Elements**: Counter + heap approach
- **Task Scheduler**: Max-heap with cooling time queue management

---

## 14. Graphs (2:41:48 - 3:01:00)

### 14.1 Graph Fundamentals
- Vertices (nodes) and edges
- Directed vs undirected
- Connected vs disconnected
- Adjacency list representation

### 14.2 Problems Covered
- **Clone Graph (Medium)**: BFS with dictionary mapping original to clone
- **Core Operations**: Find largest node, detect cycle, count edges
- **Cheapest Flights Within K Stops (Medium)**: Bellman-Ford algorithm with relaxation
- **Course Schedule (Medium)**: Cycle detection using DFS

---

## 15. Conclusion & Key Takeaways

### 15.1 Practice Philosophy
- It's advisable to start with basic topics like arrays and strings, progressing through techniques like the two-pointer approach and sliding window. Once comfortable, one can move to more complex data structures like trees and graphs.

### 15.2 Success Factors
- Consistent daily practice
- Understanding patterns, not memorizing solutions
- Drawing out problems before coding
- Reviewing and learning from other solutions

---

**Learn more on Glasp: https://glasp.co/reader?url=https://www.youtube.com/watch?v=lvO88XxNAzs**
