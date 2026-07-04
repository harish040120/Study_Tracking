# 🔄 Recursion — Complete Course
**Language:** Java | **Level:** Beginner → SDE Interview Ready
**Style:** Concept → ASCII Diagram → Mental Model → Java Code → BYTS Problems

---

# 📌 Contents

**Phase 1 — Recursion Foundations**
- 1.1 What is Recursion?
- 1.2 The Three Laws of Recursion
- 1.3 How the Call Stack Works
- 1.4 Recursion vs Iteration
- 1.5 The Universal Recursion Template
- 1.6 Java Syntax Reference

**Phase 2 — Linear Recursion (One Call)**
- 2.1 Factorial
- 2.2 Sum of N Numbers
- 2.3 Power Function
- 2.4 Reverse a String
- 2.5 Check Palindrome
- 2.6 Count Digits / Sum of Digits

**Phase 3 — Binary Recursion (Two Calls)**
- 3.1 Fibonacci (Naive → Memoized)
- 3.2 Binary Search
- 3.3 Merge Sort
- 3.4 Maximum in Array
- 3.5 Counting Problems

**Phase 4 — Tree Recursion (Trust the Subtree)**
- 4.1 Max Depth of Tree
- 4.2 Invert Binary Tree
- 4.3 Same Tree / Symmetric Tree
- 4.4 Path Sum Problems
- 4.5 Diameter of Binary Tree

**Phase 5 — Divide and Conquer**
- 5.1 The D&C Pattern
- 5.2 Merge Sort (Full)
- 5.3 Quick Sort
- 5.4 Search a 2D Matrix
- 5.5 Pow(x, n)

**Phase 6 — Backtracking (Recursion + Undo)**
- 6.1 What is Backtracking?
- 6.2 Subsets
- 6.3 Permutations
- 6.4 Combination Sum
- 6.5 N-Queens
- 6.6 Word Search

**Phase 7 — Recursion on Linked Lists**
- 7.1 Reverse Linked List Recursively
- 7.2 Merge Two Sorted Lists
- 7.3 Palindrome Check on List

**Phase 8 — Recursion to DP**
- 8.1 Recognizing Overlapping Subproblems
- 8.2 Top-Down Memoization
- 8.3 Climbing Stairs → DP
- 8.4 House Robber → DP
- 8.5 Coin Change → DP

**Master Reference**
- Recursion Pattern Decision Table
- Common Mistakes & How to Avoid
- All BYTS Recursion Problems

---

# 🟢 Phase 1 — Recursion Foundations

---

## 1.1 What is Recursion?

Recursion is when a function **calls itself** to solve a smaller version of the same problem.

Every recursive solution needs:
1. **Base case** — when to STOP calling itself
2. **Recursive case** — call itself with a SMALLER/SIMPLER input

```
Think of Russian Matryoshka dolls:
To open the outermost doll, you need to open the one inside it.
To open that one, you need to open the one inside IT.
...
Eventually you reach the smallest doll (no doll inside) → BASE CASE.
Then you "come back up" with your answer.

[Doll5 [Doll4 [Doll3 [Doll2 [Doll1]]]]]
                                  ↑
                            Base case: smallest doll, nothing inside
```

### The Mental Model — Trust the Recursion

The hardest part of recursion is **trusting that the recursive call works**.

> Assume the function ALREADY WORKS for smaller inputs.
> Your ONLY job is to handle the CURRENT level and combine.

```
factorial(5) = 5 × factorial(4)
                        ↑
               Assume this already returns 24.
               Your job: just multiply 5 × 24 = 120.
               Don't think about HOW factorial(4) works.
```

---

## 1.2 The Three Laws of Recursion

```
LAW 1: A recursive algorithm must have a BASE CASE.
       (The stopping condition — without it, infinite recursion → stack overflow)

LAW 2: A recursive algorithm must CHANGE its state and MOVE TOWARD the base case.
       (Each call makes the problem SMALLER)

LAW 3: A recursive algorithm must CALL ITSELF.
       (This is the definition)
```

---

## 1.3 How the Call Stack Works

Each recursive call creates a NEW **stack frame** — a separate memory space with its own local variables.

```
factorial(4) call sequence:

CALL STACK (grows down):                    RETURN (unwinds up):
┌──────────────────────┐
│ factorial(4)         │  → calls factorial(3)
├──────────────────────┤
│ factorial(3)         │  → calls factorial(2)
├──────────────────────┤
│ factorial(2)         │  → calls factorial(1)
├──────────────────────┤
│ factorial(1)         │  → calls factorial(0)
├──────────────────────┤
│ factorial(0) = 1     │  ← BASE CASE: return 1
└──────────────────────┘

Now unwind:
factorial(1) = 1 × 1 = 1    ← returns to factorial(2)
factorial(2) = 2 × 1 = 2    ← returns to factorial(3)
factorial(3) = 3 × 2 = 6    ← returns to factorial(4)
factorial(4) = 4 × 6 = 24   ← final answer
```

**Stack Overflow** happens when recursion goes too deep (no base case or base case never reached).

```
Java default stack size ≈ 500–1000 frames deep.
For very deep recursion, use ITERATION or MEMOIZATION.
```

---

## 1.4 Recursion vs Iteration

```
TOPIC           RECURSION           ITERATION
────────────────────────────────────────────────
Code length     Usually shorter     Usually longer
Readability     Natural for trees   Natural for arrays
Space           O(N) stack space    O(1) usually
Speed           Slightly slower     Slightly faster
Best for        Trees, DFS, D&C     Loops, arrays
Stack overflow  Risk if deep        No risk
```

**Rule of thumb:**
- Tree/graph problems → recursion almost always cleaner
- Simple loops (sum, count) → iteration is fine
- Both produce same result

---

## 1.5 The Universal Recursion Template

```java
ReturnType solve(parameters) {
    // ── STEP 1: BASE CASE ─────────────────────────
    if (baseCaseCondition) {
        return baseCaseValue;
    }

    // ── STEP 2: MAKE THE PROBLEM SMALLER ──────────
    // Reduce the input (smaller n, substring, subtree)

    // ── STEP 3: RECURSIVE CALL ────────────────────
    ReturnType subResult = solve(smallerParameters);

    // ── STEP 4: COMBINE AND RETURN ────────────────
    return combine(currentValue, subResult);
}
```

### The Four Questions to Ask for ANY Recursive Problem

```
Q1. What is my BASE CASE? (when do I stop?)
Q2. What does my function RETURN? (what is the unit of answer?)
Q3. How do I make the problem SMALLER each call?
Q4. How do I COMBINE the current value with the recursive result?
```

