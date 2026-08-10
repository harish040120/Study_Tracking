# Java DSA Notes: Stack, Queue, Trie, LRU Cache

---

## 1. Stack

### Concept
A **Stack** is a linear data structure that follows **LIFO** (Last In, First Out) order. The last element inserted is the first one removed.

Core operations:
- `push(x)` — insert element on top — O(1)
- `pop()` — remove and return top element — O(1)
- `peek()/top()` — view top element without removing — O(1)
- `isEmpty()` — check if stack has no elements — O(1)

Real-world uses: function call stack, undo/redo, expression evaluation, backtracking (maze, DFS), balanced parentheses checking, browser history.

---

### 1.1 Stack using Array

**Idea:** Maintain a fixed-size array and a `top` pointer/index. `push` increments `top` then inserts; `pop` returns element then decrements `top`.

```java
class StackArray {
    private int[] arr;
    private int top;
    private int capacity;

    public StackArray(int capacity) {
        this.capacity = capacity;
        arr = new int[capacity];
        top = -1; // stack empty
    }

    public void push(int x) {
        if (isFull()) {
            throw new RuntimeException("Stack Overflow");
        }
        arr[++top] = x;
    }

    public int pop() {
        if (isEmpty()) {
            throw new RuntimeException("Stack Underflow");
        }
        return arr[top--];
    }

    public int peek() {
        if (isEmpty()) {
            throw new RuntimeException("Stack is empty");
        }
        return arr[top];
    }

    public boolean isEmpty() {
        return top == -1;
    }

    public boolean isFull() {
        return top == capacity - 1;
    }

    public int size() {
        return top + 1;
    }
}
```

**Trade-offs:** O(1) operations, but size is fixed (unless you implement dynamic resizing by doubling the array like `ArrayList` does).

---

### 1.2 Stack using Linked List

**Idea:** Use a singly linked list; insert/remove only from the **head**, so both operations are O(1) with no size limit.

```java
class StackLinkedList {
    private static class Node {
        int data;
        Node next;
        Node(int data) { this.data = data; }
    }

    private Node head; // top of stack
    private int size;

    public void push(int x) {
        Node newNode = new Node(x);
        newNode.next = head;
        head = newNode;
        size++;
    }

    public int pop() {
        if (isEmpty()) throw new RuntimeException("Stack Underflow");
        int val = head.data;
        head = head.next;
        size--;
        return val;
    }

    public int peek() {
        if (isEmpty()) throw new RuntimeException("Stack is empty");
        return head.data;
    }

    public boolean isEmpty() {
        return head == null;
    }

    public int size() {
        return size;
    }
}
```

**Array vs Linked List Stack:**
| Aspect | Array | Linked List |
|---|---|---|
| Memory | Contiguous, less overhead | Extra pointer per node |
| Size | Fixed / needs resizing | Dynamic |
| Cache performance | Better | Worse |

---

### 1.3 Important Stack Problems

**a) Balanced Parentheses**
Concept: push opening brackets; on closing bracket, check if it matches the top; stack should be empty at the end.

```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    Map<Character, Character> map = Map.of(')', '(', ']', '[', '}', '{');
    for (char c : s.toCharArray()) {
        if (map.containsValue(c)) {
            stack.push(c);
        } else if (map.containsKey(c)) {
            if (stack.isEmpty() || stack.pop() != map.get(c)) return false;
        }
    }
    return stack.isEmpty();
}
```

**b) Next Greater Element** (Monotonic Stack)
Concept: Traverse array, maintain a stack of indices whose "next greater" hasn't been found. While current element is greater than the element at stack's top index, pop and record answer.

```java
public int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    Arrays.fill(result, -1);
    Deque<Integer> stack = new ArrayDeque<>(); // stores indices

    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
            result[stack.pop()] = nums[i];
        }
        stack.push(i);
    }
    return result;
}
```

**c) Min Stack (get minimum in O(1))**
Concept: Use an auxiliary stack that tracks the minimum at each level.

