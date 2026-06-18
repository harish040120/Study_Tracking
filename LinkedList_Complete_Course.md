# 🔗 Linked List — Complete Course
**Language:** Java | **Level:** SDE Interview Ready
**Based on:** BYTS Sheet Problems
**Style:** Concept → ASCII Diagram → Why it works → Java Code → Problems

---

# 📌 Contents

**Phase 1 — Foundations**
- 1.1 What is a Linked List?
- 1.2 Types of Linked Lists
- 1.3 ListNode in Java
- 1.4 Java Syntax Reference — Full Sheet
- 1.5 Common Tricks & Traps

**Phase 2 — Basic Operations**
- 2.1 Traverse & Count Length
- 2.2 Insert — Beginning, End, Position
- 2.3 Delete — By Value, By Position
- 2.4 Remove Nth Node From End (LC 19)
- 2.5 Delete Middle Node (LC 2095)
- 2.6 Merge Two Sorted Lists (LC 21)

**Phase 3 — Two Pointer Patterns**
- 3.1 Slow & Fast Pointers — Middle (LC 876)
- 3.2 Floyd's Cycle Detection (LC 141)
- 3.3 Find Cycle Start (LC 142)
- 3.4 Intersection of Two Lists (LC 160)
- 3.5 Fixed Gap Pointer — Remove Nth (LC 19)

**Phase 4 — Reversal Patterns**
- 4.1 Reverse Entire List (LC 206)
- 4.2 Reverse Sub-list (LC 92)
- 4.3 Reverse in K-Groups (LC 25)
- 4.4 Swap Nodes in Pairs (LC 24)
- 4.5 Odd Even Linked List (LC 328)
- 4.6 Reorder List (LC 143)
- 4.7 Palindrome Linked List (LC 234)

**Phase 5 — Merge & Sort Patterns**
- 5.1 Merge Two Sorted Lists (LC 21)
- 5.2 Merge K Sorted Lists (LC 23)
- 5.3 Sort List — Merge Sort (LC 148)
- 5.4 Add Two Numbers (LC 2)
- 5.5 Partition List (LC 86)

**Phase 6 — Advanced Patterns**
- 6.1 Copy List with Random Pointer (LC 138)
- 6.2 Rotate List (LC 61)
- 6.3 Remove Duplicates — Sorted (LC 83)
- 6.4 Remove Duplicates II — All (LC 82)
- 6.5 Plus One Linked List (LC 369)
- 6.6 LRU Cache (LC 146) — DoublyLinkedList + HashMap

**Master Reference**
- Pattern Decision Table
- All BYTS Problems by Phase

---

# 🟢 Phase 1 — Foundations

---

## 1.1 What is a Linked List?

An array stores elements in ONE contiguous block of memory.
A linked list stores elements SCATTERED in memory — connected by pointers.

```
Array:
 ┌────┬────┬────┬────┬────┐
 │ 10 │ 20 │ 30 │ 40 │ 50 │
 └────┴────┴────┴────┴────┘
  [0]  [1]  [2]  [3]  [4]

Linked List:
 ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐
 │ 10   │───▶│ 20   │───▶│ 30   │───▶│ 40   │───▶ null
 │ next │    │ next │    │ next │    │ next │
 └──────┘    └──────┘    └──────┘    └──────┘
   head
```

Each **node** holds:
1. A **value** (`val`)
2. A **pointer** to the next node (`next`)

### Why Linked List over Array?

```
Operation            Array        Linked List
─────────────────────────────────────────────
Access by index      O(1) ✅      O(N) ❌
Insert at beginning  O(N) ❌      O(1) ✅
Insert at middle     O(N)         O(1) if pointer known
Delete from middle   O(N)         O(1) if pointer known
Search               O(N)         O(N)
Memory               Fixed block  Dynamic, scattered
```

---

## 1.2 Types of Linked Lists

### Singly Linked List
Each node points to the NEXT node only. The most common type in LeetCode.

```
head → [1]→[2]→[3]→[4]→ null
```

### Doubly Linked List
Each node has pointers to BOTH next and previous.

```
null ←[1]⇄[2]⇄[3]⇄[4]→ null
       ↑prev  ↑prev  ↑prev
```

Used in: LRU Cache, browser history.

### Circular Linked List
Last node points BACK to head.

```
┌──────────────────────┐
│                      ↓
[1]→[2]→[3]→[4]───────┘
```

---

## 1.3 ListNode in Java

```java
// Standard definition (given in all LeetCode problems)
class ListNode {
    int val;
    ListNode next;

    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}

// Doubly linked list node
class DoublyNode {
    int key, val;
    DoublyNode prev, next;
    DoublyNode(int key, int val) {
        this.key = key;
        this.val = val;
    }
}
```

---

## 1.4 Java Syntax Reference — Full Sheet

```java
// ── CREATE NODES ──────────────────────────────────────────────
ListNode node = new ListNode(val);
ListNode node = new ListNode(val, nextNode);  // with next pointer

// ── TRAVERSE ──────────────────────────────────────────────────
ListNode curr = head;
while (curr != null) {
    System.out.println(curr.val);
    curr = curr.next;
}

// ── NULL SAFETY — MOST COMMON BUG IN LINKED LIST ──────────────
// Always check before accessing .next or .next.next
if (curr != null)                     // before curr.val
if (curr != null && curr.next != null) // before curr.next.val
if (fast != null && fast.next != null) // before fast.next.next

// ── ADVANCE POINTER ────────────────────────────────────────────
curr = curr.next;              // one step
fast = fast.next.next;         // two steps (check null first!)

// ── DUMMY NODE — removes head edge cases ──────────────────────
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode curr = dummy;
// ... operations ...
return dummy.next;

// ── COUNT LENGTH ──────────────────────────────────────────────
int len = 0;
for (ListNode c = head; c != null; c = c.next) len++;

// ── FIND TAIL ─────────────────────────────────────────────────
ListNode tail = head;
while (tail.next != null) tail = tail.next;

// ── CONNECT / DISCONNECT ──────────────────────────────────────
prev.next = curr.next;        // delete curr (skip over it)
curr.next = null;             // cut off rest of list from curr
nodeA.next = nodeB;           // connect A to B

// ── REVERSE POINTER (the 3-step flip) ─────────────────────────
ListNode next = curr.next;    // 1. save next
curr.next = prev;             // 2. flip pointer backward
prev = curr;                  // 3. advance prev
curr = next;                  // 4. advance curr

// ── SWAP VALUES VS SWAP NODES ─────────────────────────────────
// Swap values (easy but sometimes not allowed):
int tmp = a.val; a.val = b.val; b.val = tmp;
// Swap nodes (pointer manipulation — preferred in interviews)
// → requires tracking previous node of both

// ── PRIORITY QUEUE FOR LISTNODE ───────────────────────────────
PriorityQueue<ListNode> pq =
    new PriorityQueue<>((a, b) -> a.val - b.val); // min heap by val
pq.offer(node);
ListNode min = pq.poll();

// ── BUILD LIST FROM ARRAY (for testing) ───────────────────────
ListNode buildList(int[] arr) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    for (int x : arr) { curr.next = new ListNode(x); curr = curr.next; }
    return dummy.next;
}
```