---

## 1.6 Java Syntax Reference

```java
// ── BASIC RECURSIVE CALL ──────────────────────────────────────
int solve(int n) {
    if (n == 0) return 0;           // base case
    return n + solve(n - 1);        // recursive case
}

// ── ACCUMULATOR PATTERN (tail-recursive style) ────────────────
int solve(int n, int accumulator) {
    if (n == 0) return accumulator; // base case: return accumulated result
    return solve(n - 1, accumulator + n); // pass result forward
}

// ── HELPER METHOD PATTERN (very common) ───────────────────────
// Public method that initializes and delegates to helper
public int solve(int[] nums) {
    return helper(nums, 0);         // start from index 0
}

private int helper(int[] nums, int index) {
    if (index == nums.length) return 0; // base case
    return nums[index] + helper(nums, index + 1);
}

// ── STRING RECURSION ──────────────────────────────────────────
String process(String s) {
    if (s.isEmpty()) return "";          // base case
    return process(s.substring(1)) + s.charAt(0); // recursive
}
// s.substring(1) removes first character each call
// s.charAt(0) is the first character

// ── ARRAY RECURSION ───────────────────────────────────────────
int maxInArray(int[] arr, int n) {
    if (n == 1) return arr[0];              // base: one element
    return Math.max(arr[n-1], maxInArray(arr, n-1)); // compare last with rest
}

// ── TREE RECURSION ────────────────────────────────────────────
int height(TreeNode root) {
    if (root == null) return 0;             // base: empty tree
    return 1 + Math.max(height(root.left), height(root.right));
}

// ── GLOBAL VARIABLE (for tracking across calls) ───────────────
int maxDiameter = 0; // declared outside, updated inside

void dfs(TreeNode node) {
    if (node == null) return;
    // update global based on this node's computation
    maxDiameter = Math.max(maxDiameter, leftH + rightH);
}

// ── MEMOIZATION (top-down DP) ─────────────────────────────────
Map<Integer, Integer> memo = new HashMap<>();

int fib(int n) {
    if (n <= 1) return n;
    if (memo.containsKey(n)) return memo.get(n); // cached!
    int result = fib(n-1) + fib(n-2);
    memo.put(n, result);
    return result;
}

// ── BACKTRACKING ──────────────────────────────────────────────
void backtrack(List<Integer> current, int start) {
    result.add(new ArrayList<>(current)); // snapshot!
    for (int i = start; i < nums.length; i++) {
        current.add(nums[i]);             // CHOOSE
        backtrack(current, i + 1);        // EXPLORE
        current.remove(current.size()-1); // UNDO
    }
}
```

---

# 🔵 Phase 2 — Linear Recursion (One Recursive Call)

---

## 2.1 Factorial

The most classic recursion example. One recursive call per function.

```
factorial(5) = 5 × 4 × 3 × 2 × 1 = 120

Recursion tree:
factorial(5)
    └── 5 × factorial(4)
              └── 4 × factorial(3)
                        └── 3 × factorial(2)
                                  └── 2 × factorial(1)
                                            └── 1 × factorial(0) = 1
```

```java
// Q1. Base case? n == 0, return 1
// Q2. Returns? the factorial value (int/long)
// Q3. Smaller? n-1 each call
// Q4. Combine? n × recursive result

long factorial(int n) {
    if (n == 0) return 1;           // base: 0! = 1
    return n * factorial(n - 1);    // n! = n × (n-1)!
}

// With accumulator (avoids deep stack):
long factorialTail(int n, long acc) {
    if (n == 0) return acc;
    return factorialTail(n - 1, n * acc);
}
// Call as: factorialTail(5, 1)
```

---

## 2.2 Sum of N Numbers

```
sum(5) = 5 + 4 + 3 + 2 + 1 + 0 = 15

Call trace:
sum(5) = 5 + sum(4)
              = 5 + (4 + sum(3))
                        = 5 + (4 + (3 + sum(2)))
                                      = 5 + (4 + (3 + (2 + sum(1))))
                                                        = 5+4+3+2+1 = 15
```

```java
int sum(int n) {
    if (n == 0) return 0;       // base
    return n + sum(n - 1);      // n + sum of rest
}

// Sum of array elements
int arraySum(int[] arr, int i) {
    if (i == arr.length) return 0;          // base: past end
    return arr[i] + arraySum(arr, i + 1);   // current + rest
}
// Call: arraySum(arr, 0)
```

---

## 2.3 Power Function — Fast Exponentiation

Naive: `x^n` = multiply x n times → O(N)
Smart: Use Divide and Conquer → O(log N)

```
x^8 = (x^4)² = ((x^2)²)²
      ONLY 3 multiplications instead of 7!

x^n:
  if n is even: x^n = (x^(n/2))²
  if n is odd:  x^n = x × (x^(n/2))²

pow(2, 10):
  pow(2, 5) → need this
    pow(2, 2) → need this
      pow(2, 1) → need this
        pow(2, 0) = 1 ← base
      pow(2,1) = 2×1 = 2
    pow(2,2) = 2^2 = 4... wait, 5 is odd so:
    pow(2,5) = 2 × pow(2,2)^2 = 2 × 16 = 32
  pow(2,10) = pow(2,5)^2 = 32^2 = 1024 ✅
```

```java
// LC 50 — Pow(x, n)
double myPow(double x, int n) {
    if (n == 0) return 1;               // base: anything^0 = 1

    if (n < 0) {
        x = 1 / x;                      // handle negative exponent
        n = -n;
    }

    // Fast power: divide exponent by 2 each call
    if (n % 2 == 0) {
        double half = myPow(x, n / 2);
        return half * half;             // even: square the half result
    } else {
        double half = myPow(x, n / 2);
        return x * half * half;         // odd: one extra x
    }
}

// Cleaner version:
double myPow(double x, long n) {
    if (n == 0) return 1;
    if (n < 0) return myPow(1/x, -n);
    double half = myPow(x, n/2);
    return n % 2 == 0 ? half*half : x*half*half;
}
```

---

## 2.4 Reverse a String

```
reverse("hello") = "olleh"

Call trace:
reverse("hello")
  = reverse("ello") + 'h'
    = reverse("llo") + 'e' + 'h'
      = reverse("lo") + 'l' + 'e' + 'h'
        = reverse("o") + 'l' + 'l' + 'e' + 'h'
          = "o" (base: 1 char)
        = "ol"
      = "oll"
    = "olle"
  = "olleh" ✅
```