```java
class MinStack {
    private Deque<Integer> stack = new ArrayDeque<>();
    private Deque<Integer> minStack = new ArrayDeque<>();

    public void push(int val) {
        stack.push(val);
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        } else {
            minStack.push(minStack.peek());
        }
    }

    public void pop() {
        stack.pop();
        minStack.pop();
    }

    public int top() { return stack.peek(); }
    public int getMin() { return minStack.peek(); }
}
```

**d) Evaluate Postfix Expression**
Concept: Push numbers; when an operator is found, pop two numbers, apply operator, push result back.

```java
public int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String token : tokens) {
        switch (token) {
            case "+": stack.push(stack.pop() + stack.pop()); break;
            case "*": stack.push(stack.pop() * stack.pop()); break;
            case "-": {
                int b = stack.pop(), a = stack.pop();
                stack.push(a - b);
                break;
            }
            case "/": {
                int b = stack.pop(), a = stack.pop();
                stack.push(a / b);
                break;
            }
            default: stack.push(Integer.parseInt(token));
        }
    }
    return stack.pop();
}
```

---

## 2. Queue

### Concept
A **Queue** follows **FIFO** (First In, First Out) order — the first element added is the first removed.

Core operations:
- `enqueue(x)` — insert at the rear — O(1)
- `dequeue()` — remove from the front — O(1)
- `front()/peek()` — view front element — O(1)
- `isEmpty()` — O(1)

Real-world uses: task scheduling, BFS traversal, print queue, request handling (rate limiting), producer-consumer systems.

---

### 2.1 Queue using Array (Circular Queue)

**Idea:** A plain array queue wastes space after dequeues shift the front. A **circular array** reuses freed slots using modulo arithmetic.

```java
class QueueArray {
    private int[] arr;
    private int front, rear, size, capacity;

    public QueueArray(int capacity) {
        this.capacity = capacity;
        arr = new int[capacity];
        front = 0;
        rear = -1;
        size = 0;
    }

    public void enqueue(int x) {
        if (isFull()) throw new RuntimeException("Queue is full");
        rear = (rear + 1) % capacity;
        arr[rear] = x;
        size++;
    }

    public int dequeue() {
        if (isEmpty()) throw new RuntimeException("Queue is empty");
        int val = arr[front];
        front = (front + 1) % capacity;
        size--;
        return val;
    }

    public int peek() {
        if (isEmpty()) throw new RuntimeException("Queue is empty");
        return arr[front];
    }

    public boolean isEmpty() { return size == 0; }
    public boolean isFull() { return size == capacity; }
    public int size() { return size; }
}
```

**Why circular?** Without wraparound (`% capacity`), `front` keeps advancing and array space to the left is never reused, wasting memory even though the queue may be logically not full.

---

### 2.2 Queue using Linked List

**Idea:** Maintain `head` (front) and `tail` (rear) pointers. Enqueue adds at tail, dequeue removes from head — both O(1), no fixed capacity.

```java
class QueueLinkedList {
    private static class Node {
        int data;
        Node next;
        Node(int data) { this.data = data; }
    }

    private Node head, tail;
    private int size;

    public void enqueue(int x) {
        Node newNode = new Node(x);
        if (tail == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            tail = newNode;
        }
        size++;
    }

    public int dequeue() {
        if (isEmpty()) throw new RuntimeException("Queue is empty");
        int val = head.data;
        head = head.next;
        if (head == null) tail = null; // queue became empty
        size--;
        return val;
    }

    public int peek() {
        if (isEmpty()) throw new RuntimeException("Queue is empty");
        return head.data;
    }

    public boolean isEmpty() { return head == null; }
    public int size() { return size; }
}
```

**Array vs Linked List Queue:**
| Aspect | Array (circular) | Linked List |
|---|---|---|
| Memory | Contiguous | Extra pointer overhead |
| Capacity | Fixed | Dynamic |
| Implementation | Slightly trickier (wraparound) | Simple with head/tail |