---

## 1.5 Common Tricks & Traps

### ⭐ Trick 1 — Dummy Node

Use a dummy (sentinel) head node to avoid special-casing operations on the real head.

```
Without dummy: if deleting head, you need special logic
With dummy:    dummy→[1]→[2]→[3]→null
               always has a node before whatever you want to delete
               always return dummy.next as the real head
```

```java
ListNode dummy = new ListNode(0, head);
ListNode prev = dummy;
// delete node with value X:
while (prev.next != null) {
    if (prev.next.val == X) {
        prev.next = prev.next.next; // skip it
    } else {
        prev = prev.next;
    }
}
return dummy.next;
```

### ⭐ Trick 2 — Two Pointers with a Gap

To find Nth from end in ONE pass: advance `fast` N steps ahead first, then move both until `fast` is null — `slow` is now at the target.

```
List:  1→2→3→4→5→null   (N=2, find 2nd from end = node 4)

Step 1: advance fast N=2 steps
        fast=3, slow=1

Step 2: move both until fast reaches null
        fast=4, slow=2
        fast=5, slow=3
        fast=null, slow=4   ← slow is at the target!
```

### ⭐ Trick 3 — Slow/Fast for Middle

When `fast` reaches end (null), `slow` is at the middle.

```
1→2→3→4→5→null
slow=1, fast=1
slow=2, fast=3
slow=3, fast=5
fast.next=null → STOP

slow=3 = middle ✅

Even length:
1→2→3→4→null
slow=2 = second middle (use slow.next for first middle variant)
```

### ⭐ Trick 4 — In-Place Reversal (3-pointer pattern)

Memorize this ORDER: save next → flip → advance prev → advance curr.

```
Before flip:  prev → null    curr → [1]→[2]→[3]→null

Step 1: next = curr.next     next → [2]
Step 2: curr.next = prev     [1]→null
Step 3: prev = curr          prev → [1]
Step 4: curr = next          curr → [2]

After:  null←[1]    curr→[2]→[3]→null
```

### ⭐ Trick 5 — Don't Forget to Terminate

After rearranging a list, the tail node often still points somewhere unexpected.
Always explicitly set `.next = null` on the new tail.

```java
// WRONG: After splitting a list, oldTail still points to something
// RIGHT:
slow.next = null;  // cut the list at the split point
```

### ⚠️ Trap — NullPointerException

The #1 bug in linked list code. Always check before dereferencing.

```java
// This crashes if fast is null or fast.next is null:
fast = fast.next.next;  // ❌

// Safe version:
while (fast != null && fast.next != null) {
    fast = fast.next.next;  // ✅ only runs if both are non-null
}
```

---

# 🔵 Phase 2 — Basic Operations

---

## 2.1 Traverse & Count Length

```java
// Count length
int length(ListNode head) {
    int len = 0;
    for (ListNode curr = head; curr != null; curr = curr.next)
        len++;
    return len;
}

// Print all values
void print(ListNode head) {
    StringBuilder sb = new StringBuilder();
    for (ListNode c = head; c != null; c = c.next) {
        sb.append(c.val);
        if (c.next != null) sb.append(" → ");
    }
    sb.append(" → null");
    System.out.println(sb);
}
```

---

## 2.2 Insert — Beginning, End, Position

```java
// INSERT AT BEGINNING — O(1)
ListNode insertAtBeginning(ListNode head, int val) {
    ListNode newNode = new ListNode(val);
    newNode.next = head;
    return newNode;  // newNode becomes new head
}

// INSERT AT END — O(N)
ListNode insertAtEnd(ListNode head, int val) {
    ListNode newNode = new ListNode(val);
    if (head == null) return newNode;
    ListNode tail = head;
    while (tail.next != null) tail = tail.next;
    tail.next = newNode;
    return head;
}

// INSERT AT POSITION k (0-indexed) — O(N)
ListNode insertAt(ListNode head, int val, int k) {
    ListNode dummy = new ListNode(0, head);
    ListNode curr = dummy;
    for (int i = 0; i < k; i++) {
        if (curr.next == null) break;
        curr = curr.next;
    }
    ListNode newNode = new ListNode(val);
    newNode.next = curr.next;  // new → old kth
    curr.next = newNode;       // (k-1)th → new
    return dummy.next;
}
```

---

## 2.3 Delete — By Value, By Position

```java
// DELETE BY VALUE — remove first occurrence
ListNode deleteByValue(ListNode head, int val) {
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;

    while (prev.next != null) {
        if (prev.next.val == val) {
            prev.next = prev.next.next; // skip the node
            break;
        }
        prev = prev.next;
    }
    return dummy.next;
}

// DELETE ALL OCCURRENCES OF VALUE (LC 203)
ListNode removeElements(ListNode head, int val) {
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;

    while (prev.next != null) {
        if (prev.next.val == val) {
            prev.next = prev.next.next; // skip
            // DON'T advance prev — new prev.next might also match
        } else {
            prev = prev.next;
        }
    }
    return dummy.next;
}
```

---

## 2.4 Remove Nth Node From End (LC 19) ⭐

### Concept

Find and delete the Nth node from the end in **ONE pass** using the fixed-gap trick.