```java
String reverse(String s) {
    if (s.length() <= 1) return s;          // base: empty or single char
    return reverse(s.substring(1)) + s.charAt(0);
    // reverse everything AFTER first char, then append first char at end
}

// With array indices (avoids substring allocation):
void reverse(char[] arr, int left, int right) {
    if (left >= right) return;              // base: pointers crossed
    char temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;
    reverse(arr, left + 1, right - 1);     // recurse on inner
}
```

---

## 2.5 Check Palindrome Recursively

```
isPalindrome("racecar"):
  'r' == 'r' ✓, check isPalindrome("aceca")
    'a' == 'a' ✓, check isPalindrome("cec")
      'c' == 'c' ✓, check isPalindrome("e")
        Length 1 → BASE CASE: return true ✓
```

```java
boolean isPalindrome(String s, int left, int right) {
    if (left >= right) return true;         // base: valid palindrome
    if (s.charAt(left) != s.charAt(right)) return false; // mismatch
    return isPalindrome(s, left + 1, right - 1); // check inner
}
// Call: isPalindrome(s, 0, s.length()-1)
```

---

## 2.6 Sum of Digits / Count Digits

```java
// Sum of digits: 1234 → 1+2+3+4 = 10
int sumOfDigits(int n) {
    if (n == 0) return 0;
    return (n % 10) + sumOfDigits(n / 10);  // last digit + rest
}

// Count digits: 1234 → 4
int countDigits(int n) {
    if (n == 0) return 0;
    return 1 + countDigits(n / 10);         // 1 for this digit + rest
}

// Check if number is palindrome: 121 → true
boolean isPalindromeNum(int n) {
    String s = String.valueOf(n);
    return isPalindrome(s, 0, s.length() - 1);
}
```

### BYTS Problems — Phase 2

| # | Problem | Pattern |
|---|---------|---------|
| 344 | Reverse String | Two-pointer recursive |
| 9 | Palindrome Number | String recursive |
| 50 | Pow(x, n) | Fast exponentiation |
| 509 | Fibonacci Number | Linear recursion |
| 231 | Power of Two | Divide by 2 recursion |
| 342 | Power of Four | Divide recursion |

---

# 🟡 Phase 3 — Binary Recursion (Two Recursive Calls)

---

## 3.1 Fibonacci — Naive → Memoized

### Naive Fibonacci (Exponential — O(2^N))

```
fib(5) tree — notice the REPEATED WORK:

                     fib(5)
                   /        \
               fib(4)        fib(3)
              /      \       /     \
          fib(3)   fib(2) fib(2)  fib(1)
          /    \    /  \
       fib(2) fib(1) fib(1) fib(0)
       /    \
    fib(1) fib(0)

fib(3) computed TWICE
fib(2) computed THREE TIMES
→ O(2^N) time, terrible for large N
```

```java
// Naive — DO NOT use for large N
int fib(int n) {
    if (n <= 1) return n;           // base: fib(0)=0, fib(1)=1
    return fib(n-1) + fib(n-2);    // two recursive calls
}
```

### Memoized Fibonacci (O(N))

```java
Map<Integer, Integer> memo = new HashMap<>();

int fib(int n) {
    if (n <= 1) return n;
    if (memo.containsKey(n)) return memo.get(n); // ← CACHED!
    int result = fib(n-1) + fib(n-2);
    memo.put(n, result);            // ← STORE before returning
    return result;
}

// Now each subproblem computed ONLY ONCE → O(N) time
```

---

## 3.2 Binary Search (Recursive)

```
Search 7 in [1,3,5,7,9,11,13]:

          [1,3,5,7,9,11,13]
           mid = 7, found! ✅

Search 5 in [1,3,5,7,9,11,13]:
          [1,3,5,7,9,11,13]  mid=7, 5<7 → go left
          [1,3,5]            mid=3, 5>3 → go right
          [5]                mid=5, found! ✅

Each call HALVES the array → O(log N)
```

```java
int binarySearch(int[] nums, int target, int left, int right) {
    if (left > right) return -1;            // base: not found

    int mid = left + (right - left) / 2;   // avoid overflow

    if (nums[mid] == target) return mid;    // base: found!
    if (nums[mid] < target) {
        return binarySearch(nums, target, mid + 1, right); // go right
    } else {
        return binarySearch(nums, target, left, mid - 1);  // go left
    }
}
// Call: binarySearch(nums, target, 0, nums.length-1)
```

---

## 3.3 Merge Sort (Recursive)

```
Recursion tree for [4,2,1,3]:

                [4,2,1,3]
               /          \
          [4,2]            [1,3]
         /     \          /     \
       [4]     [2]      [1]     [3]
        \       /        \       /
          [2,4]            [1,3]
               \          /
                [1,2,3,4]
```

```java
void mergeSort(int[] arr, int left, int right) {
    if (left >= right) return;              // base: one element, already sorted

    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);              // sort left half
    mergeSort(arr, mid + 1, right);         // sort right half
    merge(arr, left, mid, right);           // merge two sorted halves
}

void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) temp[k++] = arr[i++];
        else temp[k++] = arr[j++];
    }
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];

    for (int l = 0; l < temp.length; l++) arr[left + l] = temp[l];
}
```

---

## 3.4 Maximum in Array

```
max([3,1,4,1,5,9]):
  max(3, max([1,4,1,5,9]))
         max(1, max([4,1,5,9]))
                max(4, max([1,5,9]))
                       max(1, max([5,9]))
                              max(5, max([9]))
                                     max(9, max([]))
                                            = 9 ← base
```

```java
int maxInArray(int[] arr, int i) {
    if (i == arr.length - 1) return arr[i]; // base: last element
    return Math.max(arr[i], maxInArray(arr, i + 1));
}
// Call: maxInArray(arr, 0)
```

---

## 3.5 Counting Problems

```java
// Count occurrences of x in array
int countOccurrences(int[] arr, int x, int i) {
    if (i == arr.length) return 0;
    int found = (arr[i] == x) ? 1 : 0;
    return found + countOccurrences(arr, x, i + 1);
}

// Count ways to climb stairs (1 or 2 steps at a time)
int climbWays(int n) {
    if (n == 0) return 1;   // one way: do nothing
    if (n < 0) return 0;    // impossible
    return climbWays(n-1) + climbWays(n-2); // take 1 step OR 2 steps
}
```

### BYTS Problems — Phase 3

| # | Problem | Pattern |
|---|---------|---------|
| 509 | Fibonacci Number | Two-call recursion |
| 70 | Climbing Stairs | Two-call + memo |
| 704 | Binary Search | Recursive binary search |
| 148 | Sort List | Merge sort recursive |
| 215 | Kth Largest Element | Quick select recursive |
| 241 | Different Ways to Add Parentheses | D&C two calls |