---

### 2.3 Important Queue Problems

**a) Implement Stack using Queues**
Concept: Use two queues (or one queue with rotation) so that the last-inserted element is always at front.

```java
class StackUsingQueue {
    private Deque<Integer> q = new ArrayDeque<>();

    public void push(int x) {
        q.addLast(x);
        // rotate so newest element is at the front
        for (int i = 0; i < q.size() - 1; i++) {
            q.addLast(q.pollFirst());
        }
    }

    public int pop() { return q.pollFirst(); }
    public int top() { return q.peekFirst(); }
    public boolean isEmpty() { return q.isEmpty(); }
}
```

**b) Implement Queue using Stacks**
Concept: Use two stacks — `inStack` for enqueue, `outStack` for dequeue. Transfer elements only when `outStack` is empty (amortized O(1)).

```java
class QueueUsingStacks {
    private Deque<Integer> inStack = new ArrayDeque<>();
    private Deque<Integer> outStack = new ArrayDeque<>();

    public void enqueue(int x) {
        inStack.push(x);
    }

    public int dequeue() {
        if (outStack.isEmpty()) {
            while (!inStack.isEmpty()) {
                outStack.push(inStack.pop());
            }
        }
        return outStack.pop();
    }
}
```

**c) Sliding Window Maximum** (Monotonic Deque)
Concept: Maintain a deque of indices in decreasing order of value. Front always holds the max of the current window.

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> deque = new ArrayDeque<>(); // stores indices

    for (int i = 0; i < n; i++) {
        // remove indices out of window
        if (!deque.isEmpty() && deque.peekFirst() <= i - k) {
            deque.pollFirst();
        }
        // remove smaller elements from back
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        deque.offerLast(i);
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }
    return result;
}
```

**d) BFS Traversal using Queue**
Concept: Standard graph/tree traversal, level by level, using a queue to hold nodes to visit next.

```java
public void bfs(int start, List<List<Integer>> adj, int n) {
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    visited[start] = true;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        System.out.print(node + " ");
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.offer(neighbor);
            }
        }
    }
}
```

---

## 3. Trie (Prefix Tree)

### Concept
A **Trie** is a tree-like data structure used to store a dynamic set of strings, where each node represents a character. Common prefixes share the same path from the root, which makes prefix-based operations very efficient.

- Insert / Search: O(L) where L = length of the word
- Great for: autocomplete, spell checkers, prefix matching, dictionary lookups, IP routing (bitwise tries)

Each node typically has:
- An array/map of children (e.g., 26 slots for lowercase English letters)
- A boolean flag marking end of a valid word

---

### 3.1 Trie Class Implementation

```java
class TrieNode {
    TrieNode[] children;
    boolean isEndOfWord;

    public TrieNode() {
        children = new TrieNode[26]; // for 'a' to 'z'
        isEndOfWord = false;
    }
}

class Trie {
    private final TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Insert a word into the trie - O(L)
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new TrieNode();
            }
            node = node.children[idx];
        }
        node.isEndOfWord = true;
    }

    // Search exact word - O(L)
    public boolean search(String word) {
        TrieNode node = findNode(word);
        return node != null && node.isEndOfWord;
    }

    // Check if any word starts with given prefix - O(L)
    public boolean startsWith(String prefix) {
        return findNode(prefix) != null;
    }

    private TrieNode findNode(String str) {
        TrieNode node = root;
        for (char c : str.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return null;
            node = node.children[idx];
        }
        return node;
    }

    // Delete a word from the trie
    public void delete(String word) {
        delete(root, word, 0);
    }

    private boolean delete(TrieNode node, String word, int depth) {
        if (node == null) return false;

        if (depth == word.length()) {
            if (!node.isEndOfWord) return false;
            node.isEndOfWord = false;
            return isEmptyNode(node); // true if node has no children -> can be deleted
        }

        int idx = word.charAt(depth) - 'a';
        boolean shouldDeleteChild = delete(node.children[idx], word, depth + 1);

        if (shouldDeleteChild) {
            node.children[idx] = null;
            return !node.isEndOfWord && isEmptyNode(node);
        }
        return false;
    }

    private boolean isEmptyNode(TrieNode node) {
        for (TrieNode child : node.children) {
            if (child != null) return false;
        }
        return true;
    }
}
```

---

### 3.2 Important Trie Problems

**a) Implement Trie (basic) — shown above.**

**b) Word Search II / Count words with given prefix**
Concept: Add a `wordCount`/`prefixCount` field on each node to count how many words pass through it, enabling O(L) prefix-count queries.

```java
class TrieNodeCount {
    TrieNodeCount[] children = new TrieNodeCount[26];
    int prefixCount = 0; // words passing through this node
    int wordCount = 0;   // words ending exactly here
}