```
List: 1→2→3→4→5→null   N=2

Step 1: Create dummy, advance fast N+1 steps
        dummy→1→2→3→4→5→null
              ↑
              slow=dummy
        After N+1 steps:  fast=3, slow=dummy

Step 2: Move both until fast=null
        fast=4, slow=1
        fast=5, slow=2
        fast=null, slow=3    ← slow is JUST BEFORE target (node 4)

Step 3: slow.next = slow.next.next (skip node 4)
Result: 1→2→3→5→null ✅
```

```java
ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0, head);
    ListNode fast = dummy;
    ListNode slow = dummy;

    // Advance fast n+1 steps ahead
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }

    // Move both until fast hits null
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }

    // slow is just before the node to delete
    slow.next = slow.next.next;

    return dummy.next;
}
```

---

## 2.5 Delete Middle Node (LC 2095)

### Concept

Delete the MIDDLE node of the list (if even length, delete second middle).

```
1→2→3→4→5   mid=3 (index 2), delete it → 1→2→4→5
1→2→3→4     mid=3 (second middle), delete it → 1→2→4
```

Use slow/fast pointers, but track the node BEFORE slow so you can skip slow.

```java
ListNode deleteMiddle(ListNode head) {
    if (head == null || head.next == null) return null;

    ListNode slow = head;
    ListNode fast = head;
    ListNode prev = null; // tracks node before slow

    while (fast != null && fast.next != null) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }

    // slow is at middle, prev is just before it
    prev.next = slow.next;
    return head;
}
```

---

## 2.6 Merge Two Sorted Lists (LC 21)

### Concept

Merge two sorted lists into one sorted list. Use a dummy head and a pointer — at each step attach the smaller node.

```
l1: 1→2→4→null
l2: 1→3→4→null

dummy→?
Compare: l1.val(1) == l2.val(1), attach l1
dummy→1    l1 moves to 2
Compare: l1.val(2) > l2.val(1), attach l2
dummy→1→1  l2 moves to 3
Compare: l1.val(2) < l2.val(3), attach l1
...
Result: 1→1→2→3→4→4
```

```java
ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;

    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) {
            curr.next = l1;
            l1 = l1.next;
        } else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }

    curr.next = (l1 != null) ? l1 : l2; // attach remaining

    return dummy.next;
}
```

### BYTS Problems — Phase 2

| # | Problem | Pattern |
|---|---------|---------|
| 19 | Remove Nth Node From End | Fixed-gap two pointer |
| 876 | Middle of the Linked List | Slow/fast middle |
| 2095 | Delete the Middle Node | Slow/fast + prev |
| 21 | Merge Two Sorted Lists | Dummy + compare |
| 83 | Remove Duplicates from Sorted List | Traverse and skip |

---

# 🟡 Phase 3 — Two Pointer Patterns

---

## 3.1 Slow & Fast Pointers — Middle (LC 876)

### Concept

Slow moves 1 step, fast moves 2 steps. When fast reaches the end, slow is at the middle.

```
Odd length:  1→2→3→4→5→null
  s=1,f=1
  s=2,f=3
  s=3,f=5   (fast.next = null → stop)
  slow=3 = MIDDLE ✅

Even length: 1→2→3→4→null
  s=1,f=1
  s=2,f=3
  s=3,f=null (fast = null → stop)
  slow=3 = SECOND middle
```

```java
ListNode middleNode(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow; // middle node (second middle for even length)
}
```

**Variant — first middle for even length** (stop one step earlier):

```java
while (fast.next != null && fast.next.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
// slow = first middle for even, same for odd
```

---

## 3.2 Floyd's Cycle Detection (LC 141)

### Concept

If a cycle exists, fast will LOOP BACK and eventually lap slow. Like two runners on a circular track — the faster runner catches the slower one.

```
No cycle:   1→2→3→4→null
            fast hits null → no cycle

Has cycle:  1→2→3→4→2 (points back to node 2)
            s=1,f=1
            s=2,f=3
            s=3,f=2  (fast looped back)
            s=4,f=4  ← THEY MEET → cycle confirmed!
```

```java
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) return true; // they met → cycle!
    }

    return false; // fast hit null → no cycle
}
```

---

## 3.3 Find Cycle Start (LC 142)

### Concept

After detecting the meeting point inside the cycle, there's a mathematical property:

> **Distance(head → cycle start) = Distance(meeting point → cycle start)**

Proof sketch: if cycle starts at distance `F` from head, and the cycle has length `C`, when slow and fast meet inside the cycle:
- slow has traveled: `F + a` steps (where `a` = distance from cycle start to meeting point)
- fast has traveled: `F + a + nC` steps (completed `n` full loops)
- fast = 2 × slow → `F + a + nC = 2(F + a)` → `F = nC - a`

This means: if you reset one pointer to `head` and move both one step at a time, they meet exactly at the cycle start.

```
List: 1→2→3→4→5
              ↑   ↓
              8←7←6
              (cycle: 3→4→5→6→7→8→3)

After meeting point found (somewhere inside cycle):
Reset slow → head (node 1)
Move BOTH one step at a time:
They meet at node 3 = cycle start ✅
```

```java
ListNode detectCycle(ListNode head) {
    ListNode slow = head, fast = head;

    // Phase 1: find meeting point
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) break;
    }

    // No cycle
    if (fast == null || fast.next == null) return null;

    // Phase 2: find cycle start
    slow = head;               // reset one pointer to head
    while (slow != fast) {
        slow = slow.next;      // both move ONE step now
        fast = fast.next;
    }

    return slow;               // cycle start node
}
```

---

## 3.4 Intersection of Two Linked Lists (LC 160)

### Concept

Two lists may share a common "tail" starting at some node. Find that node.

```
List A:  a1→a2→c1→c2→c3→null
List B:  b1→b2→b3→c1→c2→c3→null
                   ↑
              intersection at c1
```

**Key insight:** if pointer A traverses list A then list B, and pointer B traverses list B then list A, they travel the SAME total distance — so they meet at the intersection (or both reach null if no intersection).

```
A path: a1→a2→c1→c2→c3→null→b1→b2→b3→c1...
B path: b1→b2→b3→c1→c2→c3→null→a1→a2→c1...
Both reach c1 at the same step!
```