---

# 🟠 Phase 4 — Tree Recursion (Trust the Subtree)

---

## 4.1 Max Depth of Binary Tree (LC 104)

### The Mental Model

```
       1
      / \
     2   3
    / \
   4   5

Q: What is the height of this tree?
A: 1 (root) + max(height of left subtree, height of right subtree)
   = 1 + max(height(2's subtree), height(3))
   = 1 + max(2, 1)
   = 3 ✅

DON'T think about the whole tree.
Just answer: "given what LEFT subtree returns and RIGHT subtree returns,
              what do I return?"
```

```java
int maxDepth(TreeNode root) {
    // Q1. Base case? null node has depth 0
    if (root == null) return 0;

    // Q3. Make smaller? call on left and right children
    int leftDepth  = maxDepth(root.left);
    int rightDepth = maxDepth(root.right);

    // Q4. Combine? current node adds 1, take the larger child depth
    return 1 + Math.max(leftDepth, rightDepth);
}
```

---

## 4.2 Invert Binary Tree (LC 226)

```
Before:        After:
    4               4
   / \             / \
  2   7    →      7   2
 / \ / \         / \ / \
1  3 6  9       9  6 3  1
```

```java
TreeNode invertTree(TreeNode root) {
    if (root == null) return null;  // base: empty tree

    // Trust: invertTree already works on subtrees
    TreeNode left  = invertTree(root.left);
    TreeNode right = invertTree(root.right);

    // Combine: swap the two results
    root.left  = right;
    root.right = left;

    return root;
}
```

---

## 4.3 Same Tree / Symmetric Tree (LC 100, 101)

```java
// Same Tree
boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;   // both empty → same
    if (p == null || q == null) return false;  // one empty → different
    if (p.val != q.val) return false;          // values differ → different

    return isSameTree(p.left, q.left)
        && isSameTree(p.right, q.right);
}

// Symmetric Tree — is it a mirror of itself?
boolean isSymmetric(TreeNode root) {
    return isMirror(root, root);
}

boolean isMirror(TreeNode l, TreeNode r) {
    if (l == null && r == null) return true;
    if (l == null || r == null) return false;
    return l.val == r.val
        && isMirror(l.left, r.right)    // outer pair
        && isMirror(l.right, r.left);   // inner pair
}
```

---

## 4.4 Path Sum (LC 112, 113)

```java
// LC 112 — Does any root-to-leaf path sum to target?
boolean hasPathSum(TreeNode root, int target) {
    if (root == null) return false;

    // Leaf node: check if remaining sum equals this node's value
    if (root.left == null && root.right == null) {
        return target == root.val;
    }

    // Subtract current value and check either child
    return hasPathSum(root.left, target - root.val)
        || hasPathSum(root.right, target - root.val);
}

// LC 113 — Find ALL root-to-leaf paths with target sum
List<List<Integer>> result = new ArrayList<>();

void pathSumII(TreeNode node, int target, List<Integer> path) {
    if (node == null) return;

    path.add(node.val);         // CHOOSE: add to path

    if (node.left == null && node.right == null && target == node.val) {
        result.add(new ArrayList<>(path)); // SNAPSHOT!
    } else {
        pathSumII(node.left,  target - node.val, path);
        pathSumII(node.right, target - node.val, path);
    }

    path.remove(path.size() - 1); // UNDO: remove from path
}
```

---

## 4.5 Diameter of Binary Tree (LC 543)

### The Key Pattern — Return vs Update Global

```
       1
      / \
     2   3
    / \
   4   5

At node 2: leftHeight=1, rightHeight=1
  diameter through 2 = 1 + 1 = 2 (path: 4-2-5)
At node 1: leftHeight=2, rightHeight=1
  diameter through 1 = 2 + 1 = 3 (path: 4-2-1-3 or 5-2-1-3)

CRITICAL INSIGHT:
  The function RETURNS height (to be used by parent).
  But it UPDATES a global variable with diameter.
  Two different things from the same recursion!
```

```java
int diameter = 0;

int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return diameter;
}

int height(TreeNode node) {
    if (node == null) return 0;

    int leftH  = height(node.left);
    int rightH = height(node.right);

    // Update global: longest path THROUGH this node
    diameter = Math.max(diameter, leftH + rightH);

    // Return: height of this node (for parent's use)
    return 1 + Math.max(leftH, rightH);
}
```

### BYTS Problems — Phase 4

| # | Problem | Pattern |
|---|---------|---------|
| 104 | Maximum Depth of Binary Tree | Return from children |
| 111 | Minimum Depth | Base case with null child |
| 226 | Invert Binary Tree | Swap and return |
| 100 | Same Tree | Compare both sides |
| 101 | Symmetric Tree | Mirror comparison |
| 572 | Subtree of Another Tree | isSameTree at each node |
| 112 | Path Sum | Subtract and check leaf |
| 113 | Path Sum II | Backtrack path list |
| 543 | Diameter of Binary Tree | Return height, update global |
| 110 | Balanced Binary Tree | Return -1 sentinel |
| 124 | Binary Tree Maximum Path Sum | Max gain pattern |
| 257 | Binary Tree Paths | Build path string |

---

# 🔴 Phase 5 — Divide and Conquer

---

## 5.1 The D&C Pattern

```
DIVIDE:   Split the problem into subproblems of SAME TYPE
CONQUER:  Solve each subproblem recursively (trust recursion)
COMBINE:  Merge the results of subproblems

Template:
solve(problem):
    if problem is small enough: SOLVE DIRECTLY (base case)
    left  = solve(left half of problem)
    right = solve(right half of problem)
    return MERGE(left, right)
```

```
Classic D&C algorithms:
  Merge Sort     → split array, sort each, merge
  Quick Sort     → partition, sort each partition
  Binary Search  → check mid, recurse on one half
  Pow(x,n)       → compute half power, square it
  Closest Pair   → split points, find closest in each half
```

---

## 5.2 Merge Sort — Full Implementation

```java
int[] sortArray(int[] nums) {
    mergeSort(nums, 0, nums.length - 1);
    return nums;
}

void mergeSort(int[] arr, int left, int right) {
    if (left >= right) return;          // base: 0 or 1 element

    int mid = left + (right - left) / 2;

    mergeSort(arr, left, mid);          // DIVIDE and CONQUER left
    mergeSort(arr, mid + 1, right);     // DIVIDE and CONQUER right
    merge(arr, left, mid, right);       // COMBINE (merge sorted halves)
}

void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) temp[k++] = arr[i++];
        else temp[k++] = arr[j++];
    }
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];

    System.arraycopy(temp, 0, arr, left, temp.length);
}
```