class TrieWithCount {
    private TrieNodeCount root = new TrieNodeCount();

    public void insert(String word) {
        TrieNodeCount node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) node.children[idx] = new TrieNodeCount();
            node = node.children[idx];
            node.prefixCount++;
        }
        node.wordCount++;
    }

    public int countWordsEqualTo(String word) {
        TrieNodeCount node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return 0;
            node = node.children[idx];
        }
        return node.wordCount;
    }

    public int countWordsStartingWith(String prefix) {
        TrieNodeCount node = root;
        for (char c : prefix.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return 0;
            node = node.children[idx];
        }
        return node.prefixCount;
    }
}
```

**c) Longest Word with All Prefixes Present**
Concept: A word is "buildable" if every prefix of it is also a complete word in the trie. DFS through the trie, only continuing into children that are marked `isEndOfWord`.

```java
public String longestWord(String[] words) {
    Trie trie = new Trie();
    for (String w : words) trie.insert(w);

    String[] result = {""};
    dfs(trie, "", result);
    return result[0];
}

// Simplified logic (using the earlier Trie's package-private access, or expose root)
```

**d) Word Break Problem using Trie**
Concept: Use the trie to check if a string can be segmented into dictionary words, combined with DP for memoization.

```java
public boolean wordBreak(String s, List<String> wordDict) {
    Trie trie = new Trie();
    for (String w : wordDict) trie.insert(w);

    int n = s.length();
    boolean[] dp = new boolean[n + 1];
    dp[0] = true;

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && trie.search(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[n];
}
```

**e) Auto-complete / Print all words with given prefix**
Concept: Traverse trie to the prefix's end node, then DFS all paths below it, collecting complete words.

```java
public List<String> autocomplete(Trie trie, TrieNode root, String prefix) {
    List<String> results = new ArrayList<>();
    TrieNode node = root;
    for (char c : prefix.toCharArray()) {
        int idx = c - 'a';
        if (node.children[idx] == null) return results; // no matches
        node = node.children[idx];
    }
    collectWords(node, new StringBuilder(prefix), results);
    return results;
}

private void collectWords(TrieNode node, StringBuilder path, List<String> results) {
    if (node.isEndOfWord) results.add(path.toString());
    for (char c = 'a'; c <= 'z'; c++) {
        TrieNode child = node.children[c - 'a'];
        if (child != null) {
            path.append(c);
            collectWords(child, path, results);
            path.deleteCharAt(path.length() - 1);
        }
    }
}
```

---

## 4. LRU Cache (Least Recently Used)

### Concept
An **LRU Cache** evicts the **least recently used** item when it reaches capacity and a new item needs to be inserted. Both `get` and `put` must run in **O(1)**.

**Key design insight:** Combine two data structures:
1. **HashMap** — for O(1) lookup of a key's location.
2. **Doubly Linked List** — to maintain usage order. Most-recently-used items near the head, least-recently-used near the tail. Doubly linked (not singly) so a node can be removed from the middle in O(1) without traversal.

On `get(key)`: if key exists, move its node to the head (mark as most recently used), return its value.
On `put(key, value)`: if key exists, update value and move to head. If not, insert new node at head; if capacity exceeded, remove the tail node (least recently used) and delete it from the map too.

---

### 4.1 LRU Cache Implementation (from scratch)

```java
class LRUCache {
    private class Node {
        int key, value;
        Node prev, next;
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private final int capacity;
    private final Map<Integer, Node> map;
    private final Node head, tail; // dummy sentinel nodes

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        Node node = map.get(key);
        remove(node);
        insertAtHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node existing = map.get(key);
            remove(existing);
            map.remove(key);
        }
        if (map.size() == capacity) {
            Node lru = tail.prev; // least recently used
            remove(lru);
            map.remove(lru.key);
        }
        Node newNode = new Node(key, value);
        insertAtHead(newNode);
        map.put(key, newNode);
    }

    // Removes a node from its current position in the list
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    // Inserts a node right after the head (most recently used position)
    private void insertAtHead(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}
```

**Why sentinel (dummy) head/tail nodes?** They eliminate null checks when inserting/removing at the boundaries, simplifying the code.

**Why not just use `LinkedHashMap`?** Java's built-in `LinkedHashMap` can implement LRU cache trivially by overriding `removeEldestEntry`, but implementing from scratch (as above) is what's usually expected in interviews since it tests understanding of HashMap + Doubly Linked List combination.

---

### 4.2 LRU Cache using LinkedHashMap (shortcut / alternative)

```java
class LRUCacheBuiltIn extends LinkedHashMap<Integer, Integer> {
    private final int capacity;

    public LRUCacheBuiltIn(int capacity) {
        super(capacity, 0.75f, true); // accessOrder = true -> recently accessed entries move to the end
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; // auto-evict oldest when over capacity
    }
}
```

---

### 4.3 Important LRU-related Problems

**a) LFU Cache (Least Frequently Used)**
Concept: Similar idea but evicts based on lowest access frequency (not recency). Requires a frequency map + a map of doubly linked lists per frequency, and tracking the minimum frequency for O(1) eviction. (More advanced — a natural follow-up to LRU.)

**b) Design a Browser History (using Doubly Linked List concepts)**
Concept: Similar node-based navigation — `visit`, `back`, `forward` operations, reusing the doubly linked list traversal idea from LRU.

**c) Design In-Memory File System with caching**
Concept: Combine trie (for path lookups) with LRU eviction for cached file content — a good example of combining multiple data structures from this document.

**d) All O(1) Data Structure (Insert, Remove, GetRandom, IncreaseKey, DecreaseKey in O(1))**
Concept: Extension of the HashMap + Doubly Linked List pattern used in LRU cache, but grouping keys by their current count/value instead of recency.

---

## Quick Summary Table

| Data Structure | Backing Structure | Key Operations | Time Complexity |
|---|---|---|---|
| Stack (Array) | Fixed array + top pointer | push, pop, peek | O(1) |
| Stack (Linked List) | Singly linked list, insert/remove at head | push, pop, peek | O(1) |
| Queue (Array) | Circular array + front/rear pointers | enqueue, dequeue | O(1) |
| Queue (Linked List) | Singly linked list w/ head & tail pointers | enqueue, dequeue | O(1) |
| Trie | Tree of character nodes | insert, search, startsWith | O(L) |
| LRU Cache | HashMap + Doubly Linked List | get, put | O(1) |

---

## Tips for Interviews
- For **Stack/Queue**, know how to implement one using the other — it's a very common interview question.
- For **Trie**, always clarify the character set (lowercase only? alphanumeric?) since it decides array size vs. using a `HashMap<Character, TrieNode>`.
- For **LRU Cache**, always draw the doubly linked list + hashmap diagram before coding — it prevents pointer mistakes.
- Practice tracing through pointer updates on paper/whiteboard for linked-list-based structures; that's where most bugs occur.