```java
ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode a = headA, b = headB;

    // Both pointers travel lenA + lenB steps total
    while (a != b) {
        a = (a == null) ? headB : a.next; // when A ends, switch to B
        b = (b == null) ? headA : b.next; // when B ends, switch to A
    }

    return a; // either intersection node or null (no intersection)
}
```

---

## 3.5 Fixed Gap Pointer — Remove Nth (LC 19)

Already covered in Phase 2 Section 2.4.

### BYTS Problems — Phase 3

| # | Problem | Pattern |
|---|---------|---------|
| 876 | Middle of the Linked List | Slow/fast middle |
| 141 | Linked List Cycle | Floyd's detection |
| 142 | Linked List Cycle II | Floyd's + reset |
| 160 | Intersection of Two Linked Lists | Two pointers swap lists |
| 19 | Remove Nth Node From End | Fixed gap |
| 234 | Palindrome Linked List | Mid + reverse + compare |

---

# 🟠 Phase 4 — Reversal Patterns

---

## 4.1 Reverse Entire List (LC 206)

### Concept

Three pointers: `prev`, `curr`, `next`.
At each step: save next, flip curr's pointer backward, advance both.

```
Before:  null  [1]→[2]→[3]→[4]→null
               ↑
           curr, prev=null

Step 1: next=2, [1]→null, prev=1, curr=2
        null←[1]    curr→[2]→[3]→[4]→null

Step 2: next=3, [2]→[1], prev=2, curr=3
        null←[1]←[2]    curr→[3]→[4]→null

Step 3: next=4, [3]→[2], prev=3, curr=4
        null←[1]←[2]←[3]    curr→[4]→null

Step 4: next=null, [4]→[3], prev=4, curr=null
        null←[1]←[2]←[3]←[4]
                              ↑
                            prev = new head
```

```java
ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;

    while (curr != null) {
        ListNode next = curr.next;  // 1. SAVE next
        curr.next = prev;           // 2. FLIP pointer
        prev = curr;                // 3. ADVANCE prev
        curr = next;                // 4. ADVANCE curr
    }

    return prev; // prev is the new head
}
```

**Recursive version:**

```java
ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode newHead = reverseList(head.next); // reverse rest
    head.next.next = head; // make next node point back to current
    head.next = null;      // break original forward pointer
    return newHead;
}
```

---

## 4.2 Reverse Sub-list (LC 92)

### Concept

Reverse ONLY the nodes from position `left` to `right`.

```
Input:  1→2→3→4→5   left=2, right=4
Goal:   1→4→3→2→5

Step 1: Navigate to node just before 'left' (prev stops at node 1)
        dummy→1→2→3→4→5
                ↑
               curr = node 2 (start of reversal)

Step 2: Reverse (right-left) = 2 connections:
  Iteration 1:
    next = curr.next = 3
    curr.next = next.next = 4  →  1→2→4→5, next=3
    next.next = prev.next = 2  →  3→2
    prev.next = next = 3       →  1→3→2→4→5
    State: dummy→1→3→2→4→5, curr still = node 2

  Iteration 2:
    next = curr.next = 4
    curr.next = next.next = 5  →  2→5
    next.next = prev.next = 3  →  4→3
    prev.next = next = 4       →  1→4→3→2→5
    State: dummy→1→4→3→2→5 ✅
```

```java
ListNode reverseBetween(ListNode head, int left, int right) {
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;

    // Step 1: move prev to node just before 'left'
    for (int i = 0; i < left - 1; i++) {
        prev = prev.next;
    }

    // Step 2: reverse (right - left) connections
    ListNode curr = prev.next;
    for (int i = 0; i < right - left; i++) {
        ListNode next = curr.next;     // save next
        curr.next = next.next;         // cut next out
        next.next = prev.next;         // next → front of segment
        prev.next = next;              // prev → next (insert at front)
    }

    return dummy.next;
}
```

---

## 4.3 Reverse in K-Groups (LC 25)

### Concept

Reverse exactly K consecutive nodes at a time. If fewer than K nodes remain, leave them as-is.

```
Input:  1→2→3→4→5   k=2
Step 1: reverse [1,2] → 2→1
Step 2: reverse [3,4] → 4→3
Step 3: only [5] left, k=2 not met → leave as-is
Result: 2→1→4→3→5
```

```java
ListNode reverseKGroup(ListNode head, int k) {
    // Check if at least k nodes remain
    ListNode check = head;
    for (int i = 0; i < k; i++) {
        if (check == null) return head; // fewer than k → don't reverse
        check = check.next;
    }

    // Reverse k nodes
    ListNode prev = null, curr = head;
    for (int i = 0; i < k; i++) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }

    // head is now the TAIL of the reversed group
    // connect it to the result of reversing the REST
    head.next = reverseKGroup(curr, k);

    return prev; // prev is the new head of this group
}
```

```
Before:  [1]→[2]→[3]→[4]→[5]  k=2
After reversing first 2: prev=[2], head=[1], curr=[3]
head.next = reverseKGroup([3,4,5], 2) → returns [4]→[3]→[5]
So [1].next = [4]→[3]→[5]
Return [2]→[1]→[4]→[3]→[5] ✅
```

---

## 4.4 Swap Nodes in Pairs (LC 24)

### Concept

Swap every two adjacent nodes.

```
Input:  1→2→3→4
Output: 2→1→4→3
```

```java
ListNode swapPairs(ListNode head) {
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;

    while (prev.next != null && prev.next.next != null) {
        ListNode first  = prev.next;       // node to be second
        ListNode second = prev.next.next;  // node to be first

        // Rewire
        first.next  = second.next; // first → node after pair
        second.next = first;       // second → first
        prev.next   = second;      // prev → second (new front)

        prev = first; // advance past this pair
    }

    return dummy.next;
}
```

```
dummy→[1]→[2]→[3]→[4]
       ↑first ↑second

After rewire:
dummy→[2]→[1]→[3]→[4]
            ↑prev

Next iteration swaps [3] and [4]
dummy→[2]→[1]→[4]→[3] ✅
```

---