---

## 5.3 Quick Sort

```
Partition around pivot:
[3,6,8,10,1,2,1] pivot=1
→ [1,1,2,3,6,8,10] → pivot in correct position

After partition: everything LEFT of pivot < pivot,
                 everything RIGHT of pivot > pivot
Recurse on each side.
```

```java
void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pivot = partition(arr, low, high);
        quickSort(arr, low, pivot - 1);  // sort left
        quickSort(arr, pivot + 1, high); // sort right
    }
}

int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;
        }
    }
    int temp = arr[i+1]; arr[i+1] = arr[high]; arr[high] = temp;
    return i + 1;
}
```

---

## 5.4 Different Ways to Add Parentheses (LC 241)

Classic D&C on expressions.

```
"2-1-1"

Try each operator as the LAST operation:
  Split at '-' (index 1): "2" and "1-1"
    "2" → [2]
    "1-1" → [0]  (compute 1-1=0)
    Combine: 2-0 = 2

  Split at '-' (index 3): "2-1" and "1"
    "2-1" → [1]
    "1" → [1]
    Combine: 1-1 = 0

Result: [2, 0]
```

```java
List<Integer> diffWaysToCompute(String expression) {
    List<Integer> result = new ArrayList<>();

    for (int i = 0; i < expression.length(); i++) {
        char c = expression.charAt(i);

        if (c == '+' || c == '-' || c == '*') {
            // DIVIDE: split at this operator
            List<Integer> left  = diffWaysToCompute(expression.substring(0, i));
            List<Integer> right = diffWaysToCompute(expression.substring(i + 1));

            // COMBINE: try all combinations
            for (int l : left) {
                for (int r : right) {
                    if (c == '+') result.add(l + r);
                    else if (c == '-') result.add(l - r);
                    else result.add(l * r);
                }
            }
        }
    }

    // Base case: no operator found → it's a number
    if (result.isEmpty()) {
        result.add(Integer.parseInt(expression));
    }
    return result;
}
```

---

## 5.5 Pow(x, n) — LC 50

Already covered in Phase 2. The key D&C insight:

```
x^n = (x^(n/2))²          if n is even
x^n = x × (x^(n/2))²     if n is odd

This halves n each call → O(log N) instead of O(N)
```

### BYTS Problems — Phase 5

| # | Problem | Pattern |
|---|---------|---------|
| 50 | Pow(x, n) | Fast exponentiation D&C |
| 241 | Different Ways Add Parentheses | D&C on expressions |
| 912 | Sort an Array | Merge sort / Quick sort |
| 148 | Sort List | Merge sort on list |
| 315 | Count of Smaller Numbers After Self | Merge sort variant |

---

# 🟣 Phase 6 — Backtracking (Recursion + Undo)

---

## 6.1 What is Backtracking?

Backtracking = recursion with an **UNDO step**.

```
The three-step loop:
  for each candidate:
    1. CHOOSE   → add candidate to current path
    2. EXPLORE  → recurse deeper
    3. UNDO     → remove candidate from current path

Think of it like exploring a maze:
  Walk down a path → hit dead end → step BACK → try another path

Decision tree for subsets of [1,2,3]:
              []
           /  |  \
         [1] [2] [3]
         /\    \
      [1,2][1,3] [2,3]
       |
    [1,2,3]

Each edge = one choice, each node = one state
```

```
CRITICAL: ALWAYS snapshot the list when adding to result:
  result.add(new ArrayList<>(current)); // ✅ COPY
  result.add(current);                  // ❌ REFERENCE (will be empty!)
```

---

## 6.2 Subsets (LC 78)

```
nums = [1,2,3]
Add at EVERY NODE, not just leaves:

backtrack([], 0):
  add [] to result
  choose 1 → backtrack([1], 1):
    add [1]
    choose 2 → backtrack([1,2], 2):
      add [1,2]
      choose 3 → backtrack([1,2,3], 3): add [1,2,3]
      undo 3
    undo 2
    choose 3 → backtrack([1,3], 3): add [1,3]
    undo 3
  undo 1
  choose 2 → backtrack([2], 2): add [2]...
  ...
Result: [], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]
```

```java
List<List<Integer>> result = new ArrayList<>();

List<List<Integer>> subsets(int[] nums) {
    backtrack(nums, 0, new ArrayList<>());
    return result;
}

void backtrack(int[] nums, int start, List<Integer> current) {
    result.add(new ArrayList<>(current));   // add at EVERY node

    for (int i = start; i < nums.length; i++) {
        current.add(nums[i]);               // CHOOSE
        backtrack(nums, i + 1, current);    // EXPLORE
        current.remove(current.size() - 1); // UNDO
    }
}
```

---

## 6.3 Permutations (LC 46)

```
nums = [1,2,3]
All orderings — order MATTERS, use boolean[] used

backtrack([], used=[F,F,F]):
  try i=0: choose 1, used=[T,F,F]
    try i=1: choose 2, used=[T,T,F]
      try i=2: choose 3 → [1,2,3] ✅
    undo 2, try i=2: choose 3
      try i=1: choose 2 → [1,3,2] ✅
  undo 1, try i=1: choose 2 ...
```

```java
List<List<Integer>> result = new ArrayList<>();

List<List<Integer>> permute(int[] nums) {
    boolean[] used = new boolean[nums.length]; // auto false
    backtrack(nums, used, new ArrayList<>());
    return result;
}

void backtrack(int[] nums, boolean[] used, List<Integer> current) {
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current)); // leaf = complete permutation
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;          // skip already used

        used[i] = true;
        current.add(nums[i]);           // CHOOSE
        backtrack(nums, used, current); // EXPLORE
        current.remove(current.size()-1); // UNDO
        used[i] = false;
    }
}
```

---

## 6.4 Combination Sum I & II (LC 39, 40)

```java
// LC 39 — Reuse allowed (recurse with i, not i+1)
void backtrack39(int[] candidates, int target, int start, List<Integer> curr) {
    if (target == 0) { result.add(new ArrayList<>(curr)); return; }

    for (int i = start; i < candidates.length; i++) {
        if (candidates[i] > target) break;   // prune (must sort first)
        curr.add(candidates[i]);
        backtrack39(candidates, target - candidates[i], i, curr);  // i not i+1!
        curr.remove(curr.size() - 1);
    }
}

// LC 40 — Each used once, skip duplicates (sort first!)
void backtrack40(int[] c, int target, int start, List<Integer> curr) {
    if (target == 0) { result.add(new ArrayList<>(curr)); return; }
    if (target < 0) return;

    for (int i = start; i < c.length; i++) {
        if (i > start && c[i] == c[i-1]) continue; // skip LEVEL duplicate
        curr.add(c[i]);
        backtrack40(c, target - c[i], i + 1, curr);  // i+1 = no reuse!
        curr.remove(curr.size() - 1);
    }
}
```

