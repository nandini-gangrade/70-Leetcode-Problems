# Complete Queues Guide for Interview Preparation

## Table of Contents
1. [What is a Queue?](#what-is-a-queue)
2. [Queue vs Stack - The Key Difference](#queue-vs-stack---the-key-difference)
3. [Types of Queues](#types-of-queues)
4. [Queue Operations & Implementation](#queue-operations--implementation)
5. [Problems](#problems)
   - Implement Stack using Queues
   - Time Needed to Buy Tickets
   - Reverse First K Elements of Queue
   - Number of Recent Calls
   - Design Circular Queue
6. [Advanced Queue Concepts](#advanced-queue-concepts)

---

## What is a Queue?

A **queue** is a linear data structure that follows the **FIFO** (First In, First Out) principle. The first element added is the first one to be removed.

### Visual Representation

```
FRONT                                    REAR
  ↓                                        ↓
[1] <- [2] <- [3] <- [4] <- [5]
 ↑                              ↑
Remove                        Add
(dequeue)                   (enqueue)

First In → First Out
```

### Real-World Analogies

**1. Queue at a Ticket Counter:**
```
First person in line → First to get ticket
Last person in line → Last to get ticket

Person 1 → Person 2 → Person 3 → Counter
   ↑                              ↑
 First                          Getting
  in                            served
```

**2. Print Queue:**
```
Documents sent to printer:

Document 1 (sent first) → prints first
Document 2 (sent second) → prints second
Document 3 (sent third) → prints third

FIFO order!
```

**3. Restaurant Drive-Through:**
```
Car 1 → Car 2 → Car 3 → Window
 ↑                        ↑
First                   Being
 in                     served

First car to arrive = First car served
```

**4. Breadth-First Search (BFS) in Graphs:**
```
When exploring a graph level by level:

Start → Visit neighbors → Add neighbors to queue
       Process in order they were added

FIFO ensures level-by-level exploration!
```

---

## Queue vs Stack - The Key Difference

This is **CRUCIAL** for interviews!

### Stack (LIFO)

```
Push 1: [1]
Push 2: [1, 2]
Push 3: [1, 2, 3]
         ↑
       TOP

Pop: 3 (last in, first out)
Pop: 2
Pop: 1
```

**Analogy:** Stack of plates
- Add on top
- Remove from top

### Queue (FIFO)

```
Enqueue 1: [1]
Enqueue 2: [1, 2]
Enqueue 3: [1, 2, 3]
            ↑      ↑
          FRONT  REAR

Dequeue: 1 (first in, first out)
Dequeue: 2
Dequeue: 3
```

**Analogy:** Line at store
- Join at back
- Leave from front

### Side-by-Side Comparison

```
STACK                    QUEUE
-----                    -----

Add: [1]                Add: [1]
Add: [1,2]              Add: [1,2]
Add: [1,2,3]            Add: [1,2,3]

Remove: 3 ← Last        Remove: 1 ← First
Remove: 2               Remove: 2
Remove: 1 ← First       Remove: 3 ← Last
```

### When to Use What?

| Use Stack When | Use Queue When |
|----------------|----------------|
| Need to reverse order | Need to maintain order |
| Undo/Redo functionality | Process in arrival order |
| DFS (Depth-First Search) | BFS (Breadth-First Search) |
| Parentheses matching | Scheduling tasks |
| Function call tracking | Buffer management |

---

## Types of Queues

### 1. Simple Queue (Regular Queue)

**Operations:** Enqueue at rear, dequeue from front

```
FRONT                    REAR
  ↓                        ↓
[1] → [2] → [3] → [4] → [5]

Enqueue 6: [1] → [2] → [3] → [4] → [5] → [6]
Dequeue:   [2] → [3] → [4] → [5] → [6]
```

### 2. Circular Queue

**Why?** Regular queues waste space!

**Problem with regular queue:**
```
Queue capacity: 5

After operations:
[_, _, 3, 4, 5]
 ↑           ↑
FRONT      REAR

Front elements deleted but space wasted!
Can't enqueue even though we have space!
```

**Circular queue solution:**
```
Wrap around when reaching end!

[3, 4, 5, _, _]
       ↑  ↑
     REAR FRONT

When REAR reaches end, wrap to beginning:
[6, 4, 5, _, _]
 ↑        ↑
REAR    FRONT

No wasted space!
```

**Visual:**
```
    [0]
  [4] [1]
  [3] [2]
  
Circular!
REAR can wrap to beginning
```

### 3. Double-Ended Queue (Deque)

**Pronounced:** "deck"

**Operations:** Can add/remove from BOTH ends!

```
FRONT                    REAR
  ↓                        ↓
[1] ← → [2] ← → [3] ← → [4]

Can:
- Add to front
- Add to rear
- Remove from front
- Remove from rear
```

**When to use:**
- Need flexibility
- Sliding window problems
- Can be used as both stack and queue!

### 4. Priority Queue

**Not FIFO!** Elements have priority.

```
Regular Queue:
Enqueue: A, B, C
Dequeue: A (first in)

Priority Queue:
Add: A(priority 2), B(priority 1), C(priority 3)
Remove: B (highest priority 1)
Remove: A (priority 2)
Remove: C (priority 3)
```

**Implementation:** Usually uses heap (we'll cover this later)

---

## Queue Operations & Implementation

### Core Operations

| Operation | Description | Time Complexity |
|-----------|-------------|-----------------|
| `enqueue(x)` | Add to rear | O(1) |
| `dequeue()` | Remove from front | O(1) |
| `front()` / `peek()` | View front element | O(1) |
| `isEmpty()` | Check if empty | O(1) |
| `size()` | Get number of elements | O(1) |

### Implementation Using Array

```python
class Queue:
    def __init__(self):
        """Initialize empty queue"""
        self.items = []
    
    def enqueue(self, item):
        """Add item to rear"""
        self.items.append(item)  # O(1)
    
    def dequeue(self):
        """Remove and return front item"""
        if not self.isEmpty():
            return self.items.pop(0)  # O(n) - shift all elements!
        return None
    
    def front(self):
        """View front item without removing"""
        if not self.isEmpty():
            return self.items[0]  # O(1)
        return None
    
    def isEmpty(self):
        """Check if queue is empty"""
        return len(self.items) == 0  # O(1)
    
    def size(self):
        """Get number of items"""
        return len(self.items)  # O(1)
```

**Problem with array implementation:**
```
Dequeue is O(n)!

[1, 2, 3, 4, 5]
 ↑
Remove 1

Must shift all elements:
[2, 3, 4, 5]
 ↑  ↑  ↑  ↑
All shifted left!

n-1 shifts = O(n)
```

### Better Implementation: Using Collections.deque

Python's `deque` (double-ended queue) is perfect for queues!

```python
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque()
    
    def enqueue(self, item):
        """Add to rear - O(1)"""
        self.items.append(item)
    
    def dequeue(self):
        """Remove from front - O(1)"""
        if not self.isEmpty():
            return self.items.popleft()  # O(1) - optimized!
        return None
    
    def front(self):
        """View front - O(1)"""
        if not self.isEmpty():
            return self.items[0]
        return None
    
    def isEmpty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

**Why deque is better:**
```
Implemented as doubly-linked list internally

[1] ← → [2] ← → [3] ← → [4]
 ↑                       ↑
HEAD                    TAIL

Remove from head: O(1) - just update pointer!
Add to tail: O(1) - just update pointer!

No shifting needed!
```

### Using Python's deque

```python
from collections import deque

# Create queue
queue = deque()

# Enqueue (add to rear)
queue.append(1)
queue.append(2)
queue.append(3)
print(queue)  # deque([1, 2, 3])

# Dequeue (remove from front)
front = queue.popleft()
print(front)  # 1
print(queue)  # deque([2, 3])

# Peek at front
print(queue[0])  # 2

# Check if empty
print(len(queue) == 0)  # False

# Get size
print(len(queue))  # 2
```

### Deque Operations Summary

```python
from collections import deque

dq = deque()

# Add to rear (like regular queue)
dq.append(1)        # [1]
dq.append(2)        # [1, 2]

# Remove from front (like regular queue)
dq.popleft()        # Returns 1, queue: [2]

# Add to front (extra capability!)
dq.appendleft(0)    # [0, 2]

# Remove from rear (extra capability!)
dq.pop()            # Returns 2, queue: [0]

# Peek
dq[0]               # Front element
dq[-1]              # Rear element

# Size
len(dq)             # Number of elements
```

---

## Problems

### Problem 1: Implement Stack using Queues

**[LeetCode 225 - Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)**

#### Problem Statement

Implement a last-in-first-out (LIFO) stack using only queue(s). The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:
- `void push(int x)` Pushes element x to the top of the stack
- `int pop()` Removes the element on the top of the stack and returns it
- `int top()` Returns the element on the top of the stack
- `boolean empty()` Returns true if the stack is empty, false otherwise

**Notes:**
- You must use only standard operations of a queue
- Only push to back, peek/pop from front, size, and is empty operations are valid

**Example:**
```python
myStack = MyStack()
myStack.push(1)
myStack.push(2)
myStack.top()    # Returns 2
myStack.pop()    # Returns 2
myStack.empty()  # Returns false
```

#### Understanding the Problem

**The challenge:** Queues are FIFO, but we need LIFO behavior!

```
Queue (FIFO):
Add: 1, 2, 3
Remove: 1 (first in)

Stack (LIFO):
Add: 1, 2, 3
Remove: 3 (last in)

How to get stack behavior using only queue operations?
```

#### Approach 1: Using Two Queues (Naive)

**Idea:** Use one queue for storage, another for reordering.

```python
class MyStack:
    def __init__(self):
        self.q1 = deque()  # Main queue
        self.q2 = deque()  # Helper queue
    
    def push(self, x: int) -> None:
        """O(n) - Need to reorder"""
        # Put new element in q2
        self.q2.append(x)
        
        # Move all from q1 to q2
        while self.q1:
            self.q2.append(self.q1.popleft())
        
        # Swap q1 and q2
        self.q1, self.q2 = self.q2, self.q1
    
    def pop(self) -> int:
        """O(1)"""
        return self.q1.popleft()
    
    def top(self) -> int:
        """O(1)"""
        return self.q1[0]
    
    def empty(self) -> bool:
        """O(1)"""
        return len(self.q1) == 0
```

**Visual walkthrough:**

```
push(1):
q1: []
q2: []

Step 1: Add to q2
q2: [1]

Step 2: Move q1 to q2 (nothing to move)
q2: [1]

Step 3: Swap
q1: [1]
q2: []


push(2):
q1: [1]
q2: []

Step 1: Add to q2
q2: [2]

Step 2: Move q1 to q2
q2: [2, 1]

Step 3: Swap
q1: [2, 1]
q2: []

Now when we pop from front of q1, we get 2 (last pushed)!


push(3):
q1: [2, 1]
q2: []

Step 1: Add to q2
q2: [3]

Step 2: Move q1 to q2
q2: [3, 2, 1]

Step 3: Swap
q1: [3, 2, 1]
q2: []

pop() → 3 (last in, first out) ✓
```

**Why this works:**
- New element always goes to front of q1
- When we dequeue from front, we get most recent element
- Achieves LIFO behavior!

**Problem:**
- Push is O(n) - expensive!
- Can we do better?

#### Approach 2: Using One Queue (Optimal)

**Key insight:** Rotate the queue after each push!

**Idea:**
```
After pushing new element:
Rotate all elements before it to the back

Example:
Queue: [1, 2]
Push 3: [1, 2, 3]

Rotate:
Step 1: Move 1 to back: [2, 3, 1]
Step 2: Move 2 to back: [3, 1, 2]

Now 3 is at front! Can pop it for LIFO behavior.
```

```python
from collections import deque

class MyStack:
    def __init__(self):
        self.q = deque()
    
    def push(self, x: int) -> None:
        """O(n) - Rotate queue"""
        # Add element to rear
        self.q.append(x)
        
        # Rotate all elements before it
        # (len - 1 rotations to bring new element to front)
        for _ in range(len(self.q) - 1):
            self.q.append(self.q.popleft())
    
    def pop(self) -> int:
        """O(1)"""
        return self.q.popleft()
    
    def top(self) -> int:
        """O(1)"""
        return self.q[0]
    
    def empty(self) -> bool:
        """O(1)"""
        return len(self.q) == 0
```

#### Detailed Walkthrough

```
Initial:
q = []


Operation: push(1)

Step 1: Append 1
q = [1]

Step 2: Rotate (len - 1 = 0 times)
No rotation needed
q = [1]


Operation: push(2)

Step 1: Append 2
q = [1, 2]

Step 2: Rotate (len - 1 = 1 time)

Iteration 1:
  Remove 1 from front: [2]
  Add 1 to back: [2, 1]

q = [2, 1]


Operation: push(3)

Step 1: Append 3
q = [2, 1, 3]

Step 2: Rotate (len - 1 = 2 times)

Iteration 1:
  Remove 2 from front: [1, 3]
  Add 2 to back: [1, 3, 2]

Iteration 2:
  Remove 1 from front: [3, 2]
  Add 1 to back: [3, 2, 1]

q = [3, 2, 1]


Operation: top()
Returns q[0] = 3 ✓


Operation: pop()
Remove from front: 3 ✓
q = [2, 1]


Operation: push(4)

Step 1: Append 4
q = [2, 1, 4]

Step 2: Rotate (len - 1 = 2 times)

Iteration 1:
  Remove 2 from front: [1, 4]
  Add 2 to back: [1, 4, 2]

Iteration 2:
  Remove 1 from front: [4, 2]
  Add 1 to back: [4, 2, 1]

q = [4, 2, 1]


Operation: pop()
Returns 4 ✓
q = [2, 1]
```

#### Why the Rotation Works

**Mathematical proof:**

```
After adding element at position n:
Queue: [e1, e2, ..., e(n-1), e_new]

Need e_new at front for LIFO.

Rotation count: n - 1

After rotation 1: [e2, ..., e(n-1), e_new, e1]
After rotation 2: [e3, ..., e(n-1), e_new, e1, e2]
...
After rotation (n-1): [e_new, e1, e2, ..., e(n-1)]

e_new is now at front!
```

**Visual:**
```
Original queue after push:
[Old elements ...] [New element]
                         ↑
                   Want this at front

After rotations:
[New element] [Old elements ...]
      ↑
  Now at front!
```

#### Common Mistakes

**Mistake 1: Wrong rotation count**
```python
# WRONG
for _ in range(len(self.q)):  # Too many rotations!
    self.q.append(self.q.popleft())

# This rotates back to original position!

# RIGHT
for _ in range(len(self.q) - 1):  # Correct!
```

**Mistake 2: Rotating before appending**
```python
# WRONG
for _ in range(len(self.q)):  # Rotates without new element
    ...
self.q.append(x)

# RIGHT
self.q.append(x)  # Append first
for _ in range(len(self.q) - 1):  # Then rotate
    ...
```

**Mistake 3: Using pop() instead of popleft()**
```python
# WRONG
def pop(self):
    return self.q.pop()  # Removes from rear!

# RIGHT
def pop(self):
    return self.q.popleft()  # Removes from front
```

#### Python Queue Module (Alternative)

```python
from queue import Queue

class MyStack:
    def __init__(self):
        # Queue module provides thread-safe queue
        self.q = Queue()
    
    def push(self, x: int) -> None:
        self.q.put(x)  # Add to queue
        
        # Rotate
        for _ in range(self.q.qsize() - 1):
            self.q.put(self.q.get())
    
    def pop(self) -> int:
        return self.q.get()
    
    def top(self) -> int:
        # Queue module doesn't have peek
        # Need to pop and push back
        top_element = self.q.get()
        
        # Rebuild queue
        temp = []
        while not self.q.empty():
            temp.append(self.q.get())
        
        self.q.put(top_element)
        for item in temp:
            self.q.put(item)
        
        return top_element
    
    def empty(self) -> bool:
        return self.q.empty()
```

**Note:** `collections.deque` is usually preferred because:
- More features (can peek without removing)
- Better performance
- More Pythonic

#### Time & Space Complexity

**One Queue Approach:**

**Time Complexity:**
- push(): O(n) - need to rotate n-1 elements
- pop(): O(1) - just dequeue from front
- top(): O(1) - just peek at front
- empty(): O(1) - check length

**Space Complexity:** O(n)
- Store n elements in queue

**Trade-off:**
```
Could make pop() O(n) and push() O(1) instead:
- push(): Just enqueue - O(1)
- pop(): Move n-1 elements, then dequeue - O(n)

Depends on which operation is more frequent in your use case!
```

---

### Problem 2: Time Needed to Buy Tickets

**[LeetCode 2073 - Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)**

#### Problem Statement

There are `n` people in a line queuing to buy tickets, where the 0th person is at the front and the (n-1)th person is at the back.

You are given a 0-indexed integer array `tickets` of length `n` where `tickets[i]` is the number of tickets that the ith person wants to buy.

Each person takes exactly 1 second to buy a ticket. A person can only buy 1 ticket at a time and has to go back to the end of the line to buy more tickets. If a person does not have any tickets left to buy, they leave the line.

Return the time it takes for the person at position `k` to finish buying tickets.

**Example 1:**
```
Input: tickets = [2,3,2], k = 2
Output: 6

Explanation:
Time = 0: [2,3,2] (person 0 buys, 1 sec)
Time = 1: [3,2,1] (person 1 buys, 1 sec) 
Time = 2: [2,1,3] (person 2 buys, 1 sec)
Time = 3: [1,3,2] (person 0 buys, 1 sec)
Time = 4: [3,2,0] (person 1 buys, 1 sec)
Time = 5: [2,0,3] (person 2 buys, 1 sec)
Time = 6: [0,3,2] (person 2 done!)
```

**Example 2:**
```
Input: tickets = [5,1,1,1], k = 0
Output: 8

Explanation:
Time 0-3: Everyone buys 1 ticket each
Time 4-7: Person 0 buys 4 more tickets
Time 8: Person 0 finishes
```

#### Understanding the Problem

**What's happening:**
```
Queue of people, each wants certain number of tickets
Each person:
1. Buys 1 ticket (1 second)
2. Goes to back of queue
3. Repeats until they have all their tickets

We want to know: When does person at position k finish?
```

**Visual:**
```
tickets = [2,3,2], k = 2

Queue: P0(2) P1(3) P2(2)
                    ↑
                  Target

Round 1:
P0 buys: P1(3) P2(2) P0(1) (time = 1)
P1 buys: P2(2) P0(1) P1(2) (time = 2)
P2 buys: P0(1) P1(2) P2(1) (time = 3) ← First ticket!

Round 2:
P0 buys: P1(2) P2(1) P0(0) → P0 leaves (time = 4)
P1 buys: P2(1) P1(1) (time = 5)
P2 buys: P1(1) P2(0) (time = 6) ← Done! ✓

Answer: 6
```

#### Naive Approach: Simulation

**Idea:** Actually simulate the queue!

```python
def timeRequiredToBuy(tickets: list[int], k: int) -> int:
    time = 0
    
    while tickets[k] > 0:
        for i in range(len(tickets)):
            if tickets[i] > 0:
                tickets[i] -= 1
                time += 1
                
                # Check if person k is done
                if i == k and tickets[k] == 0:
                    return time
    
    return time
```

**Visual trace:**
```
tickets = [2,3,2], k = 2

Initial: [2,3,2], time = 0

i=0: [1,3,2], time = 1
i=1: [1,2,2], time = 2
i=2: [1,2,1], time = 3 (k bought ticket)

i=0: [0,2,1], time = 4 (person 0 done, skip in future)
i=1: [0,1,1], time = 5
i=2: [0,1,0], time = 6 (k == 0, return!) ✓
```

**Problems:**
- Time: O(n × max(tickets))
  - Worst case: tickets = [100,100,...,100]
  - 100 rounds × n people = 100n operations!
- Can we do better without simulation?

#### Optimal Approach: Mathematical Logic

**Key insight:** We don't need to simulate! We can calculate directly!

**Think about it:**
```
Person k wants tickets[k] tickets

Who affects person k's time?

1. People BEFORE k:
   - They buy tickets before k in each round
   - Affect k's waiting time

2. People AFTER k:
   - They buy tickets after k in each round
   - Less impact on k
```

**Detailed reasoning:**

```
tickets = [2, 3, 5, 1], k = 2 (wants 5 tickets)
           ↑  ↑  ↑  ↑
          P0 P1 P2 P3
           
Person k wants 5 tickets.

PEOPLE BEFORE k (P0, P1):

P0 wants 2 tickets:
- 2 < 5, so P0 finishes before k
- P0 takes all 2 seconds before finishing
- Add 2 to time

P1 wants 3 tickets:
- 3 < 5, so P1 finishes before k
- P1 takes all 3 seconds
- Add 3 to time

PEOPLE AFTER k (P3):

P3 wants 1 ticket:
- In each round, k buys before P3
- P3 can buy tickets UNTIL k is done
- But after k finishes, we don't care about P3
- P3 can buy min(1, 5-1) = min(1, 4) = 1 ticket
  (5-1 because by the time P3 gets turn, k already bought one)
- Add 1 to time

PERSON k:
- Wants 5 tickets
- Add 5 to time

Total: 2 + 3 + 1 + 5 = 11 seconds
```

**The pattern:**

```
For each person i:

If i < k (before k):
    if tickets[i] < tickets[k]:
        add tickets[i] to time  (they finish before k)
    else:
        add tickets[k] to time  (they buy tickets[k] times before k finishes)

If i == k:
    add tickets[k] to time  (k buys all their tickets)

If i > k (after k):
    if tickets[i] < tickets[k]:
        add tickets[i] to time  (they finish before k)
    else:
        add tickets[k] - 1 to time  (k finishes before they get all tickets)
```

**Why tickets[k] - 1 for people after k?**

```
Example: tickets = [5, 3], k = 0

Round 1: P0 buys (time 1), P1 buys (time 2)
Round 2: P0 buys (time 3), P1 buys (time 4)
Round 3: P0 buys (time 5), P1 buys (time 6)
Round 4: P0 buys (time 7), P1 buys (time 8)
Round 5: P0 buys (time 9) ← DONE!

P1 bought 4 tickets before P0 finished
P1 could want 100 tickets, but P0 finished after round 5
P1 only got to buy min(3, 5-1) = 3 tickets ✗ WRONG!

Actually: P1 bought 4 times = tickets[k] - 1 + 1... let me recalculate

Actually the formula is:
- People after k buy tickets[k] - 1 times
  (because k finishes on their tickets[k]th purchase,
   people after k have had tickets[k] - 1 chances to buy)

Let's verify:
tickets = [5, 3], k = 0

P0 (k=0) wants 5 tickets:
  time = 5

P1 (after k) wants 3 tickets:
  P1 can buy min(3, 5-1) = min(3, 4) = 3
  time = 3

Total: 5 + 3 = 8 ✗ Let me trace...

Round 1: P0(1), P1(1) - times: 1, 2
Round 2: P0(1), P1(1) - times: 3, 4  
Round 3: P0(1), P1(1) - times: 5, 6
Round 4: P0(1) - times: 7 (P1 can't buy, only 3 tickets)
Round 5: P0(1) - times: 8, P0 done!

So P1 bought 3 tickets before P0 finished.
tickets[k] = 5, so P1 bought min(tickets[1], tickets[k] - 1)
= min(3, 4) = 3 ✓

Wait, but time is 8, not 5 + 3 = 8. Let me reconsider...

Oh! The -1 is because by the time person after k gets their turn in the final round,
k has already bought their ticket and is done!
```

Let me recalculate properly:

```
tickets = [2,3,2], k = 2

Person 0 (before k):
  wants 2, k wants 2
  takes min(2, 2) = 2 seconds

Person 1 (before k):
  wants 3, k wants 2
  takes min(3, 2) = 2 seconds

Person 2 (k):
  wants 2
  takes 2 seconds

Total: 2 + 2 + 2 = 6 ✓

Matches example!
```

```
tickets = [5,1,1,1], k = 0

Person 0 (k):
  wants 5
  takes 5 seconds

Person 1 (after k):
  wants 1, k wants 5
  takes min(1, 5-1) = min(1, 4) = 1 second

Person 2 (after k):
  wants 1, k wants 5
  takes min(1, 5-1) = 1 second

Person 3 (after k):
  wants 1, k wants 5
  takes min(1, 5-1) = 1 second

Total: 5 + 1 + 1 + 1 = 8 ✓ Matches example!
```

#### Solution

```python
def timeRequiredToBuy(tickets: list[int], k: int) -> int:
    time = 0
    
    for i in range(len(tickets)):
        if i <= k:
            # People before k (and k itself)
            # They contribute min(their tickets, k's tickets)
            time += min(tickets[i], tickets[k])
        else:
            # People after k
            # They contribute min(their tickets, k's tickets - 1)
            # (because k finishes before they get another turn)
            time += min(tickets[i], tickets[k] - 1)
    
    return time
```

#### Detailed Walkthrough

```
tickets = [2,3,2], k = 2

i = 0 (before k):
  tickets[0] = 2, tickets[k] = 2
  i <= k, so: time += min(2, 2) = 2
  time = 2

i = 1 (before k):
  tickets[1] = 3, tickets[k] = 2
  i <= k, so: time += min(3, 2) = 2
  time = 4

i = 2 (k itself):
  tickets[2] = 2, tickets[k] = 2
  i <= k, so: time += min(2, 2) = 2
  time = 6

Return 6 ✓
```

```
tickets = [5,1,1,1], k = 0

i = 0 (k itself):
  tickets[0] = 5, tickets[k] = 5
  i <= k, so: time += min(5, 5) = 5
  time = 5

i = 1 (after k):
  tickets[1] = 1, tickets[k] = 5
  i > k, so: time += min(1, 5-1) = min(1, 4) = 1
  time = 6

i = 2 (after k):
  tickets[2] = 1, tickets[k] = 5
  i > k, so: time += min(1, 5-1) = 1
  time = 7

i = 3 (after k):
  tickets[3] = 1, tickets[k] = 5
  i > k, so: time += min(1, 5-1) = 1
  time = 8

Return 8 ✓
```

#### Why This Works - Detailed Explanation

**Consider person i in relation to person k:**

**Case 1: i < k (person before k in line)**

```
Every round:
1. Person i gets a turn
2. Then person k gets a turn

If person i wants fewer tickets than k:
  - Person i finishes first
  - Contributes tickets[i] seconds

If person i wants more tickets than k:
  - Person i is still buying when k finishes
  - Only contributes tickets[k] seconds
  - (Can only buy as many times as k buys)

So: min(tickets[i], tickets[k])
```

**Case 2: i == k (the person we're tracking)**

```
Person k buys tickets[k] tickets
Takes tickets[k] seconds
Simple!
```

**Case 3: i > k (person after k in line)**

```
Every round:
1. Person k gets a turn
2. Then person i gets a turn

When k finishes:
  - It's on k's turn, before person i's turn
  - Person i has had tickets[k] - 1 opportunities
  
If person i wants fewer tickets:
  - They finish before k
  - Contributes tickets[i] seconds

If person i wants more tickets:
  - K finishes first
  - Person i only bought tickets[k] - 1 times
  - Contributes tickets[k] - 1 seconds

So: min(tickets[i], tickets[k] - 1)
```

**Visual proof:**

```
tickets = [X, Y, Z], k = 1

Rounds:
R1: P0(1), P1(1), P2(1)  - P2 bought 1 time
R2: P0(1), P1(1), P2(1)  - P2 bought 2 times
...
RY: P0(1), P1(1), DONE!  - P2 bought Y-1 times

P2 (after k) bought tickets[k] - 1 times!
```

#### Common Mistakes

**Mistake 1: Not handling people after k correctly**
```python
# WRONG
for i in range(len(tickets)):
    time += min(tickets[i], tickets[k])

# This doesn't account for people after k
# having one less opportunity

# RIGHT
if i <= k:
    time += min(tickets[i], tickets[k])
else:
    time += min(tickets[i], tickets[k] - 1)
```

**Mistake 2: Using simulation when not needed**
```python
# SLOW - O(n × max(tickets))
while tickets[k] > 0:
    for i in range(len(tickets)):
        if tickets[i] > 0:
            tickets[i] -= 1
            time += 1

# FAST - O(n)
for i in range(len(tickets)):
    if i <= k:
        time += min(tickets[i], tickets[k])
    else:
        time += min(tickets[i], tickets[k] - 1)
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Single pass through array
- Constant time operations per element

**Space Complexity: O(1)**
- Only one variable (time)
- No extra data structures

**Comparison:**
```
Simulation:  O(n × m) time where m = max(tickets)
Mathematical: O(n) time ✓ Much better!
```

### Problem 3: Reverse First K Elements of Queue

**[LeetCode-style problem - Reverse First K Elements of Queue](https://www.geeksforgeeks.org/reversing-first-k-elements-queue/)**

#### Problem Statement

Given an integer `k` and a queue of integers, write an algorithm to reverse the order of the first `k` elements of the queue, leaving the remaining elements in the same relative order.

Only the following standard operations are allowed on queue:
- `enqueue(x)`: Add item x to rear of queue
- `dequeue()`: Remove an item from front of queue
- `size()`: Returns number of elements in queue
- `isEmpty()`: Returns true if queue is empty

**Example 1:**
```
Input: queue = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]

Explanation:
Original: 1 → 2 → 3 → 4 → 5
Reverse first 3: 3 → 2 → 1 → 4 → 5
```

**Example 2:**
```
Input: queue = [10,20,30,40,50,60], k = 4
Output: [40,30,20,10,50,60]
```

**Example 3:**
```
Input: queue = [5,10,15,20], k = 4
Output: [20,15,10,5]
```

#### Understanding the Problem

**What we're doing:**
```
Queue: [1, 2, 3, 4, 5]
        ↑_______↑
     Reverse these (k=3)

Result: [3, 2, 1, 4, 5]
         ↑_______↑  ↑___↑
        Reversed  Unchanged
```

**Key constraints:**
- Can only use queue operations
- Can't access elements in the middle
- Must maintain order of remaining elements

#### Why This Problem is Important

**Shows difference between Queue and Stack:**
```
Queue (FIFO): [1,2,3,4,5]
Remove: 1, 2, 3, 4, 5 (same order)

Stack (LIFO): [1,2,3,4,5]
Remove: 5, 4, 3, 2, 1 (reversed!)

To reverse queue elements, use stack!
```

**Real-world application:**
- Undo recent actions (keep older actions in order)
- Process recent logs first (keep historical logs in order)
- Priority handling of recent requests

#### Naive Approach (WRONG)

**Attempt 1: Try to reverse in-place**

```python
# Can we swap elements?
queue[0], queue[k-1] = queue[k-1], queue[0]

# NO! Can't access by index in a queue!
# Can only dequeue from front or enqueue to rear
```

**Attempt 2: Use array, then convert back**

```python
def reverseFirstK(queue, k):
    # Copy to array
    arr = []
    while not queue.isEmpty():
        arr.append(queue.dequeue())
    
    # Reverse first k
    arr[:k] = arr[:k][::-1]
    
    # Put back in queue
    for item in arr:
        queue.enqueue(item)
```

**Problem:** This violates "only use queue operations" constraint!

#### Correct Approach: Use Stack as Helper

**Key insight:** Stack reverses order naturally!

**Algorithm:**
1. Dequeue first k elements and push to stack
2. Pop from stack and enqueue back to queue (reversed!)
3. Move remaining (n-k) elements from front to back

**Why this works:**
```
Queue is FIFO: Remove 1, 2, 3 → Get [1, 2, 3]
Stack is LIFO: Remove 3, 2, 1 → Get [3, 2, 1]

Stack naturally reverses order!
```

#### Visual Step-by-Step

**Example: queue = [1,2,3,4,5], k = 3**

```
INITIAL STATE:
Queue: [1, 2, 3, 4, 5]
Stack: []


PHASE 1: Dequeue first k elements, push to stack

Iteration 1:
Dequeue 1 from queue
Push 1 to stack
Queue: [2, 3, 4, 5]
Stack: [1]

Iteration 2:
Dequeue 2 from queue
Push 2 to stack
Queue: [3, 4, 5]
Stack: [1, 2]

Iteration 3:
Dequeue 3 from queue
Push 3 to stack
Queue: [4, 5]
Stack: [1, 2, 3]
         ↑
       TOP


PHASE 2: Pop from stack, enqueue to queue

Iteration 1:
Pop 3 from stack (LIFO!)
Enqueue 3 to queue
Queue: [4, 5, 3]
Stack: [1, 2]

Iteration 2:
Pop 2 from stack
Enqueue 2 to queue
Queue: [4, 5, 3, 2]
Stack: [1]

Iteration 3:
Pop 1 from stack
Enqueue 1 to queue
Queue: [4, 5, 3, 2, 1]
Stack: []


PHASE 3: Move remaining elements to back
(n - k = 5 - 3 = 2 elements to move)

Current state:
Queue: [4, 5, 3, 2, 1]
        ↑_↑  ↑_______↑
      Wrong  Reversed
      position

Need to move 4, 5 to back:

Iteration 1:
Dequeue 4
Enqueue 4 to back
Queue: [5, 3, 2, 1, 4]

Iteration 2:
Dequeue 5
Enqueue 5 to back
Queue: [3, 2, 1, 4, 5]
        ↑_______↑  ↑_↑
        Reversed  Correct
                 position

FINAL: [3, 2, 1, 4, 5] ✓
```

#### Solution Using Deque

```python
from collections import deque

def reverseFirstK(queue: deque, k: int) -> deque:
    """
    Reverse first k elements of queue
    
    Args:
        queue: deque representing the queue
        k: number of elements to reverse
    
    Returns:
        Modified queue with first k elements reversed
    """
    if k <= 0 or k > len(queue):
        return queue
    
    # Stack to hold first k elements
    stack = []
    
    # Phase 1: Dequeue first k elements and push to stack
    for _ in range(k):
        stack.append(queue.popleft())
    
    # Phase 2: Pop from stack and enqueue (reverses order)
    while stack:
        queue.append(stack.pop())
    
    # Phase 3: Move remaining (n-k) elements from front to back
    for _ in range(len(queue) - k):
        queue.append(queue.popleft())
    
    return queue
```

#### Solution Using Queue Module

```python
from queue import Queue

def reverseFirstK(q: Queue, k: int) -> Queue:
    """Using Python's Queue module"""
    if k <= 0 or k > q.qsize():
        return q
    
    stack = []
    
    # Phase 1: Move first k to stack
    for _ in range(k):
        stack.append(q.get())
    
    # Phase 2: Move from stack to queue (reversed)
    while stack:
        q.put(stack.pop())
    
    # Phase 3: Rotate remaining elements
    for _ in range(q.qsize() - k):
        q.put(q.get())
    
    return q
```

#### Detailed Trace - Complex Example

**Example: queue = [10,20,30,40,50,60], k = 4**

```
INITIAL:
Queue: [10, 20, 30, 40, 50, 60]
Stack: []


PHASE 1: Dequeue 4 elements to stack

After loop:
Queue: [50, 60]
Stack: [10, 20, 30, 40]
                    ↑
                  TOP


PHASE 2: Pop from stack to queue

Iteration 1: Pop 40
Queue: [50, 60, 40]
Stack: [10, 20, 30]

Iteration 2: Pop 30
Queue: [50, 60, 40, 30]
Stack: [10, 20]

Iteration 3: Pop 20
Queue: [50, 60, 40, 30, 20]
Stack: [10]

Iteration 4: Pop 10
Queue: [50, 60, 40, 30, 20, 10]
Stack: []


PHASE 3: Move (n-k = 6-4 = 2) elements

Iteration 1:
Dequeue 50, Enqueue 50
Queue: [60, 40, 30, 20, 10, 50]

Iteration 2:
Dequeue 60, Enqueue 60
Queue: [40, 30, 20, 10, 50, 60]
        ↑_______________↑  ↑____↑
          Reversed       Original
                          order

FINAL: [40, 30, 20, 10, 50, 60] ✓
```

#### Why Phase 3 is Necessary

**Without Phase 3:**
```
After Phase 2:
Queue: [50, 60, 40, 30, 20, 10]

But we want:
Queue: [40, 30, 20, 10, 50, 60]

The reversed elements are at the BACK!
We need them at the FRONT!

Phase 3 rotates the queue:
- Move non-reversed elements to back
- Reversed elements end up at front
```

**Mathematical explanation:**
```
Original queue: [a₁, a₂, ..., aₖ, aₖ₊₁, ..., aₙ]
                 ↑___________↑  ↑____________↑
                 Reverse these  Keep in order

After Phase 2: [aₖ₊₁, ..., aₙ, aₖ, aₖ₋₁, ..., a₁]
                ↑____________↑  ↑________________↑
                Wrong position  Reversed (correct)

After Phase 3: [aₖ, aₖ₋₁, ..., a₁, aₖ₊₁, ..., aₙ]
                ↑________________↑  ↑____________↑
                Reversed (front)    Original (back)

Rotation count: n - k elements
```

#### Common Mistakes

**Mistake 1: Forgetting Phase 3**
```python
# WRONG
stack = []
for _ in range(k):
    stack.append(queue.popleft())

while stack:
    queue.append(stack.pop())

# Returns [50, 60, 40, 30, 20, 10]
# But should be [40, 30, 20, 10, 50, 60]

# RIGHT
# ... Phase 1 and 2 ...
for _ in range(len(queue) - k):
    queue.append(queue.popleft())
```

**Mistake 2: Wrong rotation count in Phase 3**
```python
# WRONG
for _ in range(k):  # Wrong count!
    queue.append(queue.popleft())

# Rotates k elements, but should rotate (n-k)

# RIGHT
for _ in range(len(queue) - k):
    queue.append(queue.popleft())
```

**Mistake 3: Not handling edge cases**
```python
# WRONG
def reverseFirstK(queue, k):
    stack = []
    for _ in range(k):  # Errors if k > len(queue)!
        stack.append(queue.popleft())

# RIGHT
def reverseFirstK(queue, k):
    if k <= 0 or k > len(queue):
        return queue  # Handle invalid k
    ...
```

#### Time & Space Complexity

**Time Complexity: O(n)**
- Phase 1: O(k) - dequeue k elements
- Phase 2: O(k) - enqueue k elements
- Phase 3: O(n-k) - rotate remaining elements
- Total: O(k + k + n-k) = O(n + k) = O(n)

**Space Complexity: O(k)**
- Stack holds k elements
- Queue already exists (not counting input)

---

### Problem 4: Number of Recent Calls

**[LeetCode 933 - Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)**

#### Problem Statement

You have a `RecentCounter` class which counts the number of recent requests within a certain time frame.

Implement the `RecentCounter` class:
- `RecentCounter()` Initializes the counter with zero recent requests
- `int ping(int t)` Adds a new request at time `t`, where `t` represents some time in milliseconds, and returns the number of requests that have happened in the past 3000 milliseconds (including the new request). Specifically, return the number of requests that have happened in the inclusive range `[t - 3000, t]`.

It is guaranteed that every call to `ping` uses a strictly larger value of `t` than the previous call.

**Example:**
```python
Input:
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [3001], [3002]]

Output:
[null, 1, 2, 3, 3]

Explanation:
RecentCounter recentCounter = new RecentCounter()
recentCounter.ping(1)     # requests = [1], range is [-2999,1], return 1
recentCounter.ping(100)   # requests = [1,100], range is [-2900,100], return 2
recentCounter.ping(3001)  # requests = [1,100,3001], range is [1,3001], return 3
recentCounter.ping(3002)  # requests = [1,100,3001,3002], range is [2,3002], return 3
```

#### Understanding the Problem

**What's a "recent" request?**
```
Current time: t
Recent window: [t - 3000, t]

Example: t = 5000
Window: [2000, 5000]

Requests in window: Count
Requests outside window: Don't count
```

**Visual:**
```
Time: ----1----100-----------3001-3002----→

ping(1):
Window: [-2999, 1]
Requests: [1]
Count: 1 ✓

ping(100):
Window: [-2900, 100]
Requests: [1, 100]
Count: 2 ✓

ping(3001):
Window: [1, 3001]
Requests: [1, 100, 3001]
Count: 3 ✓

ping(3002):
Window: [2, 3002]
Requests: [1, 100, 3001, 3002]
         ↑ Outside window!
Valid: [100, 3001, 3002]
Count: 3 ✓
```

#### Naive Approach (Inefficient)

**Attempt 1: Store all requests, count every time**

```python
class RecentCounter:
    def __init__(self):
        self.requests = []
    
    def ping(self, t: int) -> int:
        self.requests.append(t)
        
        # Count requests in window [t-3000, t]
        count = 0
        for request in self.requests:
            if request >= t - 3000 and request <= t:
                count += 1
        
        return count
```

**Problems:**
```
After 10,000 pings:
- requests = [1, 100, 200, ..., 10000]
- Each ping checks all 10,000 requests!
- Time: O(n) per ping
- Total: O(n²) for n pings

Too slow!
```

#### Key Insight: Old Requests Don't Matter!

**Critical observation:**
```
Time moves forward (strictly increasing)
Old requests eventually fall out of window

Example:
ping(1): Window [−2999, 1]
ping(5000): Window [2000, 5000]

Request at t=1 is now outside window!
We'll NEVER need it again!

Can we remove it?
```

**What data structure allows removing old data?**
- Stack? No - need to remove old, not recent
- Queue? YES! - FIFO perfect for this!

```
Queue maintains chronological order
Remove old requests from front
Add new requests to rear

Perfect!
```

#### Optimal Approach: Queue

**Algorithm:**
1. Add new request to queue
2. Remove all requests older than (t - 3000) from front
3. Return queue size

```python
from collections import deque

class RecentCounter:
    def __init__(self):
        """Initialize empty queue"""
        self.requests = deque()
    
    def ping(self, t: int) -> int:
        """Add request and count recent requests"""
        # Add new request to rear
        self.requests.append(t)
        
        # Remove old requests from front
        while self.requests and self.requests[0] < t - 3000:
            self.requests.popleft()
        
        # Count remaining requests
        return len(self.requests)
```

#### Detailed Walkthrough

```python
recentCounter = RecentCounter()


ping(1):
  Step 1: Add 1 to queue
  requests = [1]
  
  Step 2: Remove old requests
  Front = 1
  Is 1 < (1 - 3000 = -2999)? No
  Don't remove
  
  Step 3: Return size
  requests = [1]
  Return 1 ✓


ping(100):
  Step 1: Add 100
  requests = [1, 100]
  
  Step 2: Remove old
  Front = 1
  Is 1 < (100 - 3000 = -2900)? No
  Don't remove
  
  Step 3: Return size
  requests = [1, 100]
  Return 2 ✓


ping(3001):
  Step 1: Add 3001
  requests = [1, 100, 3001]
  
  Step 2: Remove old
  Front = 1
  Is 1 < (3001 - 3000 = 1)? No (1 is not < 1)
  Don't remove
  
  Step 3: Return size
  requests = [1, 100, 3001]
  Return 3 ✓


ping(3002):
  Step 1: Add 3002
  requests = [1, 100, 3001, 3002]
  
  Step 2: Remove old
  
  Check front = 1:
  Is 1 < (3002 - 3000 = 2)? Yes!
  Remove 1
  requests = [100, 3001, 3002]
  
  Check front = 100:
  Is 100 < 2? No
  Stop removing
  
  Step 3: Return size
  requests = [100, 3001, 3002]
  Return 3 ✓
```

#### Why Queue is Perfect

**Queue properties match our needs:**

```
1. Chronological order:
   - Older requests at front
   - Newer requests at rear
   - Matches time progression

2. Remove from front:
   - Old requests leave first
   - FIFO behavior

3. Add to rear:
   - New requests join back
   - Natural progression

Perfect match!
```

**Visual:**
```
Time: 1 → 100 → 3001 → 3002 →

Queue at different times:

t=1:    [1]
         ↑
       Front

t=100:  [1, 100]
         ↑    ↑
       Front Rear

t=3001: [1, 100, 3001]
         ↑           ↑
       Front       Rear

t=3002: [100, 3001, 3002]
         ↑               ↑
       1 removed       Rear
```

#### Edge Cases

**Edge Case 1: First request**
```python
ping(1)
requests = []
Add 1: [1]
Remove old: none to remove
Return 1 ✓
```

**Edge Case 2: All requests in window**
```python
ping(1)    → [1]
ping(100)  → [1, 100]
ping(200)  → [1, 100, 200]
ping(300)  → [1, 100, 200, 300]

All within 3000ms of each other
None removed
```

**Edge Case 3: Only last request in window**
```python
ping(1)     → [1]
ping(5001)  → [5001]  (1 removed, too old)

Window: [2001, 5001]
Only 5001 in window
```

#### Common Mistakes

**Mistake 1: Using list instead of deque**
```python
# SLOW
class RecentCounter:
    def __init__(self):
        self.requests = []  # list
    
    def ping(self, t):
        self.requests.append(t)
        
        while self.requests[0] < t - 3000:
            self.requests.pop(0)  # O(n) - shifts all elements!

# FAST
class RecentCounter:
    def __init__(self):
        self.requests = deque()  # deque
    
    def ping(self, t):
        self.requests.append(t)
        
        while self.requests[0] < t - 3000:
            self.requests.popleft()  # O(1) - optimized!
```

**Mistake 2: Wrong window boundary**
```python
# WRONG
while self.requests[0] <= t - 3000:  # Should be <, not <=
    self.requests.popleft()

# Example: t = 3001
# Window: [1, 3001]
# Request at t=1 should be included!
# But <= would remove it!

# RIGHT
while self.requests[0] < t - 3000:
    self.requests.popleft()
```

**Mistake 3: Not checking if queue is empty**
```python
# WRONG
while self.requests[0] < t - 3000:  # Error if requests is empty!
    self.requests.popleft()

# RIGHT
while self.requests and self.requests[0] < t - 3000:
    self.requests.popleft()
```

#### Time & Space Complexity

**Time Complexity:**
- `__init__()`: O(1)
- `ping(t)`: 
  - Append: O(1)
  - Remove old: O(k) where k = number of old requests
  - But amortized O(1)!
  
**Why amortized O(1)?**
```
Each request is:
- Added once: O(1)
- Removed at most once: O(1)

Over n pings:
- n additions: O(n)
- n removals total: O(n)
- Average per ping: O(1)

Amortized O(1) per ping!
```

**Space Complexity: O(w)**
- w = window size (3000 ms)
- At most O(w) requests in queue at any time
- Old requests removed
- Bounded by window size!

---

### Problem 5: Design Circular Queue

**[LeetCode 622 - Design Circular Queue](https://leetcode.com/problems/design-circular-queue/)**

#### Problem Statement

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle, and the last position is connected back to the first position to make a circle.

Implement the `MyCircularQueue` class:
- `MyCircularQueue(k)` Initializes the object with the size of the queue to be `k`
- `int Front()` Gets the front item from the queue. If the queue is empty, return `-1`
- `int Rear()` Gets the last item from the queue. If the queue is empty, return `-1`
- `boolean enQueue(int value)` Inserts an element into the circular queue. Return `true` if the operation is successful
- `boolean deQueue()` Deletes an element from the circular queue. Return `true` if the operation is successful
- `boolean isEmpty()` Checks whether the circular queue is empty or not
- `boolean isFull()` Checks whether the circular queue is full or not

**Example:**
```python
MyCircularQueue circularQueue = new MyCircularQueue(3)
circularQueue.enQueue(1)  # return True
circularQueue.enQueue(2)  # return True
circularQueue.enQueue(3)  # return True
circularQueue.enQueue(4)  # return False (queue is full)
circularQueue.Rear()      # return 3
circularQueue.isFull()    # return True
circularQueue.deQueue()   # return True
circularQueue.enQueue(4)  # return True
circularQueue.Rear()      # return 4
```

#### Understanding Circular Queue

**Problem with regular queue using array:**

```
Fixed-size array: [_, _, _, _, _]
Capacity: 5

Operations:
enqueue(1): [1, _, _, _, _]
enqueue(2): [1, 2, _, _, _]
enqueue(3): [1, 2, 3, _, _]
dequeue():  [_, 2, 3, _, _]  ← Space wasted!
dequeue():  [_, _, 3, _, _]  ← More waste!
enqueue(4): [_, _, 3, 4, _]
enqueue(5): [_, _, 3, 4, 5]

Can't enqueue even though we have space at beginning!
Front spaces are wasted!
```

**Circular queue solution:**

```
Wrap around when reaching end!

Array: [_, _, _, _, _]
Indices: 0  1  2  3  4

enqueue(1): [1, _, _, _, _]  front=0, rear=0
enqueue(2): [1, 2, _, _, _]  front=0, rear=1
enqueue(3): [1, 2, 3, _, _]  front=0, rear=2
dequeue():  [_, 2, 3, _, _]  front=1, rear=2
dequeue():  [_, _, 3, _, _]  front=2, rear=2
enqueue(4): [_, _, 3, 4, _]  front=2, rear=3
enqueue(5): [_, _, 3, 4, 5]  front=2, rear=4

enqueue(6): rear reaches end (4)
            Wrap to beginning!
            [6, _, 3, 4, 5]  front=2, rear=0
            
No wasted space!
```

**Visual representation:**

```
Regular Queue (Linear):
[_][_][3][4][5]
 ↑        
Wasted space

Circular Queue:
      [2]
    [1] [3]
    [0] [4]
      ↑
  Connects back!
  
When rear reaches index 4, next is index 0!
```

#### Key Concepts

**1. Front and Rear Pointers:**
```
front: Points to first element
rear: Points to last element

Initially:
front = 0
rear = -1 (or 0, depends on implementation)
```

**2. Wrap-Around Logic:**
```
Next position after index i:
next = (i + 1) % capacity

Examples with capacity = 5:
(0 + 1) % 5 = 1
(1 + 1) % 5 = 2
(4 + 1) % 5 = 0  ← Wraps around!
```

**3. Full vs Empty:**
```
Empty: When count = 0
Full: When count = capacity

Why not check if (rear + 1) % capacity == front?
Because that's ambiguous!

Example: capacity = 3
[1, 2, 3]  front=0, rear=2
Is this full or empty?

Better: Keep count!
```

#### Solution - Using Count Variable

```python
class MyCircularQueue:
    def __init__(self, k: int):
        """
        Initialize with size k
        """
        self.queue = [0] * k  # Fixed-size array
        self.capacity = k
        self.front = 0
        self.rear = -1
        self.count = 0  # Track number of elements
    
    def enQueue(self, value: int) -> bool:
        """
        Insert element at rear
        """
        if self.isFull():
            return False
        
        # Move rear to next position (with wrap-around)
        self.rear = (self.rear + 1) % self.capacity
        self.queue[self.rear] = value
        self.count += 1
        return True
    
    def deQueue(self) -> bool:
        """
        Delete element from front
        """
        if self.isEmpty():
            return False
        
        # Move front to next position (with wrap-around)
        self.front = (self.front + 1) % self.capacity
        self.count -= 1
        return True
    
    def Front(self) -> int:
        """
        Get front element
        """
        if self.isEmpty():
            return -1
        return self.queue[self.front]
    
    def Rear(self) -> int:
        """
        Get rear element
        """
        if self.isEmpty():
            return -1
        return self.queue[self.rear]
    
    def isEmpty(self) -> bool:
        """
        Check if queue is empty
        """
        return self.count == 0
    
    def isFull(self) -> bool:
        """
        Check if queue is full
        """
        return self.count == self.capacity
```

#### Detailed Walkthrough

```python
circularQueue = MyCircularQueue(3)
# queue = [0, 0, 0]
# capacity = 3
# front = 0, rear = -1, count = 0


circularQueue.enQueue(1)

Check: isFull()? No (count=0, capacity=3)

rear = (rear + 1) % capacity
rear = (-1 + 1) % 3 = 0

queue[0] = 1
count = 1

State:
queue = [1, 0, 0]
         ↑
      front=0, rear=0
count = 1

Return True ✓


circularQueue.enQueue(2)

Check: isFull()? No (count=1)

rear = (0 + 1) % 3 = 1
queue[1] = 2
count = 2

State:
queue = [1, 2, 0]
         ↑  ↑
      front rear
count = 2

Return True ✓


circularQueue.enQueue(3)

Check: isFull()? No (count=2)

rear = (1 + 1) % 3 = 2
queue[2] = 3
count = 3

State:
queue = [1, 2, 3]
         ↑     ↑
      front  rear
count = 3

Return True ✓


circularQueue.enQueue(4)

Check: isFull()? Yes! (count=3, capacity=3)
Return False ✗


circularQueue.Rear()

Check: isEmpty()? No
Return queue[rear] = queue[2] = 3 ✓


circularQueue.isFull()

Return count == capacity
Return 3 == 3 = True ✓


circularQueue.deQueue()

Check: isEmpty()? No

front = (0 + 1) % 3 = 1
count = 2

State:
queue = [1, 2, 3]
            ↑  ↑
         front rear
count = 2
(Note: 1 still in array but not accessible)

Return True ✓


circularQueue.enQueue(4)

Check: isFull()? No (count=2)

rear = (2 + 1) % 3 = 0  ← Wrap around!
queue[0] = 4
count = 3

State:
queue = [4, 2, 3]
         ↑  ↑
       rear front
count = 3

Return True ✓


circularQueue.Rear()

Check: isEmpty()? No
Return queue[rear] = queue[0] = 4 ✓
```

#### Visual Representation

```
Capacity = 3

Initial:
Indices: [0, 1, 2]
Values:  [_, _, _]
front = 0, rear = -1, count = 0


After enQueue(1), enQueue(2), enQueue(3):
Values:  [1, 2, 3]
          ↑     ↑
       front  rear
count = 3


After deQueue():
Values:  [1, 2, 3]
             ↑  ↑
          front rear
count = 2
(1 is inaccessible)


After enQueue(4):
Values:  [4, 2, 3]
          ↑  ↑
        rear front
count = 3

Notice: rear wrapped to index 0!

Circular view:
     [0]
    /   \
  [2]   [1]
  
Index 2 → 0 → 1 (circular!)
```

#### Alternative Implementation: Without Count

**Using space sacrifice method:**

```python
class MyCircularQueue:
    def __init__(self, k: int):
        """
        Sacrifice one space to differentiate full from empty
        """
        self.queue = [0] * (k + 1)  # One extra space
        self.capacity = k + 1
        self.front = 0
        self.rear = 0
    
    def enQueue(self, value: int) -> bool:
        if self.isFull():
            return False
        
        self.queue[self.rear] = value
        self.rear = (self.rear + 1) % self.capacity
        return True
    
    def deQueue(self) -> bool:
        if self.isEmpty():
            return False
        
        self.front = (self.front + 1) % self.capacity
        return True
    
    def Front(self) -> int:
        if self.isEmpty():
            return -1
        return self.queue[self.front]
    
    def Rear(self) -> int:
        if self.isEmpty():
            return -1
        # Rear points to next empty slot
        # So actual rear is (rear - 1 + capacity) % capacity
        return self.queue[(self.rear - 1 + self.capacity) % self.capacity]
    
    def isEmpty(self) -> bool:
        return self.front == self.rear
    
    def isFull(self) -> bool:
        return (self.rear + 1) % self.capacity == self.front
```

**How it works:**
```
Capacity = 3 (user wants 3 slots)
Actual array size = 4 (sacrifice 1 space)

Empty: front == rear
queue = [_, _, _, _]
         ↑
      front, rear

Full: (rear + 1) % capacity == front
queue = [1, 2, 3, _]
         ↑        ↑
       front    rear
One space always unused!

This way, full and empty have different conditions!
```

#### Common Mistakes

**Mistake 1: Not using modulo for wrap-around**
```python
# WRONG
self.rear = self.rear + 1
if self.rear >= self.capacity:
    self.rear = 0

# More code, harder to read

# RIGHT
self.rear = (self.rear + 1) % self.capacity
# Clean, handles wrap-around automatically
```

**Mistake 2: Confusing full and empty conditions**
```python
# WRONG: Without count variable
def isEmpty(self):
    return self.front == self.rear

def isFull(self):
    return self.front == self.rear  # Same condition!

# Can't differentiate!

# RIGHT: Use count
def isEmpty(self):
    return self.count == 0

def isFull(self):
    return self.count == self.capacity
```

**Mistake 3: Wrong Rear() implementation without count**
```python
# WRONG
def Rear(self):
    return self.queue[self.rear]  # rear points to NEXT empty slot!

# RIGHT
def Rear(self):
    # Actual rear is one position before self.rear
    return self.queue[(self.rear - 1 + self.capacity) % self.capacity]
```

#### Time & Space Complexity

**All Operations: O(1)**
- enQueue(): O(1)
- deQueue(): O(1)
- Front(): O(1)
- Rear(): O(1)
- isEmpty(): O(1)
- isFull(): O(1)

**Space Complexity: O(k)**
- Fixed array of size k
- Constant extra space for pointers

**Benefits:**
```
Compared to regular queue:
1. No wasted space
2. All operations O(1)
3. Predictable memory usage

Perfect for:
- Fixed-size buffers
- Resource pools
- Scheduling systems
```

---

## Queue Concepts Summary

### Key Takeaways

**1. FIFO Principle**
```
First In, First Out
Opposite of stack (LIFO)
```

**2. When to Use Queue**
- Process in order of arrival
- BFS traversal
- Task scheduling
- Buffer management

**3. When to Use Stack**
- Need to reverse order
- DFS traversal
- Undo functionality
- Expression evaluation

**4. Queue + Stack = Power!**
```
Combine both to:
- Reverse queue elements
- Implement one using the other
- Complex data manipulations
```

### Python Queue Options

```python
# 1. collections.deque (BEST for most cases)
from collections import deque
q = deque()
q.append(1)      # O(1)
q.popleft()      # O(1)

# 2. queue.Queue (Thread-safe, heavier)
from queue import Queue
q = Queue()
q.put(1)         # O(1)
q.get()          # O(1)

# 3. list (AVOID for queues)
q = []
q.append(1)      # O(1)
q.pop(0)         # O(n) - slow!
```

### Common Patterns

**Pattern 1: Sliding Window**
```python
from collections import deque

def slidingWindow(arr, k):
    queue = deque()
    result = []
    
    for i in range(len(arr)):
        # Remove elements outside window
        while queue and queue[0] < i - k + 1:
            queue.popleft()
        
        # Add current element
        queue.append(i)
        
        # Process window
        if i >= k - 1:
            result.append(arr[queue[0]])
    
    return result
```

**Pattern 2: Level Order Traversal (BFS)**
```python
from collections import deque

def levelOrder(root):
    if not root:
        return []
    
    queue = deque([root])
    result = []
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

**Pattern 3: Moving Average**
```python
from collections import deque

class MovingAverage:
    def __init__(self, size):
        self.queue = deque()
        self.size = size
        self.sum = 0
    
    def next(self, val):
        self.queue.append(val)
        self.sum += val
        
        if len(self.queue) > self.size:
            self.sum -= self.queue.popleft()
        
        return self.sum / len(self.queue)
```
