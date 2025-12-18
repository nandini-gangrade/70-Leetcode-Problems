# Complete Stacks Guide for Interview Preparation

## Table of Contents
1. [What is a Stack?](#what-is-a-stack)
2. [Stack in Real-World Applications](#stack-in-real-world-applications)
3. [Stack Operations & Implementation](#stack-operations--implementation)
4. [Problems](#problems)
   - Min Stack
   - Valid Parentheses
   - Evaluate Reverse Polish Notation
   - Daily Temperatures
   - Next Greater Element
   - Largest Rectangle in Histogram
   - Trapping Rain Water
5. [Advanced Stack Concepts](#advanced-stack-concepts)

---

## What is a Stack?

A **stack** is a linear data structure that follows the **LIFO** (Last In, First Out) principle. This means the last element added is the first one to be removed.

### Visual Representation

```
        TOP
         ↓
    [   5   ]  ← Last In, First Out
    [   3   ]
    [   7   ]
    [   2   ]  ← First In, Still Inside
    [_______]
     BOTTOM
```

### Real-World Analogies

**1. Stack of Plates in a Cafeteria:**
```
You can only:
- Add a plate on top (push)
- Remove the top plate (pop)
- Look at the top plate (peek)

    [Plate 3] ← Can remove
    [Plate 2]
    [Plate 1] ← Cannot remove without removing 2 & 3
    [  Table ]
```

**2. Browser Back Button:**
```
Visit pages:
Page 1 → Page 2 → Page 3 (current)

Stack: [Page 1, Page 2, Page 3]
                        ↑
                      Current

Click "Back":
- Remove Page 3 (pop)
- Show Page 2 (now at top)

This is LIFO!
```

**3. Undo/Redo in Text Editors:**
```
Actions:
Type "Hello" → Delete "o" → Type "p"

Undo stack: [Type "Hello", Delete "o", Type "p"]
                                         ↑
                                    Most recent

Press Undo:
- Remove "Type p" (pop)
- Revert to "Hell"
```

**4. Function Call Stack (Very Important!):**
```python
def funcA():
    funcB()
    
def funcB():
    funcC()
    
def funcC():
    print("Hello")

# When funcA() is called:

Call Stack:
[funcC]  ← Currently executing
[funcB]  ← Waiting for funcC to finish
[funcA]  ← Waiting for funcB to finish

When funcC finishes:
- Pop funcC from stack
- Resume funcB

This is how JavaScript/Python tracks function calls!
```

---

## Stack in Real-World Applications

### 1. JavaScript Runtime Environment (Call Stack)

**What is a call stack?**

When you write JavaScript code, the engine needs to track which function is currently running and which functions are waiting.

```javascript
function first() {
    console.log("First");
    second();
}

function second() {
    console.log("Second");
    third();
}

function third() {
    console.log("Third");
}

first();

// Call Stack progression:

Step 1: [first()]
        Logs "First"

Step 2: [first(), second()]
        Logs "Second"

Step 3: [first(), second(), third()]
        Logs "Third"

Step 4: [first(), second()]
        third() finishes, pop from stack

Step 5: [first()]
        second() finishes, pop from stack

Step 6: []
        first() finishes, stack empty
```

**Why use a stack?**
- Ensures functions finish in reverse order of calling
- Manages scope and local variables
- Handles return values properly

### 2. Compilers & Parsers

**Language parsers** use stacks to check syntax:

```python
# Check if brackets are balanced:
code = "if (x > 0) { print(x); }"

Stack helps check:
- '(' matches with ')'
- '{' matches with '}'

This is exactly what we'll do in "Valid Parentheses" problem!
```

### 3. Expression Evaluation

**Infix to Postfix conversion:**
```
Infix:    3 + 4 * 2
Postfix:  3 4 2 * +

Stack helps convert and evaluate expressions!
(We'll see this in Reverse Polish Notation problem)
```

---

## Stack Operations & Implementation

### Core Operations

| Operation | Description | Time Complexity |
|-----------|-------------|-----------------|
| `push(x)` | Add element to top | O(1) |
| `pop()` | Remove top element | O(1) |
| `peek()` / `top()` | View top element | O(1) |
| `isEmpty()` | Check if stack is empty | O(1) |
| `size()` | Get number of elements | O(1) |

### Why All Operations are O(1)

**Key insight:** We only interact with the top of the stack!

```
Push: Just add to end of array → O(1)
[1, 2, 3] → [1, 2, 3, 4]
             Add here ↑

Pop: Just remove from end → O(1)
[1, 2, 3, 4] → [1, 2, 3]
Remove this ↑

Peek: Just look at last element → O(1)
[1, 2, 3, 4]
Look here ↑
```

### Implementation in Python

```python
class Stack:
    def __init__(self):
        """Initialize empty stack using list"""
        self.items = []
    
    def push(self, item):
        """Add item to top of stack"""
        self.items.append(item)  # O(1)
    
    def pop(self):
        """Remove and return top item"""
        if not self.isEmpty():
            return self.items.pop()  # O(1)
        return None
    
    def peek(self):
        """View top item without removing"""
        if not self.isEmpty():
            return self.items[-1]  # O(1)
        return None
    
    def isEmpty(self):
        """Check if stack is empty"""
        return len(self.items) == 0  # O(1)
    
    def size(self):
        """Get number of items"""
        return len(self.items)  # O(1)
    
    def __str__(self):
        """String representation for debugging"""
        return str(self.items)
```

### Using the Stack

```python
# Create stack
stack = Stack()

# Push elements
stack.push(1)
stack.push(2)
stack.push(3)
print(stack)  # [1, 2, 3]

# Peek at top
print(stack.peek())  # 3 (doesn't remove)
print(stack)         # [1, 2, 3] (still there)

# Pop element
print(stack.pop())   # 3 (removes and returns)
print(stack)         # [1, 2] (3 is gone)

# Check if empty
print(stack.isEmpty())  # False

# Get size
print(stack.size())  # 2
```

### Common Python List Operations for Stacks

```python
# Using Python list as stack:
stack = []

# Push
stack.append(5)    # [5]
stack.append(3)    # [5, 3]

# Pop
top = stack.pop()  # top = 3, stack = [5]

# Peek (without removing)
top = stack[-1]    # top = 5, stack still [5]

# Check if empty
if stack:          # If stack has elements
    print("Not empty")

if not stack:      # If stack is empty
    print("Empty")

# Size
size = len(stack)
```

---

## Problems

### Problem 1: Min Stack (Already Covered - Enhanced)

**[LeetCode 155 - Min Stack](https://leetcode.com/problems/min-stack/)**

This problem was already covered above, but let's add more depth.

#### Why This Problem Matters

**Real-world application:**
Imagine you're building a trading platform. You need to:
- Track current price (top of stack)
- Know the minimum price seen so far (for stop-loss)
- Both operations must be instant (O(1))

#### The Challenge

Normal stack operations are O(1), but finding minimum in a regular array is O(n):

```python
# Regular approach - O(n)
def getMin(stack):
    return min(stack)  # Must check every element!

# For [5, 1, 3, 2]:
# min() checks: 5, 1, 3, 2 → returns 1
# That's 4 operations!
```

**Problem:** If we have 1 million elements, finding min takes 1 million operations!

#### Solution Evolution

**Attempt 1: Store min separately (WRONG)**

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_val = float('inf')
    
    def push(self, val):
        self.stack.append(val)
        self.min_val = min(self.min_val, val)
    
    def pop(self):
        val = self.stack.pop()
        # Problem: What if we popped the min?
        # min_val is now wrong!
        # Would need to scan entire stack again!
```

**Why this fails:**
```
push(2): stack=[2], min=2
push(1): stack=[2,1], min=1
push(3): stack=[2,1,3], min=1

pop(): stack=[2,1], min=1 (still correct)
pop(): stack=[2], min=1 ❌ WRONG! Should be 2!

We lost track of previous minimums!
```

**Attempt 2: Recalculate min after pop (WRONG for O(1))**

```python
def pop(self):
    self.stack.pop()
    if self.stack:
        self.min_val = min(self.stack)  # O(n) - Not allowed!
    else:
        self.min_val = float('inf')
```

**Why this fails:**
- Finding min is O(n)
- We need O(1)!

#### Correct Solution: Track Min History

**Key insight:** Store minimum AT EACH POINT in history!

**Approach 1: Stack of Tuples**

```python
class MinStack:
    def __init__(self):
        self.stack = []
    
    def push(self, val: int) -> None:
        # Store (value, current_minimum)
        if not self.stack:
            self.stack.append((val, val))
        else:
            current_min = min(val, self.stack[-1][1])
            self.stack.append((val, current_min))
    
    def pop(self) -> None:
        self.stack.pop()
    
    def top(self) -> int:
        return self.stack[-1][0]  # Return value only
    
    def getMin(self) -> int:
        return self.stack[-1][1]  # Return current min
```

**Detailed Walkthrough:**

```python
minStack = MinStack()

# Operation 1: push(-2)
minStack.push(-2)
# Stack: [(-2, -2)]
#         val  min
# Current min is -2

# Operation 2: push(0)
minStack.push(0)
# min(0, -2) = -2
# Stack: [(-2, -2), (0, -2)]
# Current min still -2

# Operation 3: push(-3)
minStack.push(-3)
# min(-3, -2) = -3
# Stack: [(-2, -2), (0, -2), (-3, -3)]
# Current min now -3

# Operation 4: getMin()
minStack.getMin()  # Returns -3 ✓

# Operation 5: pop()
minStack.pop()
# Stack: [(-2, -2), (0, -2)]
# Current min automatically restored to -2!

# Operation 6: top()
minStack.top()  # Returns 0 ✓

# Operation 7: getMin()
minStack.getMin()  # Returns -2 ✓
```

**Why this works:**

```
When we push, we store:
- The actual value
- What the minimum is AT THIS POINT

When we pop:
- We remove current value AND its minimum
- Previous minimum is automatically restored!

Example:
[(-2,-2), (0,-2), (-3,-3)]
                    ↑
              Current min = -3

After pop:
[(-2,-2), (0,-2)]
             ↑
        Min automatically = -2!
```

**Visual representation:**

```
Stack state over time:

push(-2):
┌────────┐
│ (-2,-2)│ ← Top, min = -2
└────────┘

push(0):
┌────────┐
│ (0,-2) │ ← Top, min = -2
├────────┤
│ (-2,-2)│
└────────┘

push(-3):
┌────────┐
│(-3,-3) │ ← Top, min = -3
├────────┤
│ (0,-2) │
├────────┤
│ (-2,-2)│
└────────┘

pop():
┌────────┐
│ (0,-2) │ ← Top, min = -2 (restored!)
├────────┤
│ (-2,-2)│
└────────┘
```

#### Approach 2: Two Stacks (Space Optimized)

**Why optimize?**
- Tuple approach stores min for EVERY element
- Many elements might have same min
- Can we store min more efficiently?

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []  # Only stores actual minimums
    
    def push(self, val: int) -> None:
        self.stack.append(val)
        
        # Only push to min_stack if it's a new minimum
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
    
    def pop(self) -> None:
        val = self.stack.pop()
        
        # Only pop from min_stack if we're removing current min
        if val == self.min_stack[-1]:
            self.min_stack.pop()
    
    def top(self) -> int:
        return self.stack[-1]
    
    def getMin(self) -> int:
        return self.min_stack[-1]
```

**Why `<=` and not just `<`?**

**Critical:** Handle duplicates correctly!

```python
# Example showing why we need <=

push(1):
stack:     [1]
min_stack: [1]

push(1):  # Duplicate!
stack:     [1, 1]
min_stack: [1, 1]  ← Must push again!

# Why? Watch what happens with pop:

pop():
stack:     [1]
min_stack: [1]  ← Still have a 1!

If we used < instead:
push(1):
stack:     [1, 1]
min_stack: [1]  ← Only one 1!

pop():
stack:     [1]
min_stack: []  ← WRONG! Lost minimum!
```

**Detailed walkthrough:**

```python
minStack = MinStack()

# push(-2)
# -2 <= nothing, so push to both
stack:     [-2]
min_stack: [-2]

# push(0)
# 0 > -2, don't push to min_stack
stack:     [-2, 0]
min_stack: [-2]

# push(-3)
# -3 <= -2, push to both
stack:     [-2, 0, -3]
min_stack: [-2, -3]

# getMin() → -3 ✓

# pop() removes -3
# -3 == min_stack top, so pop from min_stack too
stack:     [-2, 0]
min_stack: [-2]

# getMin() → -2 ✓
```

**Space comparison:**

```
Tuple approach:
push(-2): [(-2,-2)]
push(0):  [(-2,-2), (0,-2)]
push(-3): [(-2,-2), (0,-2), (-3,-3)]
Space: 6 values stored

Two stacks approach:
stack:     [-2, 0, -3]
min_stack: [-2, -3]
Space: 5 values stored ✓ Better!

Even better for:
push(5): [5,5,5,5,5,5,5,5]
min_stack: [5]  ← Only one entry!
```

#### Common Mistakes Summary

**Mistake 1: Not handling duplicates**
```python
# WRONG
if val < self.min_stack[-1]:  # Missing equals!

# RIGHT
if val <= self.min_stack[-1]:
```

**Mistake 2: Always popping from min_stack**
```python
# WRONG
def pop(self):
    self.stack.pop()
    self.min_stack.pop()  # Always pop!

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
    return self.stack[-1]  # Returns (-3, -3)!

# RIGHT
def top(self):
    return self.stack[-1][0]  # Returns -3 only
```

---

### Problem 2: Valid Parentheses

**[LeetCode 20 - Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)**

#### Problem Statement

Given a string `s` containing just the characters `'(', ')', '{', '}', '[', ']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets
2. Open brackets must be closed in the correct order
3. Every close bracket has a corresponding open bracket of the same type

**Examples:**
```python
Input: s = "()"
Output: true

Input: s = "()[]{}"
Output: true

Input: s = "(]"
Output: false

Input: s = "([)]"
Output: false

Input: s = "{[]}"
Output: true
```

#### Understanding the Problem

**What makes brackets valid?**

```
Valid patterns:
()       - Simple pair
[]{}     - Multiple pairs in sequence
{[]}     - Nested, closed in reverse order
([{}])   - Complex nesting

Invalid patterns:
(]       - Wrong closing bracket
([)]     - Wrong order (should be ([]) or [()])
(((      - Missing closing brackets
)))      - Missing opening brackets
```

**Why is this important?**

This is **exactly** how compilers check syntax!

```python
# Python code:
if (x > 0) {  # SYNTAX ERROR!
    print(x)
}

# Compiler checks:
# '(' found → expect ')'
# But found '{' → ERROR!
```

```javascript
// JavaScript code:
function test() {
    if (x > 0) {
        console.log(x);
    }
}

// Parser must ensure:
// Every { has matching }
// Every ( has matching )
// All closed in correct order
```

#### Naive Approach (WRONG)

**Attempt 1: Count brackets**

```python
def isValid(s):
    open_count = s.count('(') + s.count('[') + s.count('{')
    close_count = s.count(')') + s.count(']') + s.count('}')
    return open_count == close_count
```

**Why this fails:**
```python
s = "([)]"
open_count = 2
close_count = 2
Returns True ❌ But should be False!

Problem: Doesn't check ORDER or MATCHING TYPES
```

**Attempt 2: Check each type separately**

```python
def isValid(s):
    return (s.count('(') == s.count(')') and
            s.count('[') == s.count(']') and
            s.count('{') == s.count('}'))
```

**Why this fails:**
```python
s = "([)]"
'(' count = ')' count = 1 ✓
'[' count = ']' count = 1 ✓
'{' count = '}' count = 0 ✓
Returns True ❌ But should be False!

Problem: Still doesn't check ORDER!
```

#### Correct Approach: Stack

**Key insight:** Last opened bracket must be first closed! (LIFO!)

```
Pattern: {[()]}

Open {: Stack = ['{']
Open [: Stack = ['{', '[']
Open (: Stack = ['{', '[', '(']
Close ): Must match ( ✓ Stack = ['{', '[']
Close ]: Must match [ ✓ Stack = ['{']
Close }: Must match { ✓ Stack = []

Valid!
```

**Algorithm:**
1. When we see opening bracket → push to stack
2. When we see closing bracket → check if it matches top of stack
3. If matches → pop from stack
4. If doesn't match → invalid
5. At end, stack should be empty

#### Visual Step-by-Step

**Example 1: `"()[]{}"`**

```
Character: '('
Action: Opening bracket, push
Stack: ['(']

Character: ')'
Action: Closing bracket
Check: Does ')' match top of stack '('? YES!
Pop: Stack = []

Character: '['
Action: Opening bracket, push
Stack: ['[']

Character: ']'
Action: Closing bracket
Check: Does ']' match top of stack '['? YES!
Pop: Stack = []

Character: '{'
Action: Opening bracket, push
Stack: ['{']

Character: '}'
Action: Closing bracket
Check: Does '}' match top of stack '{'? YES!
Pop: Stack = []

End: Stack is empty ✓
Return True
```

**Example 2: `"{[]}"`**

```
Char: '{'
Stack: ['{']

Char: '['
Stack: ['{', '[']

Char: ']'
Matches '[' ✓
Stack: ['{']

Char: '}'
Matches '{' ✓
Stack: []

Return True ✓
```

**Example 3: `"([)]"` (INVALID)**

```
Char: '('
Stack: ['(']

Char: '['
Stack: ['(', '[']

Char: ')'
Top of stack: '['
Does ')' match '['? NO! ❌
Return False
```

**Example 4: `"((("` (INVALID)**

```
Char: '('
Stack: ['(']

Char: '('
Stack: ['(', '(']

Char: '('
Stack: ['(', '(', '(']

End: Stack not empty ❌
Return False
```

#### Solution

```python
def isValid(s: str) -> bool:
    # Stack to keep track of opening brackets
    stack = []
    
    # Mapping of closing to opening brackets
    closing_to_opening = {
        ')': '(',
        ']': '[',
        '}': '{'
    }
    
    # Process each character
    for char in s:
        # If it's a closing bracket
        if char in closing_to_opening:
            # Check if stack is empty
            if not stack:
                return False  # No matching opening bracket
            
            # Check if top of stack matches
            if stack[-1] == closing_to_opening[char]:
                stack.pop()  # Valid pair, remove opening
            else:
                return False  # Mismatch!
        else:
            # It's an opening bracket, push it
            stack.append(char)
    
    # Valid only if stack is empty
    return len(stack) == 0
```

#### Why Use HashMap/Dictionary?

**Question:** Why not use if-else statements?

```python
# Without HashMap (VERBOSE)
if char == ')':
    if stack[-1] == '(':
        stack.pop()
elif char == ']':
    if stack[-1] == '[':
        stack.pop()
elif char == '}':
    if stack[-1] == '{':
        stack.pop()

# With HashMap (CLEAN)
if stack[-1] == closing_to_opening[char]:
    stack.pop()
```

**Benefits of HashMap:**
1. Cleaner code
2. O(1) lookup time
3. Easy to extend (add more bracket types)
4. More readable

**Important note on dictionary lookup:**

```python
# Checking if key exists
if char in closing_to_opening:  # O(1) - Fast! ✓
    ...

# WRONG way
if char in closing_to_opening.keys():  # O(n) - Slow! ❌
    ...

# Why?
# "in dict" checks keys directly → O(1) hash lookup
# "in dict.keys()" creates a view, then iterates → O(n)
```

#### Alternative Implementation

```python
def isValid(s: str) -> bool:
    stack = []
    pairs = {'(': ')', '[': ']', '{': '}'}
    
    for char in s:
        if char in pairs:  # Opening bracket
            stack.append(char)
        else:  # Closing bracket
            if not stack or pairs[stack.pop()] != char:
                return False
    
    return not stack  # True if empty
```

**Difference:** Maps opening → closing instead of closing → opening

#### Edge Cases

**Edge Case 1: Empty string**
```python
s = ""
Stack = []
End: Stack is empty
Return True ✓ (Valid by definition)
```

**Edge Case 2: Single bracket**
```python
s = "("
Stack = ['(']
End: Stack not empty
Return False ✓
```

**Edge Case 3: Only closing brackets**
```python
s = ")))"
Char: ')'
Stack is empty!
Can't match, Return False ✓
```

**Edge Case 4: Complex nesting**
```python
s = "{[()()]}"
Valid! Stack handles multiple levels
```

#### Common Mistakes

**Mistake 1: Forgetting to check empty stack**
```python
# WRONG
if char in closing_to_opening:
    if stack[-1] == closing_to_opening[char]:  # Error if stack empty!
        stack.pop()

# RIGHT
if char in closing_to_opening:
    if not stack:  # Check first!
        return False
    if stack[-1] == closing_to_opening[char]:
        stack.pop()
```

**Mistake 2: Returning True immediately on match**
```python
# WRONG
if stack[-1] == closing_to_opening[char]:
    stack.pop()
    return True  # Too early!

# RIGHT
# Keep processing all characters
# Return at the end
```

**Mistake 3: Not checking if stack is empty at end**
```python
# WRONG
return True  # Always True if no errors found

# RIGHT
return len(stack) == 0  # or "not stack"
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- We process each character once
- Stack operations (push/pop) are O(1)
- Dictionary lookup is O(1)
- Total: O(n) where n = length of string

**Space Complexity: O(n)**
- Worst case: all opening brackets
- Example: `"(((("`
- Stack size = n

---

### Problem 3: Evaluate Reverse Polish Notation

**[LeetCode 150 - Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)**

#### Problem Statement

You are given an array of strings `tokens` that represents an arithmetic expression in Reverse Polish Notation (RPN). Evaluate the expression and return an integer result.

**Valid operators:** `+`, `-`, `*`, `/`

**Example 1:**
```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**
```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

#### Understanding Reverse Polish Notation (RPN)

**What is RPN?**

RPN (also called postfix notation) is a mathematical notation where operators come AFTER their operands.

**Normal notation (Infix):**
```
3 + 4        Operator between operands
```

**Reverse Polish Notation (Postfix):**
```
3 4 +        Operator after operands
```

#### Why RPN Exists

**1. No need for parentheses!**

```
Infix:  (3 + 4) * 5
RPN:    3 4 + 5 *

Infix:  3 + (4 * 5)
RPN:    3 4 5 * +
```

**2. Easy to evaluate with a stack**

Computers find RPN much easier to evaluate!

**3. Used in calculators**

Old HP calculators used RPN because it's efficient.

#### Notation Comparison

| Infix (Normal) | Postfix (RPN) | Prefix |
|----------------|---------------|--------|
| 3 + 4 | 3 4 + | + 3 4 |
| (3 + 4) * 5 | 3 4 + 5 * | * + 3 4 5 |
| 3 * (4 + 5) | 3 4 5 + * | * 3 + 4 5 |

#### How to Read RPN

**Rule:** When you see an operator, apply it to the two most recent numbers.

**Example: `"2 1 + 3 *"`**

```
Step-by-step reading:

Read: 2       → Remember 2
Read: 1       → Remember 1
Read: +       → Apply + to 2 and 1 → Get 3
Read: 3       → Remember 3
Read: *       → Apply * to 3 and 3 → Get 9

Result: 9
```

**Visual representation:**

```
Expression: 2 1 + 3 *

Step 1: 2
Numbers: [2]

Step 2: 1
Numbers: [2, 1]

Step 3: +
Take last two: 2 + 1 = 3
Numbers: [3]

Step 4: 3
Numbers: [3, 3]

Step 5: *
Take last two: 3 * 3 = 9
Numbers: [9]

Answer: 9
```

#### Complex Example

**Convert to normal math:**
```
RPN: 3 4 + 5 *

Step 1: 3        Stack: [3]
Step 2: 4        Stack: [3, 4]
Step 3: +        3 + 4 = 7, Stack: [7]
Step 4: 5        Stack: [7, 5]
Step 5: *        7 * 5 = 35, Stack: [35]

Answer: 35

In normal notation: (3 + 4) * 5 = 35
```

#### Why Stack is Perfect for RPN

**Key insight:** Stack is LIFO, and we need to process most recent numbers first!

```
When we see operator:
1. Pop last two numbers (LIFO!)
2. Apply operator
3. Push result back (for next operator)

Perfect match for stack behavior!
```

#### Visual Step-by-Step

**Example: `["2","1","+","3","*"]`**

```
Token: "2"
Is it operator? No
Push to stack
Stack: [2]

Token: "1"
Is it operator? No
Push to stack
Stack: [2, 1]

Token: "+"
Is it operator? Yes!
Pop two numbers: right=1, left=2
Calculate: 2 + 1 = 3
Push result back
Stack: [3]
Token: "3"
Is it operator? No
Push to stack