## 4.5 Odd Even Linked List (LC 328)

### Concept

Group all ODD-indexed nodes first, then all EVEN-indexed nodes. Maintain relative order within each group.

```
Input:  1→2→3→4→5
Odd:    1→3→5
Even:   2→4
Output: 1→3→5→2→4
```

```java
ListNode oddEvenList(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode odd  = head;            // odd group pointer
    ListNode even = head.next;       // even group pointer
    ListNode evenHead = even;        // save even group's start

    while (even != null && even.next != null) {
        odd.next  = even.next;       // odd → next odd node
        odd       = odd.next;        // advance odd

        even.next = odd.next;        // even → next even node
        even      = even.next;       // advance even
    }

    odd.next = evenHead;             // connect odd tail to even head

    return head;
}
```

---

## 4.6 Reorder List (LC 143)

### Concept

Reorder L0→L1→...→Ln-1→Ln as L0→Ln→L1→Ln-1→L2→Ln-2→...

**Algorithm:** three steps:
1. Find the middle (slow/fast)
2. Reverse the second half
3. Merge the two halves alternately

```
Input:   1→2→3→4→5
Middle:  1→2→3  and  4→5

Reverse second half: 5→4
Merge alternately:   1→5→2→4→3
```

```java
void reorderList(ListNode head) {
    if (head == null || head.next == null) return;

    // Step 1: Find middle
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    // slow = end of first half

    // Step 2: Reverse second half
    ListNode second = reverseList(slow.next);
    slow.next = null; // cut the list — IMPORTANT!

    // Step 3: Merge alternately
    ListNode first = head;
    while (second != null) {
        ListNode tmp1 = first.next;  // save first's next
        ListNode tmp2 = second.next; // save second's next

        first.next  = second;        // first → second
        second.next = tmp1;          // second → rest of first

        first  = tmp1;               // advance first
        second = tmp2;               // advance second
    }
}
```

```
first:  1→2→3→null
second: 5→4→null

Iteration 1:
  tmp1=2, tmp2=4
  1→5, 5→2
  first=2, second=4

Iteration 2:
  tmp1=3, tmp2=null
  2→4, 4→3
  first=3, second=null

Result: 1→5→2→4→3 ✅
```

---

## 4.7 Palindrome Linked List (LC 234)

### Concept

Check if a linked list is a palindrome. Three steps:
1. Find middle
2. Reverse second half in-place
3. Compare both halves

```
Input: 1→2→2→1

Middle: 1→2 and 2→1
Reverse second half: 1→2
Compare: 1==1, 2==2 ✅ palindrome
```

```java
boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) return true;

    // Step 1: Find middle (second middle for even length)
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // Step 2: Reverse second half
    ListNode secondHalf = reverseList(slow);
    ListNode secondCopy = secondHalf; // save to restore later (optional)

    // Step 3: Compare
    ListNode left = head, right = secondHalf;
    boolean isPalin = true;
    while (right != null) {
        if (left.val != right.val) {
            isPalin = false;
            break;
        }
        left = left.next;
        right = right.next;
    }

    return isPalin;
}

ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### BYTS Problems — Phase 4

| # | Problem | Pattern |
|---|---------|---------|
| 206 | Reverse Linked List | 3-pointer reversal |
| 92 | Reverse Linked List II | Partial reversal |
| 25 | Reverse Nodes in k-Group | Group reversal recursive |
| 24 | Swap Nodes in Pairs | Pair reversal |
| 328 | Odd Even Linked List | Group by parity |
| 143 | Reorder List | Middle + reverse + merge |
| 234 | Palindrome Linked List | Middle + reverse + compare |

---

# 🔴 Phase 5 — Merge & Sort Patterns

---

## 5.1 Merge Two Sorted Lists (LC 21)

Already covered in Phase 2. Key recap:

```java
// Dummy head + compare at each step
ListNode dummy = new ListNode(0);
ListNode curr = dummy;
while (l1 != null && l2 != null) {
    if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
    else                  { curr.next = l2; l2 = l2.next; }
    curr = curr.next;
}
curr.next = (l1 != null) ? l1 : l2;
return dummy.next;
```

---

## 5.2 Merge K Sorted Lists (LC 23)

### Concept

Put the HEAD of each list into a MinHeap. Always pull the minimum, then add its `.next` into the heap.

```
lists = [1→4→5, 1→3→4, 2→6]

Heap initially: [1(list0), 1(list1), 2(list2)]

Pull min=1(list0), add 4(list0) to heap:  result=1
Heap: [1(list1), 2(list2), 4(list0)]

Pull min=1(list1), add 3(list1) to heap:  result=1→1
Heap: [2(list2), 3(list1), 4(list0)]

Pull min=2(list2), add 6(list2) to heap:  result=1→1→2
...continues until all lists exhausted
```

```java
ListNode mergeKLists(ListNode[] lists) {
    // MinHeap ordered by node value
    PriorityQueue<ListNode> pq =
        new PriorityQueue<>((a, b) -> a.val - b.val);

    // Add first node of each list
    for (ListNode head : lists) {
        if (head != null) pq.offer(head);
    }

    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;

    while (!pq.isEmpty()) {
        curr.next = pq.poll();           // take the min
        curr = curr.next;

        if (curr.next != null) {
            pq.offer(curr.next);          // add its next to heap
        }
    }

    return dummy.next;
}
```

**Time: O(N log K)** where N = total nodes, K = number of lists
**Space: O(K)** for the heap

---

## 5.3 Sort List (LC 148) — Merge Sort

### Concept

Sort a linked list in O(N log N) time and O(log N) space (recursion stack).

**Why merge sort?** QuickSort on linked lists is slow because there's no random access. Merge sort is perfect — split in half repeatedly, merge results.

```
4→2→1→3
   ↓ split
4→2   1→3
  ↓ split  ↓ split
4  2    1  3
  ↓ merge  ↓ merge
2→4    1→3
    ↓ merge
1→2→3→4
```

```java
ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;

    // Step 1: Find middle and split
    ListNode mid = getMiddle(head);
    ListNode rightHead = mid.next;
    mid.next = null;              // CUT the list in two halves

    // Step 2: Sort each half recursively
    ListNode left  = sortList(head);
    ListNode right = sortList(rightHead);

    // Step 3: Merge two sorted halves
    return mergeTwoLists(left, right);
}

