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