---

## 6.5 N-Queens (LC 51)

```
Place N queens on N×N board: no two queens share row, column, or diagonal.

For row=0, try each column:
  Place at (0,0) → try row=1...
    Place at (1,2) → try row=2...

Check safe: no other queen in same column, diagonal, or anti-diagonal.
Use HashSets for O(1) check:
  cols: set of used columns
  diag: row-col (same for all cells on a diagonal)
  antiDiag: row+col (same for all cells on anti-diagonal)
```

```java
List<List<String>> result = new ArrayList<>();

List<List<String>> solveNQueens(int n) {
    char[][] board = new char[n][n];
    for (char[] row : board) java.util.Arrays.fill(row, '.');
    backtrack(board, 0, n,
              new java.util.HashSet<>(),
              new java.util.HashSet<>(),
              new java.util.HashSet<>());
    return result;
}

void backtrack(char[][] board, int row, int n,
               java.util.Set<Integer> cols,
               java.util.Set<Integer> diag,
               java.util.Set<Integer> antiDiag) {
    if (row == n) {
        List<String> solution = new ArrayList<>();
        for (char[] r : board) solution.add(new String(r));
        result.add(solution);
        return;
    }

    for (int col = 0; col < n; col++) {
        if (cols.contains(col)
            || diag.contains(row - col)
            || antiDiag.contains(row + col)) continue; // unsafe

        board[row][col] = 'Q';
        cols.add(col); diag.add(row-col); antiDiag.add(row+col);

        backtrack(board, row + 1, n, cols, diag, antiDiag);

        board[row][col] = '.';
        cols.remove(col); diag.remove(row-col); antiDiag.remove(row+col);
    }
}
```

---

## 6.6 Word Search (LC 79)

```
Grid:
A B C E
S F C S
A D E E

Find "ABCCED":
Start at (0,0)='A', go right to (0,1)='B',
go right to (0,2)='C', go down to (1,2)='C',
go down to (2,2)='E', go left to (2,1)='D' ✅
```

```java
boolean exist(char[][] board, String word) {
    int m = board.length, n = board[0].length;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (dfs(board, word, i, j, 0)) return true;
        }
    }
    return false;
}

boolean dfs(char[][] board, String word, int i, int j, int index) {
    if (index == word.length()) return true;      // found entire word!

    if (i < 0 || i >= board.length) return false;
    if (j < 0 || j >= board[0].length) return false;
    if (board[i][j] != word.charAt(index)) return false;

    char temp = board[i][j];
    board[i][j] = '#';                            // MARK VISITED (CHOOSE)

    boolean found = dfs(board, word, i-1, j, index+1)
                 || dfs(board, word, i+1, j, index+1)
                 || dfs(board, word, i, j-1, index+1)
                 || dfs(board, word, i, j+1, index+1);

    board[i][j] = temp;                           // RESTORE (UNDO)

    return found;
}
```

### BYTS Problems — Phase 6

| # | Problem | Pattern |
|---|---------|---------|
| 78 | Subsets | Add at every node |
| 90 | Subsets II | Sort + skip duplicates |
| 46 | Permutations | boolean[] used |
| 47 | Permutations II | Sort + !used[i-1] |
| 39 | Combination Sum | Recurse with i (reuse) |
| 40 | Combination Sum II | Sort + skip + i+1 |
| 77 | Combinations | start index + stop early |
| 131 | Palindrome Partitioning | isPalin + recurse |
| 51 | N-Queens | 3 HashSets + row by row |
| 52 | N-Queens II | Count only |
| 79 | Word Search | Mark/unmark + 4 dirs |
| 93 | Restore IP Addresses | 4 segments |
| 22 | Generate Parentheses | Count open/close |
| 17 | Letter Combinations Phone | Map + index |
| 37 | Sudoku Solver | isValid + cell-by-cell |

---

# ⚫ Phase 7 — Recursion on Linked Lists

---

## 7.1 Reverse Linked List (LC 206)

```
Recursive insight:
  To reverse 1→2→3→4→5:
  reverse(1→2→3→4→5)
    = reverse(2→3→4→5), then make 2 point to 1
    = [5→4→3→2] with 1 dangling
    Then: 2.next = 1, 1.next = null
    = 5→4→3→2→1 ✅

At each call: we get the reversed rest back,
              then fix the current node's pointer.
```

```java
ListNode reverseList(ListNode head) {
    // Base: empty list or single node
    if (head == null || head.next == null) return head;

    // Trust: this reverses everything after head
    ListNode newHead = reverseList(head.next);

    // Fix: make next node point back to head
    head.next.next = head;  // e.g., 2.next = 1
    head.next = null;        // break original forward link

    return newHead;          // new head is always the original tail
}
```

```
Call trace for 1→2→3:
  reverseList(1) calls reverseList(2)
    reverseList(2) calls reverseList(3)
      reverseList(3): head.next==null → return 3 (base)
    Back at node 2: 3.next = 2, 2.next = null → 3→2
    return 3
  Back at node 1: 2.next = 1, 1.next = null → 3→2→1
  return 3
```

---

## 7.2 Merge Two Sorted Lists (LC 21)

```
merge(1→3→5, 2→4→6):
  1 < 2 → 1.next = merge(3→5, 2→4→6)
                    2 < 3 → 2.next = merge(3→5, 4→6)
                              3 < 4 → 3.next = merge(5, 4→6)
                                        4 < 5 → 4.next = merge(5, 6)
                                                  5 < 6 → 5.next = merge(null, 6)
                                                            = 6 (base)
                                                  = 5→6
                                                = 4→5→6
                                              = 3→4→5→6
                                            = 2→3→4→5→6
                                          = 1→2→3→4→5→6 ✅
```

```java
ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    // Base cases
    if (l1 == null) return l2;
    if (l2 == null) return l1;

    // Compare and recurse
    if (l1.val <= l2.val) {
        l1.next = mergeTwoLists(l1.next, l2); // l1 leads
        return l1;
    } else {
        l2.next = mergeTwoLists(l1, l2.next); // l2 leads
        return l2;
    }
}
```

---

## 7.3 Palindrome Check on Linked List

```
Key trick: use a global "left" pointer that starts at head,
while the recursion goes to the TAIL.
When recursion unwinds, compare left (moving right) with
current node (moving left as stack unwinds).

1→2→2→1:
  recurse to end, then compare:
  current=1(tail), left=1(head): match, left=left.next
  current=2, left=2: match, left=left.next
  ...
```