ListNode getMiddle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow; // first middle for even length (important for correct split)
}

ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
        else                  { curr.next = l2; l2 = l2.next; }
        curr = curr.next;
    }
    curr.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

---

## 5.4 Add Two Numbers (LC 2)

### Concept

Two numbers stored as REVERSED linked lists (least significant digit first). Add digit by digit with a carry.

```
l1: 2→4→3   (represents 342)
l2: 5→6→4   (represents 465)
Sum: 342 + 465 = 807
Result: 7→0→8 ✅

Step by step:
  2+5=7, carry=0  → node 7
  4+6=10, carry=1 → node 0
  3+4+1=8, carry=0→ node 8
```

```java
ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    int carry = 0;

    while (l1 != null || l2 != null || carry != 0) {
        int sum = carry;

        if (l1 != null) { sum += l1.val; l1 = l1.next; }
        if (l2 != null) { sum += l2.val; l2 = l2.next; }

        carry = sum / 10;              // carry for next digit
        curr.next = new ListNode(sum % 10); // current digit
        curr = curr.next;
    }

    return dummy.next;
}
```

---

## 5.5 Partition List (LC 86)

### Concept

Rearrange list so all nodes with value < x come before nodes with value ≥ x. Maintain relative order within each partition.

```
Input: 1→4→3→2→5→2, x=3
Less (<3):  1→2→2
Greater (≥3): 4→3→5
Result: 1→2→2→4→3→5
```

```java
ListNode partition(ListNode head, int x) {
    ListNode lessHead    = new ListNode(0); // dummy for < x chain
    ListNode greaterHead = new ListNode(0); // dummy for ≥ x chain
    ListNode less    = lessHead;
    ListNode greater = greaterHead;

    while (head != null) {
        if (head.val < x) {
            less.next = head;
            less = less.next;
        } else {
            greater.next = head;
            greater = greater.next;
        }
        head = head.next;
    }

    greater.next = null;             // ⚠️ MUST terminate the list!
    less.next = greaterHead.next;    // connect less-chain to greater-chain

    return lessHead.next;
}
```

### BYTS Problems — Phase 5

| # | Problem | Pattern |
|---|---------|---------|
| 21 | Merge Two Sorted Lists | Dummy + compare |
| 23 | Merge K Sorted Lists | MinHeap |
| 148 | Sort List | Merge sort on list |
| 2 | Add Two Numbers | Digit carry |
| 86 | Partition List | Two dummy chains |
| 88 | Merge Sorted Array | Two pointer in-array |

---

# 🟣 Phase 6 — Advanced Patterns

---

## 6.1 Copy List with Random Pointer (LC 138)

### Concept

Each node has `next` AND a `random` pointer (which can point to ANY node or null). Create a DEEP copy.

**Naive approach:** HashMap stores `original → copy` mapping.

```
Original:  [1]→[2]→[3]→null
            ↓random   ↑random
            [3]     [1]

Step 1: Create all copy nodes, store mapping
  map: {1→1', 2→2', 3→3'}

Step 2: Set next and random for each copy
  1'.next = map[1.next] = map[2] = 2'
  1'.random = map[1.random] = map[3] = 3'
  ...
```

```java
// METHOD 1: HashMap approach — O(N) space
Node copyRandomList(Node head) {
    if (head == null) return null;

    Map<Node, Node> map = new HashMap<>();

    // Pass 1: create all copy nodes
    Node curr = head;
    while (curr != null) {
        map.put(curr, new Node(curr.val));
        curr = curr.next;
    }

    // Pass 2: set next and random pointers
    curr = head;
    while (curr != null) {
        map.get(curr).next   = map.get(curr.next);   // copy's next
        map.get(curr).random = map.get(curr.random); // copy's random
        curr = curr.next;
    }

    return map.get(head);
}
```

**METHOD 2: Interleave copy nodes — O(1) space**

```
Step 1: Weave copy nodes after originals
  1→1'→2→2'→3→3'→null

Step 2: Set random pointers for copies
  1'.random = 1.random.next = 3.next = 3'

Step 3: Separate the two lists
```

```java
Node copyRandomList(Node head) {
    if (head == null) return null;

    // Step 1: Insert copy nodes after each original
    Node curr = head;
    while (curr != null) {
        Node copy = new Node(curr.val);
        copy.next = curr.next;
        curr.next = copy;
        curr = copy.next;
    }

    // Step 2: Set random pointers for copies
    curr = head;
    while (curr != null) {
        if (curr.random != null) {
            curr.next.random = curr.random.next; // copy's random
        }
        curr = curr.next.next; // skip to next original
    }

    // Step 3: Separate original and copy lists
    Node dummy = new Node(0);
    Node copyCurr = dummy;
    curr = head;
    while (curr != null) {
        copyCurr.next = curr.next;       // copy node
        curr.next = curr.next.next;      // restore original's next
        copyCurr = copyCurr.next;
        curr = curr.next;
    }

    return dummy.next;
}
```

---

## 6.2 Rotate List (LC 61)

### Concept

Rotate the list to the right by k places.

```
Input:  1→2→3→4→5   k=2
Output: 4→5→1→2→3

Key insight:
  New head = (n-k)th node from beginning
  Old tail connects to old head
  Node just before new head becomes new tail

length=5, k=2
  new head position = 5-2 = 3rd node = node 4
  Connect tail(5) → head(1)
  Cut: node3.next = null, node4 is new head
```

```java
ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;

    // Step 1: Find length and tail
    int len = 1;
    ListNode tail = head;
    while (tail.next != null) { tail = tail.next; len++; }

    // Step 2: k mod len (if k=len, no change)
    k = k % len;
    if (k == 0) return head;

    // Step 3: Find new tail (len - k - 1 steps from head)
    ListNode newTail = head;
    for (int i = 0; i < len - k - 1; i++) newTail = newTail.next;

    // Step 4: Rewire
    ListNode newHead = newTail.next;
    newTail.next = null;  // cut
    tail.next = head;     // old tail → old head (make circular)

    return newHead;
}
```

---

