# 📚 Stack, Queue & Heap — Complete Course
**Language:** Java | **Level:** SDE Interview Ready
**Style:** Concept → ASCII Diagram → Why it works → Java Code → Problems

---

# 📌 Contents

**Phase 1 — Foundations**
- 1.1 What is a Stack?
- 1.2 What is a Queue?
- 1.3 What is a Heap / Priority Queue?
- 1.4 Java Syntax Reference — Full Sheet
- 1.5 Common Tricks & Traps

**Phase 2 — Stack Patterns**
- 2.1 Valid Parentheses (LC 20)
- 2.2 Min Stack (LC 155)
- 2.3 Evaluate Reverse Polish Notation (LC 150)
- 2.4 Daily Temperatures — Monotonic Stack (LC 739)
- 2.5 Next Greater Element I & II (LC 496, 503)
- 2.6 Largest Rectangle in Histogram (LC 84)
- 2.7 Maximal Rectangle (LC 85)
- 2.8 Decode String (LC 394)
- 2.9 Basic Calculator II (LC 227)
- 2.10 Remove K Digits (LC 402)
- 2.11 Asteroid Collision (LC 735)
- 2.12 Simplify Path (LC 71)
- 2.13 Online Stock Span (LC 901)

**Phase 3 — Queue Patterns**
- 3.1 Implement Queue using Stacks (LC 232)
- 3.2 Implement Stack using Queues (LC 225)
- 3.3 Moving Average from Data Stream (LC 346)
- 3.4 Design Hit Counter (LC 362)
- 3.5 BFS with Queue (LC 994 — Rotting Oranges)

**Phase 4 — Monotonic Stack (Deep Dive)**
- 4.1 The Monotonic Stack Pattern
- 4.2 Sum of Subarray Minimums (LC 907)
- 4.3 Maximum Width Ramp (LC 962)
- 4.4 Shortest Subarray with Sum ≥ K (LC 862)
- 4.5 Jump Game VI (LC 1696)

**Phase 5 — Heap / Priority Queue Patterns**
- 5.1 Kth Largest Element (LC 215)
- 5.2 Kth Largest in a Stream (LC 703)
- 5.3 Last Stone Weight (LC 1046)
- 5.4 K Closest Points to Origin (LC 973)
- 5.5 Top K Frequent Elements (LC 347)
- 5.6 Find K Pairs with Smallest Sums (LC 373)
- 5.7 Kth Smallest in Sorted Matrix (LC 378)
- 5.8 Task Scheduler (LC 621)
- 5.9 Reorganize String (LC 767)
- 5.10 Find Median from Data Stream (LC 295)
- 5.11 Merge K Sorted Lists (LC 23)
- 5.12 Smallest Range Covering K Lists (LC 632)

**Phase 6 — Design Problems**
- 6.1 LRU Cache (LC 146)
- 6.2 LFU Cache (LC 460)

**Master Reference**
- Pattern Decision Table
- All LC Problems by Category

---

# 🟢 Phase 1 — Foundations

---

## 1.1 What is a Stack?

A Stack is a **Last In, First Out (LIFO)** data structure.

```
Think of a stack of plates:
  Push (add to top):  plate goes ON TOP
  Pop  (remove top):  remove from TOP
  Peek (see top):     look at TOP without removing

      ┌───────┐
      │  30   │  ← top (last in, first out)
      ├───────┤
      │  20   │
      ├───────┤
      │  10   │  ← bottom (first in, last out)
      └───────┘

push(40):           pop():
      ┌───────┐           ┌───────┐
      │  40   │  ← new    │  30   │  ← becomes top
      ├───────┤           ├───────┤
      │  30   │           │  20   │
      ├───────┤           ├───────┤
      │  20   │           │  10   │
      ├───────┤           └───────┘
      │  10   │
      └───────┘
```

### When to use a Stack?

```
✅ Matching brackets / parentheses
✅ Undo / redo operations
✅ DFS (iterative)
✅ Expression evaluation
✅ Monotonic Stack (next greater/smaller element)
✅ Backtracking state management
✅ Parsing nested structures (e.g., decode string)
```

---

## 1.2 What is a Queue?

A Queue is a **First In, First Out (FIFO)** data structure.

```
Think of people standing in a line:
  Offer (enqueue): person joins at BACK
  Poll  (dequeue): person leaves from FRONT
  Peek:            see who's at FRONT

 FRONT                          BACK
   ↓                              ↓
  [10] → [20] → [30] → [40] → [50]

offer(60):  [10]→[20]→[30]→[40]→[50]→[60]
poll():     [20]→[30]→[40]→[50]→[60]  (removed 10)
```

### When to use a Queue?

```
✅ BFS (level-by-level graph traversal)
✅ Process tasks in order (scheduling)
✅ Sliding window with ordering (Deque)
✅ Multi-source BFS (start from multiple nodes)
✅ Data stream / real-time processing (Moving Average, Hit Counter)
```

### Deque — Double Ended Queue

A Deque lets you add/remove from BOTH ends. Used for:
- Sliding window maximum/minimum (monotonic deque)
- BFS with priority at both ends

```
       add front   add back
           ↓           ↓
Front → [10][20][30][40] ← Back
           ↑           ↑
       remove front  remove back
```

---

## 1.3 What is a Heap / Priority Queue?

A Heap is a **complete binary tree** that satisfies the heap property.

```
Min-Heap: parent is ALWAYS ≤ its children
          → smallest element always at the ROOT (top)

          1          ← always min
        /   \
       3     2
      / \   / \
     6   5 4   8

Max-Heap: parent is ALWAYS ≥ its children
          → largest element always at the ROOT

          8          ← always max
        /   \
       7     5
      / \   /
     6   4 3
```

### Operations

```
Operation     MinHeap          Time
─────────────────────────────────────
peek          see minimum      O(1)
offer/push    add element      O(log N)
poll/pop      remove minimum   O(log N)
```

### When to use a Heap?

```
✅ Find Kth largest / smallest element
✅ Stream median (two heaps)
✅ Merge K sorted arrays/lists
✅ Top K frequent / closest elements
✅ Task scheduling / CPU simulation
✅ Dijkstra shortest path (MinHeap)
✅ Always need the current min or max quickly
```

---

## 1.4 Java Syntax Reference — Full Sheet