```java
ListNode left;

boolean isPalindrome(ListNode head) {
    left = head;
    return check(head);
}

boolean check(ListNode right) {
    if (right == null) return true; // base: reached end

    // Recurse all the way to the end FIRST
    boolean subResult = check(right.next);

    // On unwind: compare left pointer with current right
    boolean match = left.val == right.val;
    left = left.next;    // advance left (global pointer)

    return subResult && match;
}
```

### BYTS Problems — Phase 7

| # | Problem | Pattern |
|---|---------|---------|
| 206 | Reverse Linked List | Recursive fix pointers |
| 21 | Merge Two Sorted Lists | Recursive compare |
| 234 | Palindrome Linked List | Global left pointer |
| 24 | Swap Nodes in Pairs | Recursive pair swap |
| 25 | Reverse Nodes in k-Group | Recursive group |
| 143 | Reorder List | Mid + reverse + merge |

---

# 🔵 Phase 8 — Recursion to DP

---

## 8.1 Recognizing Overlapping Subproblems

When recursive calls solve the SAME subproblem multiple times → convert to DP.

```
SIGNAL: The same function call appears multiple times in the recursion tree.

Fibonacci:      fib(5)
               /       \
          fib(4)       fib(3)   ← fib(3) appears again!
         /     \
     fib(3)   fib(2)            ← fib(3) is repeated

Climbing Stairs:  climbWays(5)
                 /             \
         climbWays(4)       climbWays(3)   ← repeated!
        /          \
  climbWays(3)  climbWays(2)  ← repeated again!

These problems BENEFIT from memoization or tabulation.
```

### The Three-Step Conversion

```
STEP 1: Write the recursive solution first (for clarity)
STEP 2: Add a memo HashMap (top-down memoization)
STEP 3: Optional: convert to bottom-up dp array (tabulation)
```

---

## 8.2 Top-Down Memoization

```java
// Pattern for ANY recursive → memoized conversion

// Before (naive recursion — O(2^N)):
int solve(int n) {
    if (n <= 1) return n;
    return solve(n-1) + solve(n-2);
}

// After (memoized — O(N)):
Map<Integer, Integer> memo = new HashMap<>();

int solve(int n) {
    if (n <= 1) return n;

    if (memo.containsKey(n)) return memo.get(n); // ← RETURN CACHED

    int result = solve(n-1) + solve(n-2);

    memo.put(n, result);  // ← CACHE BEFORE RETURN
    return result;
}

// For 2D state (e.g., grid problems):
int[][] memo2D = new int[m][n];
for (int[] row : memo2D) Arrays.fill(row, -1); // -1 = not computed

int solve(int row, int col) {
    if (outOfBounds) return 0;
    if (memo2D[row][col] != -1) return memo2D[row][col];
    int result = /* ... */;
    return memo2D[row][col] = result; // assign and return
}
```

---

## 8.3 Climbing Stairs (LC 70) — Recursion → DP

```java
// STEP 1: Naive recursion — O(2^N)
int climbNaive(int n) {
    if (n == 0) return 1;
    if (n < 0) return 0;
    return climbNaive(n-1) + climbNaive(n-2);
}

// STEP 2: Memoized — O(N)
Map<Integer, Integer> memo = new HashMap<>();
int climbMemo(int n) {
    if (n == 0) return 1;
    if (n < 0) return 0;
    if (memo.containsKey(n)) return memo.get(n);
    int result = climbMemo(n-1) + climbMemo(n-2);
    memo.put(n, result);
    return result;
}

// STEP 3: Bottom-up DP — O(N), O(1) space
int climbDP(int n) {
    if (n <= 2) return n;
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

---

## 8.4 House Robber (LC 198) — Recursion → DP

```java
// STEP 1: Naive recursion
int robNaive(int[] nums, int i) {
    if (i >= nums.length) return 0;
    // Rob house i (skip i+1) OR skip house i
    return Math.max(
        nums[i] + robNaive(nums, i + 2),
        robNaive(nums, i + 1)
    );
}

// STEP 2: Memoized
int[] memo;
int robMemo(int[] nums, int i) {
    if (i >= nums.length) return 0;
    if (memo[i] != -1) return memo[i];
    return memo[i] = Math.max(
        nums[i] + robMemo(nums, i + 2),
        robMemo(nums, i + 1)
    );
}