## 6.3 Remove Duplicates — Sorted (LC 83)

```java
ListNode deleteDuplicates(ListNode head) {
    ListNode curr = head;

    while (curr != null && curr.next != null) {
        if (curr.val == curr.next.val) {
            curr.next = curr.next.next; // skip duplicate
        } else {
            curr = curr.next;
        }
    }
    return head;
}
```

---

## 6.4 Remove All Duplicates — Sorted (LC 82)

Remove ALL nodes that had ANY duplicates (not just the extra copies).

```
Input:  1→2→3→3→4→4→5
Output: 1→2→5
(both 3s and both 4s removed entirely)
```

```java
ListNode deleteDuplicates(ListNode head) {
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;

    while (prev.next != null) {
        ListNode curr = prev.next;

        // Check if this node has duplicates
        if (curr.next != null && curr.val == curr.next.val) {
            int dupVal = curr.val;
            // Skip ALL nodes with this value
            while (prev.next != null && prev.next.val == dupVal) {
                prev.next = prev.next.next;
            }
            // DON'T advance prev — new prev.next might also be duplicate
        } else {
            prev = prev.next; // no duplicate, safe to advance
        }
    }

    return dummy.next;
}
```

---

## 6.5 Plus One Linked List (LC 369)

Add 1 to a number represented as a linked list (most significant digit first).

```
Input:  1→2→9  (represents 129)
Output: 1→3→0  (represents 130)

Input:  9→9→9  (represents 999)
Output: 1→0→0→0  (represents 1000)
```

**Key insight:** find the RIGHTMOST non-9 node, increment it, set all nodes to its right to 0.

```java
ListNode plusOne(ListNode head) {
    ListNode dummy = new ListNode(0, head);
    ListNode notNine = dummy; // rightmost node that is NOT 9

    // Find rightmost non-9 node
    ListNode curr = head;
    while (curr != null) {
        if (curr.val != 9) notNine = curr;
        curr = curr.next;
    }

    // Increment it
    notNine.val++;

    // Set all nodes after it to 0
    ListNode toZero = notNine.next;
    while (toZero != null) {
        toZero.val = 0;
        toZero = toZero.next;
    }

    return dummy.val == 1 ? dummy : dummy.next;
}
```

---

## 6.6 LRU Cache (LC 146) — DoublyLinkedList + HashMap

### Concept

Design a data structure for Least Recently Used (LRU) cache:
- `get(key)` → return value if exists, else -1. Marks key as recently used.
- `put(key, value)` → insert/update key. If at capacity, evict the LEAST recently used key.

**Both operations must be O(1).**

**Data structure:**
- **HashMap** `{key → node}` for O(1) lookup
- **Doubly Linked List** to track usage order — most recent at FRONT, least recent at BACK
- Dummy `head` and `tail` nodes to avoid edge cases

```
head(dummy) ⇄ [most_recent] ⇄ ... ⇄ [least_recent] ⇄ tail(dummy)

get(key):   move that node to front
put(key):   add new node to front; if over capacity, remove from back
```

```java
class LRUCache {
    private Map<Integer, DoublyNode> map;
    private int capacity;
    private DoublyNode head, tail; // dummy boundary nodes

    class DoublyNode {
        int key, val;
        DoublyNode prev, next;
        DoublyNode(int key, int val) { this.key = key; this.val = val; }
    }

    LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();
        head = new DoublyNode(0, 0); // dummy head (most recent side)
        tail = new DoublyNode(0, 0); // dummy tail (least recent side)
        head.next = tail;
        tail.prev = head;
    }

    int get(int key) {
        if (!map.containsKey(key)) return -1;
        DoublyNode node = map.get(key);
        moveToFront(node);    // mark as recently used
        return node.val;
    }

    void put(int key, int value) {
        if (map.containsKey(key)) {
            DoublyNode node = map.get(key);
            node.val = value;
            moveToFront(node); // update and mark recent
        } else {
            if (map.size() == capacity) {
                // Evict LRU (just before dummy tail)
                DoublyNode lru = tail.prev;
                remove(lru);
                map.remove(lru.key);
            }
            DoublyNode newNode = new DoublyNode(key, value);
            insertFront(newNode);
            map.put(key, newNode);
        }
    }

    // Insert node right after dummy head
    private void insertFront(DoublyNode node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    // Remove a node from its current position
    private void remove(DoublyNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void moveToFront(DoublyNode node) {
        remove(node);
        insertFront(node);
    }
}
```

```
State after put(1,1), put(2,2), capacity=2:
head ⇄ [2,2] ⇄ [1,1] ⇄ tail

After get(1):
head ⇄ [1,1] ⇄ [2,2] ⇄ tail   (1 moved to front)

After put(3,3) — capacity full, evict LRU which is [2,2]:
head ⇄ [3,3] ⇄ [1,1] ⇄ tail
```

### BYTS Problems — Phase 6

| # | Problem | Pattern |
|---|---------|---------|
| 138 | Copy List with Random Pointer | HashMap / interleave |
| 61 | Rotate List | Length + circular link |
| 83 | Remove Duplicates (Sorted) | Skip duplicates |
| 82 | Remove Duplicates II (all gone) | Dummy + outer skip |
| 369 | Plus One Linked List | Find rightmost non-9 |
| 146 | LRU Cache | Doubly LL + HashMap |

---

# 📋 Master Java Syntax Sheet — Linked List