```java
// ════════════════════════════════════════════════
// STACK
// ════════════════════════════════════════════════

// ✅ Preferred: ArrayDeque as Stack (faster than Stack class)
Deque<Integer> stack = new ArrayDeque<>();
stack.push(x);         // add to top        O(1)
stack.pop();           // remove top        O(1)
stack.peek();          // see top           O(1)
stack.isEmpty();       // check empty       O(1)
stack.size();

// ⚠️ AVOID: Stack<Integer> stack = new Stack<>();
// Stack class is synchronized (slower) and extends Vector (bad design)

// Store indices (very common in monotonic stack):
Deque<Integer> stack = new ArrayDeque<>(); // stores INDICES not values
stack.push(i);
int idx = stack.peek();
int val = nums[stack.peek()]; // access value via stored index

// ════════════════════════════════════════════════
// QUEUE
// ════════════════════════════════════════════════

// Standard Queue
Queue<Integer> q = new LinkedList<>();
q.offer(x);            // add to back       O(1)
q.poll();              // remove from front O(1)
q.peek();              // see front         O(1)
q.isEmpty();
q.size();

// Deque as Queue (double-ended)
Deque<Integer> dq = new ArrayDeque<>();
dq.offerLast(x);       // add to back
dq.offerFirst(x);      // add to front
dq.pollFirst();        // remove from front
dq.pollLast();         // remove from back
dq.peekFirst();        // see front
dq.peekLast();         // see back

// Queue for BFS with arrays (grid problems):
Queue<int[]> q = new LinkedList<>();
q.offer(new int[]{row, col});
q.offer(new int[]{row, col, dist}); // with distance

// ════════════════════════════════════════════════
// HEAP / PRIORITY QUEUE
// ════════════════════════════════════════════════

// Min-Heap (default in Java)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(x);      // add               O(log N)
minHeap.poll();        // remove min        O(log N)
minHeap.peek();        // see min           O(1)

// Max-Heap
PriorityQueue<Integer> maxHeap =
    new PriorityQueue<>(Collections.reverseOrder());

// Min-Heap with custom comparator (sort by second element)
PriorityQueue<int[]> pq =
    new PriorityQueue<>((a, b) -> a[1] - b[1]);

// Max-Heap with custom comparator
PriorityQueue<int[]> pq =
    new PriorityQueue<>((a, b) -> b[1] - a[1]);

// COMMON PATTERNS:
// Top K Largest → MinHeap of size K
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
for (int num : nums) {
    minHeap.offer(num);
    if (minHeap.size() > k) minHeap.poll(); // evict smallest
}
// minHeap now contains K largest elements

// Top K Smallest → MaxHeap of size K
PriorityQueue<Integer> maxHeap =
    new PriorityQueue<>(Collections.reverseOrder());
for (int num : nums) {
    maxHeap.offer(num);
    if (maxHeap.size() > k) maxHeap.poll(); // evict largest
}

// Two-Heap for Median:
PriorityQueue<Integer> maxHeap =  // lower half
    new PriorityQueue<>(Collections.reverseOrder());
PriorityQueue<Integer> minHeap =  // upper half
    new PriorityQueue<>();
```

---

## 1.5 Common Tricks & Traps

### ⭐ Trick 1 — Store INDICES, not Values, in Monotonic Stack

Many problems require knowing the POSITION of elements, not just values. Always push indices — you can always recover the value as `nums[stack.peek()]`.

```java
Deque<Integer> stack = new ArrayDeque<>();
for (int i = 0; i < nums.length; i++) {
    while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
        int idx = stack.pop();    // idx = position of smaller element
        int val = nums[idx];      // value at that position
        // distance to current element: i - idx
    }
    stack.push(i);  // push INDEX
}
```

### ⭐ Trick 2 — Two Heaps for Median

Split the data into two halves: maxHeap holds the lower half, minHeap holds the upper half. The median is at their tops.

```
Lower half (maxHeap):  [1, 2, 3]  → top = 3
Upper half (minHeap):  [4, 5, 6]  → top = 4

If even count: median = (maxHeap.peek() + minHeap.peek()) / 2.0
If odd count:  median = the larger heap's top
```

### ⭐ Trick 3 — Monotonic Deque for Sliding Window