// STEP 3: Bottom-up DP
int robDP(int[] nums) {
    int prev2 = 0, prev1 = 0;
    for (int num : nums) {
        int curr = Math.max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

---

## 8.5 Coin Change (LC 322) — Recursion → DP

```java
// STEP 1: Naive recursion
int coinNaive(int[] coins, int amount) {
    if (amount == 0) return 0;
    if (amount < 0) return Integer.MAX_VALUE;

    int min = Integer.MAX_VALUE;
    for (int coin : coins) {
        int sub = coinNaive(coins, amount - coin);
        if (sub != Integer.MAX_VALUE) {
            min = Math.min(min, sub + 1);
        }
    }
    return min;
}

// STEP 2: Memoized
Map<Integer, Integer> memo = new HashMap<>();
int coinMemo(int[] coins, int amount) {
    if (amount == 0) return 0;
    if (amount < 0) return Integer.MAX_VALUE;
    if (memo.containsKey(amount)) return memo.get(amount);

    int min = Integer.MAX_VALUE;
    for (int coin : coins) {
        int sub = coinMemo(coins, amount - coin);
        if (sub != Integer.MAX_VALUE) min = Math.min(min, sub + 1);
    }
    memo.put(amount, min);
    return min;
}

// STEP 3: Bottom-up DP (standard)
int coinDP(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1); // "infinity"
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i) dp[i] = Math.min(dp[i], dp[i-coin] + 1);
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```

### BYTS Problems — Phase 8

| # | Problem | Recursion → DP |
|---|---------|----------------|
| 70 | Climbing Stairs | fib-style recursion |
| 198 | House Robber | take/skip recursion |
| 213 | House Robber II | run twice |
| 322 | Coin Change | try each coin |
| 300 | Longest Increasing Subsequence | O(N²) recursion |
| 139 | Word Break | try each split point |
| 1143 | Longest Common Subsequence | 2D recursion |
| 72 | Edit Distance | 3-way min recursion |
| 62 | Unique Paths | grid recursion |
| 416 | Partition Equal Subset Sum | knapsack recursion |
| 494 | Target Sum | +/- recursion |
| 337 | House Robber III | tree rob/skip |

---

# 📋 Master Reference

---

## Recursion Pattern Decision Table

```
PROBLEM SIGNAL                           → RECURSION TYPE
─────────────────────────────────────────────────────────────────
"compute n, use n-1 result"              → Linear recursion
"string/array from both ends"            → Two-pointer recursion
"split into two halves"                  → Binary/D&C recursion
"tree: answer at node from children"     → Tree recursion (postorder)
"tree: carry info from root to leaf"     → Tree recursion (preorder)
"split, solve each, combine"             → Divide and Conquer
"find all combinations/permutations"     → Backtracking
"grid DFS, mark/unmark visited"          → Backtracking on matrix
"same subproblem called multiple times"  → Memoize! (Recursion → DP)
"optimal substructure + overlapping"     → Recursion → DP
"linked list: solve from tail"           → Recursive + unwind
```

---

## Common Mistakes & How to Avoid

```
MISTAKE 1: Missing or wrong base case
  ❌ int sum(int n) { return n + sum(n-1); }  // infinite!
  ✅ if (n == 0) return 0;                     // add base case

MISTAKE 2: Not making problem smaller
  ❌ return solve(n);      // same n → infinite recursion
  ✅ return solve(n-1);    // n decreases each call

MISTAKE 3: Snapshot when adding to result
  ❌ result.add(current);             // adds reference, will be empty!
  ✅ result.add(new ArrayList<>(current)); // adds copy ✅

MISTAKE 4: Forgetting to UNDO in backtracking
  ❌ path.add(node); recurse(next);          // no undo
  ✅ path.add(node); recurse(next); path.remove(last); // always undo

MISTAKE 5: Forgetting UNDO for visited in word search
  ❌ board[i][j] = '#'; dfs(...); // forgot to restore
  ✅ char tmp = board[i][j]; board[i][j]='#'; dfs(...); board[i][j]=tmp;

MISTAKE 6: Not trusting recursion
  Trying to trace through all recursive calls mentally.
  INSTEAD: assume recursive call returns correct answer,
           just handle the current level.

MISTAKE 7: Wrong base case for tree
  ❌ if (root == null) return -1;   // wrong for height problems
  ✅ if (root == null) return 0;    // empty tree has height 0
  ✅ if (root == null) return null; // for pointer-returning functions

MISTAKE 8: Using int when overflow possible in recursion
  ❌ int result = factorial(20);   // overflows int
  ✅ long result = factorial(20);  // use long!
```

---

## All BYTS Recursion Problems

### Phase 2 — Linear Recursion
| # | Problem | Technique |
|---|---------|-----------|
| 344 | Reverse String | Two-pointer recursive |
| 9 | Palindrome Number | String recursive |
| 50 | Pow(x, n) | Fast exponentiation |
| 509 | Fibonacci Number | Linear recursion |
| 231 | Power of Two | Divide by 2 |
| 342 | Power of Four | Divide by 4 |

### Phase 3 — Binary Recursion
| # | Problem | Technique |
|---|---------|-----------|
| 509 | Fibonacci Number | Two-call + memo |
| 70 | Climbing Stairs | Two-call + memo |
| 704 | Binary Search | Halving recursion |
| 241 | Different Ways Add Parentheses | D&C two calls |

### Phase 4 — Tree Recursion
| # | Problem | Technique |
|---|---------|-----------|
| 104 | Maximum Depth | Return from children |
| 111 | Minimum Depth | Null child base case |
| 226 | Invert Binary Tree | Swap + return |
| 100 | Same Tree | Compare both sides |
| 101 | Symmetric Tree | Mirror compare |
| 572 | Subtree of Another Tree | isSameTree each node |
| 110 | Balanced Binary Tree | -1 sentinel |
| 543 | Diameter | Height + global update |
| 112 | Path Sum | Subtract to leaf |
| 113 | Path Sum II | Backtrack path |
| 124 | Binary Tree Max Path Sum | Max gain pattern |
| 257 | Binary Tree Paths | Build string path |
| 105 | Construct from Preorder+Inorder | Split recursion |
| 236 | LCA of Binary Tree | Both-sides check |
| 297 | Serialize/Deserialize | Preorder + queue |

### Phase 5 — Divide and Conquer
| # | Problem | Technique |
|---|---------|-----------|
| 50 | Pow(x, n) | Half exponent D&C |
| 241 | Different Ways Add Parens | D&C expressions |
| 912 | Sort an Array | Merge sort |
| 148 | Sort List | Merge sort linked list |

### Phase 6 — Backtracking
| # | Problem | Technique |
|---|---------|-----------|
| 78 | Subsets | Add at every node |
| 90 | Subsets II | Sort + skip dup |
| 46 | Permutations | boolean[] used |
| 47 | Permutations II | Sort + !used[i-1] |
| 39 | Combination Sum | Recurse with i |
| 40 | Combination Sum II | Sort + skip + i+1 |
| 77 | Combinations | start index |
| 131 | Palindrome Partitioning | isPalin + recurse |
| 51 | N-Queens | 3 HashSets |
| 52 | N-Queens II | Count solutions |
| 79 | Word Search | Mark/unmark grid |
| 93 | Restore IP Addresses | 4 segments |
| 22 | Generate Parentheses | Count open/close |
| 17 | Letter Combinations Phone | Map + index |
| 37 | Sudoku Solver | isValid + fill |

### Phase 7 — Linked List Recursion
| # | Problem | Technique |
|---|---------|-----------|
| 206 | Reverse Linked List | Fix pointers on unwind |
| 21 | Merge Two Sorted Lists | Recursive merge |
| 234 | Palindrome Linked List | Global left pointer |
| 24 | Swap Nodes in Pairs | Recursive pair |
| 25 | Reverse Nodes in k-Group | Recursive groups |

### Phase 8 — Recursion → DP
| # | Problem | Technique |
|---|---------|-----------|
| 70 | Climbing Stairs | Memo → tabulate |
| 198 | House Robber | Take/skip memo |
| 213 | House Robber II | Run twice |
| 322 | Coin Change | Try each coin memo |
| 300 | LIS | O(N²) memo |
| 139 | Word Break | Split point memo |
| 1143 | LCS | 2D memo |
| 72 | Edit Distance | 3-way min memo |
| 62 | Unique Paths | Grid memo |
| 416 | Partition Equal Subset Sum | Knapsack memo |
| 494 | Target Sum | +/- memo |
| 337 | House Robber III | Tree rob/skip pair |

---

*Updated: 2026-06-18 | Java | BYTS Sheet*
*All recursion patterns: Linear · Binary · Tree · D&C · Backtracking · Linked List · Recursion→DP*