```java
// ── NODE CREATION ──────────────────────────────────────────────
ListNode node = new ListNode(val);
ListNode node = new ListNode(val, nextNode);

// ── TRAVERSE ──────────────────────────────────────────────────
for (ListNode c = head; c != null; c = c.next) {
    // use c.val
}

// ── DUMMY NODE (use for all insert/delete problems) ────────────
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode prev = dummy;
// ... modify list using prev ...
return dummy.next; // real head (handles case where head changed)

// ── NULL SAFETY (memorize these checks) ───────────────────────
if (fast != null && fast.next != null)         // before fast.next.next
if (curr != null && curr.next != null)         // before curr.next.val
if (prev.next != null)                         // before prev.next.val

// ── SLOW / FAST (middle + cycle) ──────────────────────────────
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
// slow = middle (second middle if even)

// ── 3-POINTER REVERSAL ────────────────────────────────────────
ListNode prev = null, curr = head;
while (curr != null) {
    ListNode next = curr.next; // save
    curr.next = prev;          // flip
    prev = curr;               // advance prev
    curr = next;               // advance curr
}
// prev = new head

// ── FIXED-GAP TWO POINTER (remove nth from end) ───────────────
ListNode dummy = new ListNode(0, head);
ListNode fast = dummy, slow = dummy;
for (int i = 0; i <= n; i++) fast = fast.next;
while (fast != null) { slow = slow.next; fast = fast.next; }
slow.next = slow.next.next;

// ── MERGE TWO SORTED ──────────────────────────────────────────
ListNode dummy = new ListNode(0), curr = dummy;
while (l1 != null && l2 != null) {
    if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
    else { curr.next = l2; l2 = l2.next; }
    curr = curr.next;
}
curr.next = (l1 != null) ? l1 : l2;

// ── FIND LENGTH ───────────────────────────────────────────────
int len = 0;
for (ListNode c = head; c != null; c = c.next) len++;

// ── FIND TAIL ─────────────────────────────────────────────────
ListNode tail = head;
while (tail.next != null) tail = tail.next;

// ── CONNECT LISTS ──────────────────────────────────────────────
tail.next = otherHead;        // append other list
node.next = null;             // terminate list

// ── SPLIT LIST AT MIDPOINT ────────────────────────────────────
// Find middle first, then:
ListNode secondHalf = mid.next;
mid.next = null;              // cut into two halves

// ── PRIORITY QUEUE FOR MERGE K ────────────────────────────────
PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);
for (ListNode h : lists) if (h != null) pq.offer(h);
while (!pq.isEmpty()) {
    ListNode node = pq.poll();
    curr.next = node; curr = curr.next;
    if (node.next != null) pq.offer(node.next);
}

// ── DOUBLY LINKED LIST OPERATIONS ─────────────────────────────
// Insert after dummy head:
node.next = head.next;
node.prev = head;
head.next.prev = node;
head.next = node;

// Remove a node:
node.prev.next = node.next;
node.next.prev = node.prev;
```

---

# 🧭 Pattern Decision Table

```
SIGNAL IN PROBLEM                        → PATTERN
──────────────────────────────────────────────────────────────────
"Find middle of list"                    → Slow/fast pointer
"Detect cycle"                           → Floyd's slow/fast
"Find where cycle starts"                → Floyd's + reset to head
"Intersection of two lists"              → Swap list traversal
"Remove Nth from end"                    → Fixed-gap (fast N+1 ahead)
"Reverse the whole list"                 → 3-pointer: prev/curr/next
"Reverse from left to right"             → Dummy + segment reversal
"Reverse every K nodes"                  → Recursive group reversal
"Swap adjacent pairs"                    → Pair pointer rewire
"Odd-indexed first, even-indexed after"  → Separate odd/even chains
"Reorder L0→Ln→L1→Ln-1..."              → Middle + reverse + merge
"Is palindrome?"                         → Middle + reverse + compare
"Merge multiple sorted lists"            → MinHeap of size K
"Sort the list"                          → Merge sort (split + merge)
"Add two numbers as lists"               → Digit-by-digit with carry
"Partition around value x"               → Two dummy chains (less, greater)
"Copy list with random pointers"         → HashMap original→copy
"Rotate right by k"                      → Length + circular + cut
"Remove ALL duplicates from sorted"      → Dummy + skip whole group
"LRU / LFU cache"                        → Doubly LL + HashMap
"Delete middle node"                     → Slow/fast + track prev
```

---

# 📚 All BYTS Problems by Phase

**Phase 2 (Basic Operations):** 19, 21, 83, 876, 2095

**Phase 3 (Two Pointers):** 19, 141, 142, 160, 234, 876

**Phase 4 (Reversal):** 24, 25, 92, 143, 206, 234, 328

**Phase 5 (Merge & Sort):** 2, 21, 23, 86, 88, 148

**Phase 6 (Advanced):** 61, 82, 83, 138, 146, 369

---

## Full BYTS Linked List Problem List

| # | Problem | Pattern | Difficulty |
|---|---------|---------|------------|
| 2 | Add Two Numbers | Digit carry | Med |
| 19 | Remove Nth Node From End | Fixed-gap pointer | Med |
| 21 | Merge Two Sorted Lists | Dummy + compare | Easy |
| 23 | Merge K Sorted Lists | MinHeap | Hard |
| 24 | Swap Nodes in Pairs | Pair rewire | Med |
| 25 | Reverse Nodes in k-Group | Group reversal | Hard |
| 61 | Rotate List | Length + circular | Med |
| 82 | Remove Duplicates II (all) | Dummy + skip group | Med |
| 83 | Remove Duplicates (sorted) | Traverse + skip | Easy |
| 86 | Partition List | Two dummy chains | Med |
| 88 | Merge Sorted Array | Two pointer | Easy |
| 92 | Reverse Linked List II | Partial reversal | Med |
| 138 | Copy List with Random Ptr | HashMap / interleave | Med |
| 141 | Linked List Cycle | Floyd's detect | Easy |
| 142 | Linked List Cycle II | Floyd's start | Med |
| 143 | Reorder List | Mid+rev+merge | Med |
| 146 | LRU Cache | Doubly LL + HashMap | Med |
| 148 | Sort List | Merge sort | Med |
| 160 | Intersection of Two Lists | Swap traversal | Easy |
| 206 | Reverse Linked List | 3-pointer | Easy |
| 234 | Palindrome Linked List | Mid+rev+compare | Easy |
| 287 | Find the Duplicate Number | Floyd's cycle | Med |
| 328 | Odd Even Linked List | Group by parity | Med |
| 369 | Plus One Linked List | Rightmost non-9 | Med |
| 876 | Middle of the Linked List | Slow/fast middle | Easy |
| 2095 | Delete the Middle Node | Slow/fast + prev | Med |

---

*Updated: 2026-06-18 | Java | BYTS Sheet*
*All linked list patterns: Traversal · Two Pointers · Reversal · Merge/Sort · Floyd's · LRU*