For the sliding window MAXIMUM problem, maintain a deque where the FRONT always holds the index of the window's maximum. Remove elements from back if they're smaller than the new element (they'll never be the max). Remove front if it's outside the window.

```
nums = [1,3,-1,-3,5,3,6,7], k=3

Step by step, deque holds INDICES (values shown for clarity):
i=0: dq=[0(1)]
i=1: 3>1 → remove 0, dq=[1(3)]
i=2: -1<3, dq=[1(3),2(-1)]    window=[0..2], max=nums[1]=3
i=3: -3<-1, dq=[1(3),2(-1),3(-3)] window=[1..3], max=nums[1]=3
i=4: 5>all → remove all, dq=[4(5)] window=[2..4], max=nums[4]=5
...
```

### ⭐ Trick 4 — "Lazy" Deletion in Heaps

If you need to "delete" an arbitrary element from a heap (which isn't directly supported), use a HashSet or counter to mark elements as "invalid". When polling, skip those invalid entries.

```java
Set<Integer> deleted = new HashSet<>();

// "Delete" element x:
deleted.add(x);

// Safe poll (skip deleted):
while (!heap.isEmpty() && deleted.contains(heap.peek())) {
    deleted.remove(heap.poll());
}
int top = heap.poll();
```

### ⚠️ Trap — poll() on Empty Structure

```java
// This throws NoSuchElementException:
stack.pop(); // when stack is empty

// Always check first:
if (!stack.isEmpty()) stack.pop();
// OR use the safe versions:
stack.pollFirst(); // returns null if empty (Deque interface)
```

### ⚠️ Trap — Heap Comparator Integer Overflow

```java
// WRONG: can overflow for large negative numbers
(a, b) -> a - b

// SAFE: use Integer.compare
(a, b) -> Integer.compare(a, b)    // min-heap
(a, b) -> Integer.compare(b, a)    // max-heap
```

---

# 🔵 Phase 2 — Stack Patterns

---

## 2.1 Valid Parentheses (LC 20) ⭐ Most Asked

### Concept

Every closing bracket must match the most recent UNMATCHED opening bracket.

```
Input:  "({[]})"

Stack trace:
  '(' → push '('     stack: ['(']
  '{' → push '{'     stack: ['(', '{']
  '[' → push '['     stack: ['(', '{', '[']
  ']' → closing, check top='[' matches ']' ✅ pop   stack: ['(', '{']
  '}' → closing, check top='{' matches '}' ✅ pop   stack: ['(']
  ')' → closing, check top='(' matches ')' ✅ pop   stack: []
  
End: stack is EMPTY → Valid ✅

Input: "([)]"
  '(' → push, '['→push, ')' → top='[' ≠ ')' → INVALID ❌
```

```java
boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    Map<Character, Character> map = Map.of(
        ')', '(',
        '}', '{',
        ']', '['
    );

    for (char c : s.toCharArray()) {
        if (map.containsKey(c)) {               // closing bracket
            if (stack.isEmpty() || stack.peek() != map.get(c)) {
                return false;                   // no match
            }
            stack.pop();
        } else {
            stack.push(c);                      // opening bracket
        }
    }
    return stack.isEmpty();                     // all matched = empty
}
```

---

## 2.2 Min Stack (LC 155)

### Concept

Design a stack that supports `getMin()` in O(1). The trick: maintain a PARALLEL stack that always holds the current minimum at each stack state.

```
Operations:    push(5), push(3), push(7), push(2), pop(), getMin()

Main stack:  [5, 3, 7, 2]
Min  stack:  [5, 3, 3, 2]  ← each level's minimum

After pop():
Main stack:  [5, 3, 7]
Min  stack:  [5, 3, 3]     ← 3 is still the min! O(1)
```

```java
class MinStack {
    Deque<Integer> stack    = new ArrayDeque<>();
    Deque<Integer> minStack = new ArrayDeque<>();

    void push(int val) {
        stack.push(val);
        int curMin = minStack.isEmpty()
            ? val
            : Math.min(val, minStack.peek());
        minStack.push(curMin);           // track min AT THIS LEVEL
    }

    void pop() {
        stack.pop();
        minStack.pop();                  // always pop together
    }

    int top()    { return stack.peek(); }
    int getMin() { return minStack.peek(); } // O(1)!
}
```

---

## 2.3 Evaluate Reverse Polish Notation (LC 150)

### Concept

Evaluate expressions in postfix notation (operators come AFTER operands).

```
["2","1","+","3","*"]  →  ((2+1)*3) = 9

Stack trace:
  "2" → push 2    [2]
  "1" → push 1    [2, 1]
  "+" → pop 1,2, push 2+1=3    [3]
  "3" → push 3    [3, 3]
  "*" → pop 3,3, push 3*3=9    [9]

Result = 9 ✅
```

```java
int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();

    for (String token : tokens) {
        if ("+-*/".contains(token)) {
            int b = stack.pop();  // second operand (popped first)
            int a = stack.pop();  // first operand
            switch (token) {
                case "+": stack.push(a + b); break;
                case "-": stack.push(a - b); break;
                case "*": stack.push(a * b); break;
                case "/": stack.push(a / b); break;
            }
        } else {
            stack.push(Integer.parseInt(token));
        }
    }
    return stack.pop();
}
```

---

## 2.4 Daily Temperatures — Monotonic Stack (LC 739) ⭐

### Concept

For each day, find how many days until a WARMER temperature. This is the "Next Greater Element" pattern.

```
temps = [73,74,75,71,69,72,76,73]
answer = [1,  1,  4,  2,  1,  1,  0,  0]

Key insight: maintain a DECREASING monotonic stack (of indices).
When we find a temperature GREATER than the stack's top,
the top element has FOUND its answer: today's index - stack top index.

Trace:
i=0: stack=[], push 0       stack=[0(73)]
i=1: 74>73 → pop 0, ans[0]=1-0=1, push 1  stack=[1(74)]
i=2: 75>74 → pop 1, ans[1]=2-1=1, push 2  stack=[2(75)]
i=3: 71<75, push 3          stack=[2(75),3(71)]
i=4: 69<71, push 4          stack=[2(75),3(71),4(69)]
i=5: 72>69 → pop 4, ans[4]=5-4=1
     72>71 → pop 3, ans[3]=5-3=2
     72<75, push 5          stack=[2(75),5(72)]
i=6: 76>72 → pop 5, ans[5]=6-5=1
     76>75 → pop 2, ans[2]=6-2=4
     push 6                 stack=[6(76)]
i=7: 73<76, push 7          stack=[6(76),7(73)]
End: remaining in stack → ans stays 0
```

```java
int[] dailyTemperatures(int[] temperatures) {
    int n = temperatures.length;
    int[] answer = new int[n]; // default 0
    Deque<Integer> stack = new ArrayDeque<>(); // stores INDICES

    for (int i = 0; i < n; i++) {
        // While current temp is WARMER than stack top:
        while (!stack.isEmpty()
               && temperatures[i] > temperatures[stack.peek()]) {
            int idx = stack.pop();
            answer[idx] = i - idx; // days waited = distance
        }
        stack.push(i);
    }
    return answer;
}
```

---

## 2.5 Next Greater Element I & II (LC 496, 503)

### Next Greater Element I (LC 496)

```java
// nums1 is a subset of nums2, find next greater in nums2 for each nums1 element
int[] nextGreaterElement(int[] nums1, int[] nums2) {
    Map<Integer, Integer> map = new HashMap<>(); // val → next greater val
    Deque<Integer> stack = new ArrayDeque<>();

    for (int num : nums2) {
        while (!stack.isEmpty() && stack.peek() < num) {
            map.put(stack.pop(), num); // num is next greater for popped element
        }
        stack.push(num);
    }
    // remaining in stack have no next greater → map returns default (not set)

    int[] result = new int[nums1.length];
    for (int i = 0; i < nums1.length; i++) {
        result[i] = map.getOrDefault(nums1[i], -1);
    }
    return result;
}
```

### Next Greater Element II (LC 503) — Circular Array

```java
// Simulate circular array by looping TWICE
int[] nextGreaterElements(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    Arrays.fill(result, -1);
    Deque<Integer> stack = new ArrayDeque<>(); // stores indices

    for (int i = 0; i < 2 * n; i++) {  // loop twice for circular
        int idx = i % n;
        while (!stack.isEmpty()
               && nums[stack.peek()] < nums[idx]) {
            result[stack.pop()] = nums[idx];
        }
        if (i < n) stack.push(idx);     // only push in first pass
    }
    return result;
}
```

---

## 2.6 Largest Rectangle in Histogram (LC 84) ⭐ Hard but Frequent

### Concept

Find the largest rectangle that fits within the histogram bars.

```
heights = [2,1,5,6,2,3]

         ┌──┐
         │  ├──┐
   ┌──┐  │  │  │  ┌──┐
   │  ├──┤  │  ├──┤  │
   │  │  │  │  │  │  │
   2  1  5  6  2  3

Largest rectangle = 10 (using bars of height 5 and 6, width 2)

Key: Use a MONOTONIC INCREASING stack of indices.
When we find a bar SHORTER than the stack top, we can compute
the rectangle using the top as the HEIGHT — its width extends
LEFT to the next shorter bar and RIGHT to the current position.

Trick: append a 0 at the end to flush remaining elements from stack.
```

```java
int largestRectangleArea(int[] heights) {
    Deque<Integer> stack = new ArrayDeque<>();
    int maxArea = 0;
    int n = heights.length;

    for (int i = 0; i <= n; i++) {
        int h = (i == n) ? 0 : heights[i]; // sentinel 0 at end

        while (!stack.isEmpty() && heights[stack.peek()] > h) {
            int height = heights[stack.pop()];
            // Width: from current position to element now at stack top
            int width = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        stack.push(i);
    }
    return maxArea;
}
```

```
Trace with heights=[2,1,5,6,2,3]:
i=0: h=2, stack=[0]
i=1: h=1, 1<2 → pop 0 (h=2), width=1(no left boundary), area=2*1=2
     stack=[], push 1   stack=[1]
i=2: h=5, stack=[1,2]
i=3: h=6, stack=[1,2,3]
i=4: h=2, 2<6 → pop 3 (h=6), width=4-2-1=1, area=6*1=6
              2<5 → pop 2 (h=5), width=4-1-1=2, area=5*2=10 ← MAX
              2>1, push 4   stack=[1,4]
i=5: h=3, stack=[1,4,5]
i=6: h=0 (sentinel):
     pop 5 (h=3), width=6-4-1=1, area=3
     pop 4 (h=2), width=6-1-1=4, area=8
     pop 1 (h=1), width=6, area=6
Answer = 10 ✅
```

---

## 2.7 Maximal Rectangle (LC 85)

### Concept

Find the largest rectangle containing only 1s in a binary matrix. The trick: convert each row into a histogram and run LC 84 on each row.

```
Matrix:            Row histograms:
1 0 1 0 0          Row 0: [1,0,1,0,0]
1 0 1 1 1          Row 1: [2,0,2,1,1]
1 1 1 1 1          Row 2: [3,1,3,2,2]
1 0 0 1 0          Row 3: [4,0,0,3,0]

Apply largestRectangleArea on each row:
Row 0: 1, Row 1: 3, Row 2: 6 ← MAX, Row 3: 4
```

```java
int maximalRectangle(char[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[] heights = new int[n];
    int maxArea = 0;

    for (int i = 0; i < m; i++) {
        // Update histogram heights
        for (int j = 0; j < n; j++) {
            heights[j] = matrix[i][j] == '1' ? heights[j] + 1 : 0;
        }
        maxArea = Math.max(maxArea, largestRectangleArea(heights));
    }
    return maxArea;
}
// reuse largestRectangleArea from LC 84
```

---

## 2.8 Decode String (LC 394) ⭐

### Concept

`k[encoded_string]` means repeat `encoded_string` exactly `k` times. Handles nesting.

```
Input: "3[a2[c]]"
→ inner: "2[c]" = "cc"
→ outer: "3[acc]" = "accaccacc"

Use TWO stacks:
  countStack: saves multipliers
  strStack:   saves strings built before the current bracket
```

```java
String decodeString(String s) {
    Deque<Integer> countStack = new ArrayDeque<>();
    Deque<StringBuilder> strStack = new ArrayDeque<>();
    StringBuilder curr = new StringBuilder();
    int k = 0;

    for (char c : s.toCharArray()) {
        if (Character.isDigit(c)) {
            k = k * 10 + (c - '0');   // handles multi-digit: "12[a]"
        } else if (c == '[') {
            countStack.push(k);        // save current multiplier
            strStack.push(curr);       // save string built so far
            curr = new StringBuilder(); // start fresh inside brackets
            k = 0;
        } else if (c == ']') {
            int times = countStack.pop();
            StringBuilder prev = strStack.pop();
            for (int i = 0; i < times; i++) prev.append(curr);
            curr = prev;               // restore and extend
        } else {
            curr.append(c);
        }
    }
    return curr.toString();
}
```

---

## 2.9 Basic Calculator II (LC 227)

### Concept

Evaluate a string containing `+`, `-`, `*`, `/` with correct operator precedence (no parentheses).

```
"3+2*2" → 7  (not 10, * has higher precedence)

Strategy: use a stack to handle precedence.
  + and -: push the number (positive or negative) onto stack
  * and /: pop stack top, compute with current number, push result
  At end: sum all values in stack

"3+2*2":
  3, op='+' → push +3    stack=[3]
  2, op='*' → push +2    stack=[3,2]
  2, op=end → pop 2, compute 2*2=4, push 4   stack=[3,4]
  Sum = 3+4 = 7 ✅
```

```java
int calculate(String s) {
    Deque<Integer> stack = new ArrayDeque<>();
    int num = 0;
    char op = '+'; // start with '+' so first number is pushed positively

    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);

        if (Character.isDigit(c)) {
            num = num * 10 + (c - '0');  // build multi-digit number
        }

        // Process when we hit an operator or end of string
        if ((!Character.isDigit(c) && c != ' ') || i == s.length() - 1) {
            switch (op) {
                case '+': stack.push(num);         break;
                case '-': stack.push(-num);         break;
                case '*': stack.push(stack.pop() * num); break;
                case '/': stack.push(stack.pop() / num); break;
            }
            op = c;   // save current operator for the NEXT number
            num = 0;
        }
    }

    int result = 0;
    while (!stack.isEmpty()) result += stack.pop();
    return result;
}
```

---

## 2.10 Remove K Digits (LC 402)

### Concept

Remove k digits from a number string to get the SMALLEST possible number.

```
"1432219", k=3

Strategy: maintain a MONOTONICALLY INCREASING stack.
When we see a digit smaller than the top, pop the top (remove it, decrement k).

Stack trace:
  '1' → push    stack=[1]
  '4' → push    stack=[1,4]
  '3' → 3<4, pop '4' k=2, push 3   stack=[1,3]
  '2' → 2<3, pop '3' k=1, push 2   stack=[1,2]
  '2' → 2=2, push               stack=[1,2,2]
  '1' → 1<2, pop k=0, push 1   stack=[1,2,1]
            (k=0, stop popping)
  '9' → push    stack=[1,2,1,9]

Result = "1219"  ✅ (smallest after removing 3 digits)
```

```java
String removeKdigits(String num, int k) {
    Deque<Character> stack = new ArrayDeque<>();

    for (char c : num.toCharArray()) {
        // Remove larger digits from top while we still have removals
        while (k > 0 && !stack.isEmpty() && stack.peek() > c) {
            stack.pop();
            k--;
        }
        stack.push(c);
    }

    // If k > 0 still, remove from the end (largest remaining)
    while (k-- > 0) stack.pop();

    // Build result, skipping leading zeros
    StringBuilder sb = new StringBuilder();
    boolean leadingZero = true;
    for (char c : stack) {  // ArrayDeque iterates bottom-to-top naturally? No—
        // Actually need to collect in order, iterate from bottom
        sb.append(c);
    }
    // Fix: use a different approach — rebuild from the deque as a string
    String result = sb.toString().replaceAll("^0+", ""); // strip leading zeros
    return result.isEmpty() ? "0" : result;
}

// Cleaner version using LinkedList as deque (preserves order):
String removeKdigits(String num, int k) {
    LinkedList<Character> stack = new LinkedList<>();
    for (char c : num.toCharArray()) {
        while (k > 0 && !stack.isEmpty() && stack.peekLast() > c) {
            stack.pollLast();
            k--;
        }
        stack.addLast(c);
    }
    while (k-- > 0) stack.pollLast();

    StringBuilder sb = new StringBuilder();
    boolean leadZero = true;
    for (char c : stack) {
        if (leadZero && c == '0') continue;
        leadZero = false;
        sb.append(c);
    }
    return sb.length() == 0 ? "0" : sb.toString();
}
```

---

## 2.11 Asteroid Collision (LC 735)

### Concept

Positive asteroids move RIGHT, negative move LEFT. When they collide, the smaller one explodes. Equal ones both explode.

```
[5, 10, -5]  → [5, 10]
  -5 vs 10: 10 survives

[8, -8]  → []
  -8 vs 8: both explode

[10, 2, -5]  → [10]
  -5 vs 2: 2 explodes
  -5 vs 10: 10 survives
```

```java
int[] asteroidCollision(int[] asteroids) {
    Deque<Integer> stack = new ArrayDeque<>();

    for (int a : asteroids) {
        boolean alive = true;

        while (alive && a < 0 && !stack.isEmpty() && stack.peek() > 0) {
            if (stack.peek() < -a) {
                stack.pop();            // stack top is smaller, it explodes
            } else if (stack.peek() == -a) {
                stack.pop();            // equal: both explode
                alive = false;
            } else {
                alive = false;          // stack top is larger, current explodes
            }
        }

        if (alive) stack.push(a);
    }

    int[] result = new int[stack.size()];
    for (int i = result.length - 1; i >= 0; i--) {
        result[i] = stack.pop();
    }
    return result;
}
```

---

## 2.12 Simplify Path (LC 71)

```java
String simplifyPath(String path) {
    Deque<String> stack = new ArrayDeque<>();
    for (String part : path.split("/")) {
        if (part.equals("..")) {
            if (!stack.isEmpty()) stack.pop();
        } else if (!part.isEmpty() && !part.equals(".")) {
            stack.push(part);
        }
    }
    StringBuilder sb = new StringBuilder();
    for (String dir : stack) sb.insert(0, "/" + dir);
    return sb.length() == 0 ? "/" : sb.toString();
}
```

---

## 2.13 Online Stock Span (LC 901)

### Concept

For each day's price, find how many CONSECUTIVE days (including today) the price was ≤ today's price.

```
prices = [100, 80, 60, 70, 60, 75, 85]
spans  = [1,   1,  1,  2,  1,  4,  6]

Day 6 (price=85): spans back over 75,60,70,60,80 — all ≤ 85? No, 100 is not.
                  span = 1+4+1 = 6? Let's trace...
                  Stack holds (price, span) pairs.
```

```java
class StockSpanner {
    Deque<int[]> stack = new ArrayDeque<>(); // [price, span]

    int next(int price) {
        int span = 1;
        while (!stack.isEmpty() && stack.peek()[0] <= price) {
            span += stack.pop()[1]; // accumulate spans of dominated prices
        }
        stack.push(new int[]{price, span});
        return span;
    }
}
```

### BYTS Problems — Phase 2

| # | Problem | Pattern | Difficulty |
|---|---------|---------|------------|
| 20 | Valid Parentheses | Stack matching | Easy |
| 155 | Min Stack | Parallel min stack | Med |
| 150 | Evaluate Reverse Polish Notation | Stack evaluation | Med |
| 739 | Daily Temperatures | Monotonic decreasing | Med |
| 496 | Next Greater Element I | Monotonic + HashMap | Easy |
| 503 | Next Greater Element II | Circular monotonic | Med |
| 84 | Largest Rectangle in Histogram | Monotonic increasing | Hard |
| 85 | Maximal Rectangle | Histogram per row | Hard |
| 394 | Decode String | Two stacks | Med |
| 227 | Basic Calculator II | Stack + operator | Med |
| 224 | Basic Calculator | Stack + recursion | Hard |
| 402 | Remove K Digits | Monotonic increasing | Med |
| 735 | Asteroid Collision | Stack simulation | Med |
| 71 | Simplify Path | Stack path | Med |
| 901 | Online Stock Span | Monotonic + span | Med |
| 32 | Longest Valid Parentheses | Stack indices | Hard |

---

# 🟡 Phase 3 — Queue Patterns

---

## 3.1 Implement Queue using Stacks (LC 232)

### Concept

Use TWO stacks. Stack 1 = input, Stack 2 = output. Transfer from input to output when output is empty.

```
push(1), push(2), push(3), pop()

s1 (input): [1,2,3]     s2 (output): []
pop(): s2 empty → transfer ALL from s1 to s2
s1: []                  s2: [3,2,1] (reversed!)
pop from s2: 1 ✅ (correct FIFO order!)
```

```java
class MyQueue {
    Deque<Integer> s1 = new ArrayDeque<>(); // input stack
    Deque<Integer> s2 = new ArrayDeque<>(); // output stack

    void push(int x) {
        s1.push(x);
    }

    int pop() {
        transfer();
        return s2.pop();
    }

    int peek() {
        transfer();
        return s2.peek();
    }

    boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
    }

    private void transfer() {
        if (s2.isEmpty()) {
            while (!s1.isEmpty()) s2.push(s1.pop());
        }
    }
}
// Amortized O(1) per operation — each element transferred at most once!
```

---

## 3.2 Implement Stack using Queues (LC 225)

```java
class MyStack {
    Queue<Integer> q = new LinkedList<>();

    void push(int x) {
        q.offer(x);
        // Rotate: move all elements before x to after x
        for (int i = 0; i < q.size() - 1; i++) {
            q.offer(q.poll());
        }
        // Now x is at the FRONT = top of "stack"
    }

    int pop()  { return q.poll(); }
    int top()  { return q.peek(); }
    boolean empty() { return q.isEmpty(); }
}
```

---

## 3.3 Moving Average from Data Stream (LC 346)

### Concept

Keep a sliding window of size k using a queue. Remove oldest when window is full.

```java
class MovingAverage {
    Queue<Integer> window = new LinkedList<>();
    int size;
    double sum = 0;

    MovingAverage(int size) { this.size = size; }

    double next(int val) {
        if (window.size() == size) {
            sum -= window.poll(); // remove oldest
        }
        window.offer(val);
        sum += val;
        return sum / window.size();
    }
}
```

---

## 3.4 Design Hit Counter (LC 362)

```java
class HitCounter {
    Queue<Integer> hits = new LinkedList<>();

    void hit(int timestamp) {
        hits.offer(timestamp);
    }

    int getHits(int timestamp) {
        // Remove hits older than 300 seconds
        while (!hits.isEmpty() && timestamp - hits.peek() >= 300) {
            hits.poll();
        }
        return hits.size();
    }
}
```

---

## 3.5 BFS with Queue — Rotting Oranges (LC 994)

```java
int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> q = new LinkedList<>();
    int fresh = 0;

    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) q.offer(new int[]{i, j});
            if (grid[i][j] == 1) fresh++;
        }

    if (fresh == 0) return 0;

    int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    int minutes = 0;

    while (!q.isEmpty() && fresh > 0) {
        minutes++;
        int size = q.size();
        for (int k = 0; k < size; k++) {
            int[] curr = q.poll();
            for (int[] d : dirs) {
                int r = curr[0]+d[0], c = curr[1]+d[1];
                if (r>=0&&r<m&&c>=0&&c<n&&grid[r][c]==1) {
                    grid[r][c] = 2;
                    fresh--;
                    q.offer(new int[]{r, c});
                }
            }
        }
    }
    return fresh == 0 ? minutes : -1;
}
```

### BYTS Problems — Phase 3

| # | Problem | Pattern | Difficulty |
|---|---------|---------|------------|
| 232 | Implement Queue using Stacks | Two stacks | Easy |
| 225 | Implement Stack using Queues | One queue rotate | Easy |
| 346 | Moving Average from Data Stream | Queue window | Easy |
| 362 | Design Hit Counter | Queue + time window | Med |
| 994 | Rotting Oranges | Multi-source BFS | Med |
| 542 | 01 Matrix | Multi-source BFS | Med |
| 127 | Word Ladder | BFS shortest path | Hard |

---

# 🟠 Phase 4 — Monotonic Stack (Deep Dive)

---

## 4.1 The Monotonic Stack Pattern

### The Two Types

```
MONOTONIC DECREASING STACK:
  Elements are in DECREASING order from bottom to top.
  When new element is LARGER → pop elements (they found their "next greater").
  Used for: Next Greater Element, Daily Temperatures, Stock Span

  Visualization: popping happens when we see something BIGGER
  [...7, 5, 3] ← top=3
  new=4: 4>3 → pop 3 (4 is its next greater), 4>5? No → push 4
  [...7, 4]

MONOTONIC INCREASING STACK:
  Elements are in INCREASING order from bottom to top.
  When new element is SMALLER → pop elements (they found their "next smaller").
  Used for: Largest Rectangle, Remove K Digits, Trapping Rain Water

  Visualization: popping happens when we see something SMALLER
  [...2, 5, 7] ← top=7
  new=4: 4<7 → pop 7 (4 is its next smaller), 4<5 → pop 5 → push 4
  [...2, 4]
```

### Template

```java
// NEXT GREATER (decreasing stack)
int[] result = new int[n];
Arrays.fill(result, -1);
Deque<Integer> stack = new ArrayDeque<>(); // stores indices

for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
        result[stack.pop()] = nums[i]; // nums[i] is the answer for popped
    }
    stack.push(i);
}

// NEXT SMALLER (increasing stack)
int[] result = new int[n];
Arrays.fill(result, -1);
Deque<Integer> stack = new ArrayDeque<>();

for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && nums[stack.peek()] > nums[i]) {
        result[stack.pop()] = nums[i];
    }
    stack.push(i);
}
```

---

## 4.2 Sum of Subarray Minimums (LC 907)

### Concept

For every subarray, find its minimum. Return the sum of all these minimums.

```
arr = [3, 1, 2, 4]
Subarrays and their mins:
[3]=3, [1]=1, [2]=2, [4]=4
[3,1]=1, [1,2]=1, [2,4]=2
[3,1,2]=1, [1,2,4]=1
[3,1,2,4]=1
Sum = 3+1+2+4+1+1+2+1+1+1 = 17

For each element, count how many subarrays it is the MINIMUM of.
For arr[i]: left[i]=distance to previous smaller, right[i]=distance to next smaller
Contribution = arr[i] * left[i] * right[i]
```

```java
int sumSubarrayMins(int[] arr) {
    int n = arr.length;
    int MOD = 1_000_000_007;
    int[] left = new int[n];   // distance to previous smaller or equal
    int[] right = new int[n];  // distance to next smaller

    Deque<Integer> stack = new ArrayDeque<>();

    // Previous smaller (use ≤ to avoid double counting)
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && arr[stack.peek()] >= arr[i]) stack.pop();
        left[i] = stack.isEmpty() ? i + 1 : i - stack.peek();
        stack.push(i);
    }

    stack.clear();

    // Next smaller (use < strictly)
    for (int i = n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && arr[stack.peek()] > arr[i]) stack.pop();
        right[i] = stack.isEmpty() ? n - i : stack.peek() - i;
        stack.push(i);
    }

    long result = 0;
    for (int i = 0; i < n; i++) {
        result = (result + (long) arr[i] * left[i] * right[i]) % MOD;
    }
    return (int) result;
}
```

---

## 4.3 Maximum Width Ramp (LC 962)

### Concept

Find maximum `j - i` where `i < j` and `nums[i] <= nums[j]`.

```
Strategy:
1. Build a decreasing stack of indices from left (candidates for i)
2. Scan from RIGHT, for each j, pop stack while nums[stack.top] <= nums[j]
   → the ramp width is j - stack.top, update max, keep popping
```

```java
int maxWidthRamp(int[] nums) {
    int n = nums.length;
    Deque<Integer> stack = new ArrayDeque<>();

    // Step 1: Build decreasing stack (potential left endpoints)
    for (int i = 0; i < n; i++) {
        if (stack.isEmpty() || nums[stack.peek()] > nums[i]) {
            stack.push(i);
        }
    }

    // Step 2: Scan from right, find widest ramp
    int maxWidth = 0;
    for (int j = n - 1; j >= 0; j--) {
        while (!stack.isEmpty() && nums[stack.peek()] <= nums[j]) {
            maxWidth = Math.max(maxWidth, j - stack.pop());
        }
    }
    return maxWidth;
}
```

### BYTS Problems — Phase 4

| # | Problem | Pattern | Difficulty |
|---|---------|---------|------------|
| 739 | Daily Temperatures | Monotonic decreasing | Med |
| 496 | Next Greater Element I | Monotonic + HashMap | Easy |
| 503 | Next Greater Element II | Circular | Med |
| 84 | Largest Rectangle in Histogram | Monotonic increasing | Hard |
| 907 | Sum of Subarray Minimums | Left/Right contribution | Med |
| 962 | Maximum Width Ramp | Mono stack 2-pass | Med |
| 862 | Shortest Subarray with Sum ≥ K | Mono deque | Hard |
| 1696 | Jump Game VI | Mono deque DP | Med |

---

# 🔴 Phase 5 — Heap / Priority Queue Patterns

---

## 5.1 Kth Largest Element (LC 215) ⭐ Most Asked

### Concept

Find the Kth largest element. Use a MIN-heap of size K.
Why min-heap? It naturally evicts the SMALLEST of the K kept elements, leaving only the K LARGEST.

```
nums = [3,2,1,5,6,4], k=2

Process each number with a min-heap of max size k=2:
  3: heap=[3]
  2: heap=[2,3]       (size=2, ok)
  1: heap=[1,2,3]→[2,3] (size>2, evict min=1)
  5: heap=[2,3,5]→[3,5] (evict min=2)
  6: heap=[3,5,6]→[5,6] (evict min=3)
  4: heap=[4,5,6]→[5,6] (evict min=4)

Top of min-heap = 5 = 2nd largest ✅
```

```java
int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();

    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) minHeap.poll(); // evict smallest
    }

    return minHeap.peek(); // top = kth largest
}
```

---

## 5.2 Kth Largest in a Stream (LC 703)

```java
class KthLargest {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    int k;

    KthLargest(int k, int[] nums) {
        this.k = k;
        for (int n : nums) add(n);
    }

    int add(int val) {
        minHeap.offer(val);
        if (minHeap.size() > k) minHeap.poll();
        return minHeap.peek();
    }
}
```

---

## 5.3 Last Stone Weight (LC 1046)

```java
int lastStoneWeight(int[] stones) {
    PriorityQueue<Integer> maxHeap =
        new PriorityQueue<>(Collections.reverseOrder());

    for (int s : stones) maxHeap.offer(s);

    while (maxHeap.size() > 1) {
        int y = maxHeap.poll(); // heaviest
        int x = maxHeap.poll(); // second heaviest
        if (x != y) maxHeap.offer(y - x);
    }

    return maxHeap.isEmpty() ? 0 : maxHeap.peek();
}
```

---

## 5.4 K Closest Points to Origin (LC 973)

### Use MAX-heap of size K

```java
int[][] kClosest(int[][] points, int k) {
    // Max-heap by distance, keep K smallest distances
    PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
        (a, b) -> (b[0]*b[0]+b[1]*b[1]) - (a[0]*a[0]+a[1]*a[1])
    );

    for (int[] p : points) {
        maxHeap.offer(p);
        if (maxHeap.size() > k) maxHeap.poll(); // evict farthest
    }

    int[][] result = new int[k][2];
    for (int i = 0; i < k; i++) result[i] = maxHeap.poll();
    return result;
}
```

---

## 5.5 Top K Frequent Elements (LC 347) ⭐

```java
int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int n : nums) freq.merge(n, 1, Integer::sum);

    // Min-heap by frequency, keep K highest frequency elements
    PriorityQueue<Integer> minHeap =
        new PriorityQueue<>((a, b) -> freq.get(a) - freq.get(b));

    for (int key : freq.keySet()) {
        minHeap.offer(key);
        if (minHeap.size() > k) minHeap.poll(); // evict least frequent
    }

    int[] result = new int[k];
    for (int i = k - 1; i >= 0; i--) result[i] = minHeap.poll();
    return result;
}
```

**Alternative — Bucket Sort O(N):**

```java
int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int n : nums) freq.merge(n, 1, Integer::sum);

    List<Integer>[] buckets = new List[nums.length + 1];
    for (int key : freq.keySet()) {
        int f = freq.get(key);
        if (buckets[f] == null) buckets[f] = new ArrayList<>();
        buckets[f].add(key);
    }

    int[] result = new int[k];
    int idx = 0;
    for (int i = buckets.length - 1; i >= 0 && idx < k; i--) {
        if (buckets[i] != null)
            for (int n : buckets[i]) if (idx < k) result[idx++] = n;
    }
    return result;
}
```

---

## 5.6 Find K Pairs with Smallest Sums (LC 373)

### Concept

Given two sorted arrays, find K pairs `(nums1[i], nums2[j])` with smallest sums.

```
Key insight: start with (0,0). When we pop (i,j), add (i,j+1) and if j==0, also (i+1,j).
This explores pairs in order of increasing sum.
```

```java
List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
    List<List<Integer>> result = new ArrayList<>();
    if (nums1.length == 0 || nums2.length == 0) return result;

    // Min-heap: [sum, i, j]
    PriorityQueue<int[]> pq =
        new PriorityQueue<>((a, b) -> a[0] - b[0]);

    // Initialize with all pairs (i, 0) — first element of nums2
    for (int i = 0; i < Math.min(nums1.length, k); i++) {
        pq.offer(new int[]{nums1[i] + nums2[0], i, 0});
    }

    while (!pq.isEmpty() && result.size() < k) {
        int[] curr = pq.poll();
        int i = curr[1], j = curr[2];
        result.add(Arrays.asList(nums1[i], nums2[j]));

        if (j + 1 < nums2.length) {
            pq.offer(new int[]{nums1[i] + nums2[j+1], i, j+1});
        }
    }
    return result;
}
```

---

## 5.7 Kth Smallest in Sorted Matrix (LC 378)

```java
int kthSmallest(int[][] matrix, int k) {
    int n = matrix.length;
    // Min-heap: [value, row, col]
    PriorityQueue<int[]> pq =
        new PriorityQueue<>((a, b) -> a[0] - b[0]);

    for (int i = 0; i < n; i++) pq.offer(new int[]{matrix[i][0], i, 0});

    int result = 0;
    while (k-- > 0) {
        int[] curr = pq.poll();
        result = curr[0];
        int r = curr[1], c = curr[2];
        if (c + 1 < n) pq.offer(new int[]{matrix[r][c+1], r, c+1});
    }
    return result;
}
```

---

## 5.8 Task Scheduler (LC 621)

### Concept

Schedule tasks with cooldown `n` between same task types. Minimize total time.

```
tasks = [A,A,A,B,B,B], n=2

Optimal schedule: A B _ A B _ A B
                  1 2 3 4 5 6 7 8 → wait, result = 8

Key formula:
  Most frequent task count = maxCount
  Number of tasks with maxCount = maxCountTasks
  Result = max(len(tasks), (maxCount-1)*(n+1) + maxCountTasks)
```

```java
int leastInterval(char[] tasks, int n) {
    int[] freq = new int[26];
    for (char c : tasks) freq[c - 'A']++;
    Arrays.sort(freq);

    int maxCount = freq[25];
    int maxCountTasks = 0;
    for (int f : freq) if (f == maxCount) maxCountTasks++;

    return Math.max(
        tasks.length,
        (maxCount - 1) * (n + 1) + maxCountTasks
    );
}
```

---

## 5.9 Reorganize String (LC 767)

### Concept

Rearrange string so no two adjacent characters are the same. Return "" if impossible.

```
Strategy: always pick the MOST frequent character that isn't the same as the previous.
Use a max-heap by frequency.

"aab" → freq: a=2, b=1
  Pick a (max freq), place it: "a"     heap: [(2,b),(1,a)]? No...
  Actually: heap has ('a',2) and ('b',1)
  Pick 'a': result="a", reduce to ('a',1)
  Pick 'b': result="ab", reduce to ('b',0), done
  Put ('a',1) back: result="aba" ← but we need to ensure not adjacent
```

```java
String reorganizeString(String s) {
    int[] freq = new int[26];
    for (char c : s.toCharArray()) freq[c - 'a']++;

    // Max-heap by frequency
    PriorityQueue<int[]> maxHeap =
        new PriorityQueue<>((a, b) -> b[1] - a[1]);
    for (int i = 0; i < 26; i++) {
        if (freq[i] > 0) maxHeap.offer(new int[]{i, freq[i]});
    }

    StringBuilder sb = new StringBuilder();
    while (maxHeap.size() >= 2) {
        int[] first  = maxHeap.poll();
        int[] second = maxHeap.poll();

        sb.append((char)('a' + first[0]));
        sb.append((char)('a' + second[0]));

        if (--first[1]  > 0) maxHeap.offer(first);
        if (--second[1] > 0) maxHeap.offer(second);
    }

    if (!maxHeap.isEmpty()) {
        int[] last = maxHeap.poll();
        if (last[1] > 1) return ""; // impossible: one char would be adjacent
        sb.append((char)('a' + last[0]));
    }
    return sb.toString();
}
```

---

## 5.10 Find Median from Data Stream (LC 295) ⭐⭐ HARD

### Concept

Maintain two heaps:
- `maxHeap` (lower half) → top is the LARGEST of the smaller half
- `minHeap` (upper half) → top is the SMALLEST of the larger half

```
Data:  [1, 2, 3, 4, 5]

maxHeap (lower): [1, 2]  → peek = 2
minHeap (upper): [3, 4, 5] → peek = 3

Median = (2 + 3) / 2.0 = 2.5  (even total)

Data: [1, 2, 3]
maxHeap: [1, 2]  minHeap: [3]  OR  maxHeap: [1,2,3]  minHeap: []
Keep |maxHeap| = |minHeap| or |maxHeap| = |minHeap| + 1

Invariant:
  maxHeap.size() == minHeap.size()        (even count)  → median = avg of tops
  maxHeap.size() == minHeap.size() + 1   (odd count)   → median = maxHeap.peek()
```

```java
class MedianFinder {
    PriorityQueue<Integer> maxHeap = // lower half
        new PriorityQueue<>(Collections.reverseOrder());
    PriorityQueue<Integer> minHeap = // upper half
        new PriorityQueue<>();

    void addNum(int num) {
        // Step 1: Add to maxHeap (lower half)
        maxHeap.offer(num);

        // Step 2: Balance: ensure maxHeap.top <= minHeap.top
        if (!minHeap.isEmpty() && maxHeap.peek() > minHeap.peek()) {
            minHeap.offer(maxHeap.poll());
        }

        // Step 3: Balance sizes: maxHeap can have at most 1 more
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.offer(maxHeap.poll());
        } else if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }

    double findMedian() {
        if (maxHeap.size() == minHeap.size()) {
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        }
        return maxHeap.peek(); // maxHeap has one more
    }
}
```

---

## 5.11 Merge K Sorted Lists (LC 23)

```java
ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq =
        new PriorityQueue<>((a, b) -> a.val - b.val);

    for (ListNode head : lists) {
        if (head != null) pq.offer(head);
    }

    ListNode dummy = new ListNode(0), curr = dummy;

    while (!pq.isEmpty()) {
        curr.next = pq.poll();
        curr = curr.next;
        if (curr.next != null) pq.offer(curr.next);
    }
    return dummy.next;
}
```

---

## 5.12 Smallest Range Covering K Lists (LC 632) ⭐ Hard

### Concept

Find the smallest range that includes at least one number from each of K lists.

```
Strategy: use a min-heap. Always track the current MAX element across all lists.
Pop the minimum, the range is [min, currentMax]. To reduce range, advance the
minimum element's list by one step, updating currentMax.
```

```java
int[] smallestRange(List<List<Integer>> nums) {
    // Min-heap: [value, listIndex, elementIndex]
    PriorityQueue<int[]> pq =
        new PriorityQueue<>((a, b) -> a[0] - b[0]);

    int curMax = Integer.MIN_VALUE;

    for (int i = 0; i < nums.size(); i++) {
        pq.offer(new int[]{nums.get(i).get(0), i, 0});
        curMax = Math.max(curMax, nums.get(i).get(0));
    }

    int[] result = new int[]{-100000, 100000};

    while (pq.size() == nums.size()) {
        int[] curr = pq.poll();
        int val = curr[0], i = curr[1], j = curr[2];

        if (curMax - val < result[1] - result[0]) {
            result[0] = val;
            result[1] = curMax;
        }

        if (j + 1 < nums.get(i).size()) {
            int nextVal = nums.get(i).get(j + 1);
            pq.offer(new int[]{nextVal, i, j + 1});
            curMax = Math.max(curMax, nextVal);
        }
        // If a list is exhausted, we can't cover all K lists → stop
    }
    return result;
}
```

### BYTS Problems — Phase 5

| # | Problem | Pattern | Difficulty |
|---|---------|---------|------------|
| 703 | Kth Largest in Stream | MinHeap size K | Easy |
| 1046 | Last Stone Weight | MaxHeap | Easy |
| 215 | Kth Largest Element | MinHeap size K | Med |
| 973 | K Closest Points | MaxHeap size K | Med |
| 347 | Top K Frequent Elements | MinHeap + freq | Med |
| 373 | Find K Pairs Smallest Sums | MinHeap explore | Med |
| 378 | Kth Smallest in Sorted Matrix | MinHeap explore | Med |
| 621 | Task Scheduler | Frequency formula | Med |
| 767 | Reorganize String | MaxHeap + freq | Med |
| 295 | Find Median from Data Stream | Two heaps | Hard |
| 23 | Merge K Sorted Lists | MinHeap | Hard |
| 632 | Smallest Range Covering K Lists | MinHeap + curMax | Hard |
| 658 | Find K Closest Elements | Binary search + deque | Med |
| 1642 | Furthest Building | MinHeap ladders | Med |
| 1792 | Maximum Average Pass Ratio | MaxHeap by improvement | Med |
| 857 | Minimum Cost to Hire K Workers | Sort + MaxHeap | Hard |

---

# ⚫ Phase 6 — Design Problems

---

## 6.1 LRU Cache (LC 146) ⭐⭐ Most Important Design Problem

### Concept

**L**east **R**ecently **U**sed Cache:
- `get(key)` → return value, mark as recently used
- `put(key, val)` → insert, evict LRU if at capacity
- Both O(1)

**Data structure:** HashMap + Doubly Linked List

```
head(dummy) ⇄ [MRU] ⇄ ... ⇄ [LRU] ⇄ tail(dummy)

get → move accessed node to front
put → insert at front; if full, remove from back (LRU)
```

```java
class LRUCache {
    class Node {
        int key, val;
        Node prev, next;
        Node(int k, int v) { key=k; val=v; }
    }

    Map<Integer, Node> map = new HashMap<>();
    Node head = new Node(0,0);  // dummy MRU end
    Node tail = new Node(0,0);  // dummy LRU end
    int capacity;

    LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    int get(int key) {
        if (!map.containsKey(key)) return -1;
        Node node = map.get(key);
        remove(node);
        insertFront(node);
        return node.val;
    }

    void put(int key, int value) {
        if (map.containsKey(key)) {
            remove(map.get(key));
        } else if (map.size() == capacity) {
            Node lru = tail.prev;    // least recently used
            remove(lru);
            map.remove(lru.key);
        }
        Node newNode = new Node(key, value);
        insertFront(newNode);
        map.put(key, newNode);
    }

    void remove(Node n) {
        n.prev.next = n.next;
        n.next.prev = n.prev;
    }

    void insertFront(Node n) {
        n.next = head.next;
        n.prev = head;
        head.next.prev = n;
        head.next = n;
        map.put(n.key, n);      // keep map updated
    }
}
```

---

## 6.2 LFU Cache (LC 460) — Hard

### Concept

**L**east **F**requently **U**sed Cache. When evicting, remove the element with the lowest access count. Tie-break by LRU.

**Data structure:** HashMap (key→node) + HashMap (freq→LinkedHashSet) + minFreq tracking

```java
class LFUCache {
    Map<Integer, Integer> keyVal  = new HashMap<>();
    Map<Integer, Integer> keyFreq = new HashMap<>();
    Map<Integer, LinkedHashSet<Integer>> freqKeys = new HashMap<>();
    int capacity, minFreq = 0;

    LFUCache(int capacity) { this.capacity = capacity; }

    int get(int key) {
        if (!keyVal.containsKey(key)) return -1;
        updateFreq(key);
        return keyVal.get(key);
    }

    void put(int key, int value) {
        if (capacity <= 0) return;

        if (keyVal.containsKey(key)) {
            keyVal.put(key, value);
            updateFreq(key);
            return;
        }

        if (keyVal.size() == capacity) {
            // Evict LFU (then LRU for tie)
            LinkedHashSet<Integer> minFreqSet = freqKeys.get(minFreq);
            int evict = minFreqSet.iterator().next(); // oldest = LRU
            minFreqSet.remove(evict);
            keyVal.remove(evict);
            keyFreq.remove(evict);
        }

        keyVal.put(key, value);
        keyFreq.put(key, 1);
        freqKeys.computeIfAbsent(1, k -> new LinkedHashSet<>()).add(key);
        minFreq = 1;
    }

    void updateFreq(int key) {
        int f = keyFreq.get(key);
        keyFreq.put(key, f + 1);
        freqKeys.get(f).remove(key);
        if (freqKeys.get(f).isEmpty() && f == minFreq) minFreq++;
        freqKeys.computeIfAbsent(f + 1, k -> new LinkedHashSet<>()).add(key);
    }
}
```

### BYTS Problems — Phase 6

| # | Problem | Pattern | Difficulty |
|---|---------|---------|------------|
| 146 | LRU Cache | Doubly LL + HashMap | Med |
| 460 | LFU Cache | freq map + DLL | Hard |

---

# 📋 Master Pattern Decision Table

```
PROBLEM SIGNAL                            → USE
──────────────────────────────────────────────────────────────────
Matching brackets, nesting validation     → Stack (push open, check close)
Undo / evaluate expression                → Stack
Next greater / smaller element            → Monotonic Decreasing Stack
Next smaller element                      → Monotonic Increasing Stack
Largest rectangle / area in histogram    → Monotonic Increasing Stack
Remove digits to minimize number          → Monotonic Increasing Stack
Decode nested structure (k[abc])          → Two Stacks
Calculator with operator precedence       → Stack + operator logic

FIFO processing, BFS levels               → Queue
Sliding window in order                   → Deque (add/remove both ends)
Sliding window MAXIMUM                    → Monotonic Deque (decreasing)
Sliding window MINIMUM                    → Monotonic Deque (increasing)
Shortest path (unweighted)                → BFS Queue
Multi-source spread (rotting, 01 matrix)  → BFS Queue (init all sources)

Kth LARGEST element                       → MinHeap of size K
Kth SMALLEST element                      → MaxHeap of size K
Top K frequent                            → MinHeap by frequency, size K
Stream median                             → Two heaps (max + min)
Merge K sorted lists/arrays              → MinHeap (always add next from winner's list)
Always need current min                   → MinHeap
Always need current max                   → MaxHeap
Task scheduling with cooldown             → MaxHeap + formula
Greedy: always pick most frequent         → MaxHeap by frequency

Design O(1) get + O(1) evict LRU          → Doubly LL + HashMap
Design O(1) get + O(1) evict LFU          → freq map + LinkedHashSet + minFreq
```

---

# 📚 All LC Problems by Category

### Stack — Must Solve
| # | Problem | Difficulty |
|---|---------|------------|
| 20 | Valid Parentheses | Easy |
| 155 | Min Stack | Med |
| 150 | Evaluate Reverse Polish Notation | Med |
| 739 | Daily Temperatures | Med |
| 496 | Next Greater Element I | Easy |
| 503 | Next Greater Element II | Med |
| 394 | Decode String | Med |
| 227 | Basic Calculator II | Med |
| 402 | Remove K Digits | Med |
| 735 | Asteroid Collision | Med |
| 71 | Simplify Path | Med |
| 901 | Online Stock Span | Med |
| 84 | Largest Rectangle in Histogram | Hard |
| 85 | Maximal Rectangle | Hard |
| 907 | Sum of Subarray Minimums | Med |
| 32 | Longest Valid Parentheses | Hard |
| 224 | Basic Calculator | Hard |

### Queue — Must Solve
| # | Problem | Difficulty |
|---|---------|------------|
| 232 | Implement Queue using Stacks | Easy |
| 225 | Implement Stack using Queues | Easy |
| 346 | Moving Average from Data Stream | Easy |
| 362 | Design Hit Counter | Med |
| 994 | Rotting Oranges | Med |
| 542 | 01 Matrix | Med |

### Monotonic Deque
| # | Problem | Difficulty |
|---|---------|------------|
| 239 | Sliding Window Maximum | Hard |
| 862 | Shortest Subarray with Sum ≥ K | Hard |
| 1696 | Jump Game VI | Med |
| 907 | Sum of Subarray Minimums | Med |
| 962 | Maximum Width Ramp | Med |

### Heap — Must Solve
| # | Problem | Difficulty |
|---|---------|------------|
| 703 | Kth Largest in Stream | Easy |
| 1046 | Last Stone Weight | Easy |
| 215 | Kth Largest Element | Med |
| 347 | Top K Frequent Elements | Med |
| 973 | K Closest Points to Origin | Med |
| 373 | Find K Pairs Smallest Sums | Med |
| 378 | Kth Smallest in Sorted Matrix | Med |
| 621 | Task Scheduler | Med |
| 767 | Reorganize String | Med |
| 658 | Find K Closest Elements | Med |
| 1642 | Furthest Building You Can Reach | Med |
| 295 | Find Median from Data Stream | Hard |
| 23 | Merge K Sorted Lists | Hard |
| 632 | Smallest Range Covering K Lists | Hard |
| 857 | Minimum Cost to Hire K Workers | Hard |

### Design
| # | Problem | Difficulty |
|---|---------|------------|
| 146 | LRU Cache | Med |
| 460 | LFU Cache | Hard |

---

*Updated: 2026-06-18 | Java | BYTS Sheet*
*All patterns: Stack · Monotonic Stack · Queue · Deque · Monotonic Deque · Heap · Two Heaps · Design*
