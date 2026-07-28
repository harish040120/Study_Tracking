# 🧮 Dynamic Programming — Complete Course
**Language:** Java | **Level:** SDE Interview Ready
**Based on:** BYTS Sheet Problems
**Style:** Concept → ASCII Diagram → Why it works → Java Code → Problems

---

# 📌 Contents

**Phase 1 — DP Foundations**
- 1.1 What is Dynamic Programming?
- 1.2 The Two Properties DP Needs
- 1.3 How to Recognize a DP Problem
- 1.4 Top-Down vs Bottom-Up
- 1.5 Java Syntax Reference — Full Sheet
- 1.6 Common Tricks & Traps

**Phase 2 — 1D DP — Linear Sequence Patterns**
- 2.1 Fibonacci Style (Climbing Stairs)
- 2.2 House Robber (Non-Adjacent Selection)
- 2.3 Kadane's Algorithm (Max Subarray)
- 2.4 Maximum Product Subarray
- 2.5 Decode Ways

**Phase 3 — 1D DP — Unbounded & Combinatorial**
- 3.1 Coin Change (Min Coins)
- 3.2 Coin Change II (Count Ways)
- 3.3 Combination Sum IV
- 3.4 Word Break
- 3.5 Longest Increasing Subsequence

**Phase 4 — 2D DP — Grid Patterns**
- 4.1 Unique Paths
- 4.2 Unique Paths II (Obstacles)
- 4.3 Minimum Path Sum
- 4.4 Triangle
- 4.5 Minimum Falling Path Sum

**Phase 5 — 2D DP — String Matching**
- 5.1 Longest Common Subsequence
- 5.2 Edit Distance
- 5.3 Delete Operation for Two Strings
- 5.4 Distinct Subsequences

**Phase 6 — Knapsack Family**
- 6.1 0/1 Knapsack (Theory)
- 6.2 Partition Equal Subset Sum
- 6.3 Target Sum
- 6.4 Maximal Square

**Phase 7 — Tree DP & Special DP**
- 7.1 House Robber II (Circular)
- 7.2 House Robber III (Tree)
- 7.3 Delete and Earn
- 7.4 Russian Doll Envelopes

**Phase 8 — Hard / Interval DP**
- 8.1 Burst Balloons
- 8.2 Remove Boxes
- 8.3 Count Square Submatrices

**Master Reference**
- Pattern Decision Table
- All BYTS Problems by Phase

---

# 🟢 Phase 1 — DP Foundations

---

## 1.1 What is Dynamic Programming?

Dynamic Programming (DP) is an optimization technique for problems that can be broken into smaller, OVERLAPPING subproblems.

> **Core idea: never calculate the same thing twice. Store the result the first time, reuse it later.**

### The Fibonacci Example — Why DP Matters

```
WITHOUT DP (plain recursion):

                    fib(5)
                  /        \
              fib(4)        fib(3)
             /     \        /     \
         fib(3)   fib(2) fib(2)  fib(1)
         /    \    /  \   /  \
     fib(2) fib(1)...  ...  ...
     ←━━━━━━━━━━━━━━━━━━━━━━━━━━→
     fib(3) calculated TWICE
     fib(2) calculated THREE times
     Time: O(2^N) — exponential, terrible ❌

WITH DP (cache results):

     fib(2) calculated ONCE → cached → reused everywhere
     Time: O(N) — linear, excellent ✅
```

---

## 1.2 The Two Properties DP Needs

### Property 1 — Overlapping Subproblems

The SAME smaller problem gets solved multiple times if you don't cache it.

```
fib(5) needs fib(4) and fib(3)
fib(4) needs fib(3) and fib(2)
                ↑
        fib(3) is needed by BOTH fib(5) and fib(4)
        → OVERLAPPING subproblem
```

### Property 2 — Optimal Substructure

The optimal answer to the BIG problem can be built from optimal answers to SMALLER subproblems.

```
Shortest path from A to C through B:
  shortestPath(A,C) = shortestPath(A,B) + shortestPath(B,C)
  ↑ big problem solved using smaller optimal subproblems
```

---

## 1.3 How to Recognize a DP Problem

Ask these 4 questions:

```
1. Is the answer MAX / MIN / COUNT / LONGEST / SHORTEST?
2. Does the answer depend on PREVIOUS choices made?
3. Can you define it as a SMALLER version of the same problem?
4. Does brute force have REPEATED calculations?

→ YES to most of these = likely DP
```

### Keyword Signal Table

```
Problem says...                       → Likely DP Type
─────────────────────────────────────────────────────────
"minimum number of..."                → 1D or 2D DP
"maximum sum/product..."              → Kadane's / 1D DP
"number of ways to..."                → Combinatorial DP
"longest subsequence/substring..."    → String DP
"can you partition into..."           → Knapsack
"two strings, transform/compare..."   → 2D String DP
"grid, only right/down moves..."      → Grid DP
"tree, rob/select non-adjacent..."    → Tree DP
```

---

## 1.4 Top-Down vs Bottom-Up

### Top-Down (Memoization)

Write the NATURAL recursive solution first, then CACHE results so repeated calls return instantly.

```
Direction:  BIG problem → split into smaller → solve → combine
            (starts at the top, recurses down)
```

```java
Map<Integer, Integer> memo = new HashMap<>();

int fib(int n) {
    if (n <= 1) return n;
    if (memo.containsKey(n)) return memo.get(n); // ← CACHED, instant
    int result = fib(n - 1) + fib(n - 2);
    memo.put(n, result);                          // ← STORE for later
    return result;
}
```

### Bottom-Up (Tabulation) ✅ Preferred in interviews

Fill a `dp[]` array starting from the SMALLEST subproblem, building UP to the answer.

```
Direction:  SMALL problems first → build up → reach the answer
            (starts at the bottom, iterates up)
```

```java
int fib(int n) {
    if (n <= 1) return n;
    int[] dp = new int[n + 1];
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2]; // build up from base cases
    }
    return dp[n];
}
```

### Comparison Table

```
                    Top-Down            Bottom-Up
─────────────────────────────────────────────────────
Direction           Big → small         Small → big
Code style          Recursive           Iterative loop
Stack overflow risk Yes (deep recur.)   No
Space optimization  Harder              Easy (rolling vars)
Best when            Sparse states       Need ALL subproblems
Interview preference Either OK           ✅ usually preferred
```

---

## 1.5 Java Syntax Reference — Full Sheet

```java
// ── ARRAY DP INITIALIZATION ───────────────────────────────────
int[] dp = new int[n + 1];                  // all zeros by default
boolean[] dp = new boolean[n + 1];          // all false by default
int[][] dp = new int[m + 1][n + 1];         // 2D, all zeros

Arrays.fill(dp, Integer.MAX_VALUE);         // "infinity" sentinel
Arrays.fill(dp, -1);                        // "not computed" sentinel
Arrays.fill(dp, amount + 1);                // safe "infinity" for coin change

// ── MEMOIZATION (TOP-DOWN) ────────────────────────────────────
Map<Integer, Integer> memo = new HashMap<>();
if (memo.containsKey(n)) return memo.get(n);
memo.put(n, result);

// For 2D state, encode as a string key or use a 2D array:
Map<String, Integer> memo2D = new HashMap<>();
String key = i + "," + j;

int[][] memo2D = new int[m][n];
Arrays.fill(memo2D, -1); // doesn't work directly for 2D — loop per row:
for (int[] row : memo2D) Arrays.fill(row, -1);

// ── COMMON RECURRENCES (memorize these) ───────────────────────
// Fibonacci style:
dp[i] = dp[i-1] + dp[i-2];

// Max of options (optimization):
dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);

// Min of options:
dp[i] = Math.min(dp[i], dp[i - coin] + 1);

// Count ways (sum of options):
dp[i] += dp[i - coin];

// 2D string matching:
dp[i][j] = dp[i-1][j-1] + 1;                          // match
dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);          // no match

// Grid path:
dp[i][j] = dp[i-1][j] + dp[i][j-1];

// 0/1 Knapsack:
dp[i][w] = Math.max(dp[i-1][w], val[i-1] + dp[i-1][w-wt[i-1]]);

// ── SPACE OPTIMIZATION — rolling variables ────────────────────
// Instead of dp[i] needing only dp[i-1] and dp[i-2]:
int prev2 = base0, prev1 = base1;
for (int i = 2; i <= n; i++) {
    int curr = prev1 + prev2;  // or whatever the recurrence is
    prev2 = prev1;
    prev1 = curr;
}
return prev1;

// ── 0/1 KNAPSACK SPACE OPTIMIZATION — REVERSE loop ────────────
// (prevents reusing the same item twice)
for (int num : nums) {
    for (int j = target; j >= num; j--) {       // ← REVERSE
        dp[j] = dp[j] || dp[j - num];
    }
}

// ── UNBOUNDED KNAPSACK SPACE OPTIMIZATION — FORWARD loop ──────
// (allows reusing the same item multiple times)
for (int coin : coins) {
    for (int j = coin; j <= amount; j++) {      // ← FORWARD
        dp[j] += dp[j - coin];
    }
}

// ── FINDING MAX/MIN IN DP ARRAY ────────────────────────────────
int ans = Arrays.stream(dp).max().getAsInt();
int ans = 0; for (int x : dp) ans = Math.max(ans, x);

// ── BINARY SEARCH WITHIN DP (for O(N log N) LIS) ──────────────
List<Integer> tails = new ArrayList<>();
int pos = Collections.binarySearch(tails, num);
if (pos < 0) pos = -(pos + 1); // convert to insertion point
```

---

## 1.6 Common Tricks & Traps

### ⭐ Trick 1 — "Infinity" Sentinel for Min Problems

```java
// When finding MINIMUM, initialize with a value LARGER than any real answer
int[] dp = new int[amount + 1];
Arrays.fill(dp, amount + 1);  // amount+1 coins is impossible → safe "infinity"
dp[0] = 0;
// ... after filling ...
return dp[amount] > amount ? -1 : dp[amount]; // still "infinity" = unreachable
```

### ⭐ Trick 2 — Padding dp Array by +1

Most 2D string/array DP uses `dp[i][j]` to represent "using the first `i` characters". This means `dp` needs size `(n+1)` so `dp[0]` represents "using 0 characters" (empty case).

```
String "abc" has length 3
dp array size = 4  → dp[0], dp[1], dp[2], dp[3]
dp[0] = base case (empty string)
dp[3] = answer using all 3 characters
```

### ⭐ Trick 3 — Reverse Loop = No Reuse, Forward Loop = Reuse Allowed

This is THE most common bug in knapsack-style DP.

```
0/1 Knapsack (each item used ONCE):
  for each item:
    for j = target DOWNTO item_value:   ← REVERSE
      dp[j] = dp[j] OR dp[j - item_value]

Unbounded Knapsack (item used UNLIMITED times):
  for each item:
    for j = item_value TO target:        ← FORWARD
      dp[j] += dp[j - item_value]

WHY: Reverse loop means when computing dp[j], dp[j-item] hasn't
     been updated YET in this same item's pass → can't double-count.
     Forward loop means dp[j-item] MIGHT already include this item
     → reuse is allowed.
```

### ⭐ Trick 4 — Outer/Inner Loop Order Matters for Counting Ways

```
"Number of ways" with COMBINATIONS (order doesn't matter):
  outer loop = items (coins)
  inner loop = amount
  → counts {1,2} same as {2,1} — ONE way

"Number of ways" with PERMUTATIONS (order matters):
  outer loop = amount
  inner loop = items (coins)
  → counts {1,2} and {2,1} as DIFFERENT ways
```

### ⭐ Trick 5 — Track BOTH Max and Min for Product Problems

Negative × negative = positive! For "maximum product" type problems, a SMALL (very negative) number times another negative number could become the new MAX.

```java
int maxP = nums[0], minP = nums[0];
for (int i = 1; i < n; i++) {
    int temp = maxP; // save before overwriting
    maxP = Math.max(nums[i], Math.max(maxP*nums[i], minP*nums[i]));
    minP = Math.min(nums[i], Math.min(temp*nums[i], minP*nums[i]));
}
```

### ⚠️ Trap — Off-by-One in dp Array Indexing

The #1 source of bugs in DP. Always be crystal clear whether `dp[i]` means "using index i" or "using the FIRST i elements" — these are different by one.

```java
// "first i characters" convention (very common, avoids negative index)
// s.charAt(i-1) is the i-th character (1-indexed conceptually)
if (s1.charAt(i-1) == s2.charAt(j-1)) dp[i][j] = dp[i-1][j-1] + 1;
```

### ⚠️ Trap — Forgetting Base Cases

```java
// WRONG: dp[0][j] and dp[i][0] left as default 0 when they
//        should represent something specific (e.g., edit distance i or j)
for (int i = 0; i <= m; i++) dp[i][0] = i;  // ✅ explicit base case
for (int j = 0; j <= n; j++) dp[0][j] = j;  // ✅ explicit base case
```

---

# 🔵 Phase 2 — 1D DP — Linear Sequence Patterns

---

## 2.1 Fibonacci Style — Climbing Stairs (LC 70)

### Concept

Each state depends on 1 or 2 PREVIOUS states. Classic "count the ways" pattern.

```
dp[i] = number of ways to reach step i
dp[i] = dp[i-1] + dp[i-2]
        (you arrived here from step i-1, OR from step i-2)
```

```
n=5 stairs, can climb 1 or 2 steps at a time:

dp[0]=1 (base: 1 way to "stay" at start)
dp[1]=1 (only 1 way: single step)
dp[2]=2 (1+1, or 2)
dp[3]=dp[2]+dp[1]=3
dp[4]=dp[3]+dp[2]=5
dp[5]=dp[4]+dp[3]=8

Visualization:
step: 0  1  2  3  4  5
ways: 1  1  2  3  5  8
```

```java
int climbStairs(int n) {
    if (n <= 2) return n;

    int prev2 = 1, prev1 = 2; // dp[1]=1, dp[2]=2

    for (int i = 3; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### Min Cost Climbing Stairs (LC 746)

```java
// dp[i] = min cost to REACH step i
// dp[i] = cost[i] + min(dp[i-1], dp[i-2])
// You can START at step 0 OR step 1 for free

int minCostClimbingStairs(int[] cost) {
    int n = cost.length;
    int prev2 = 0, prev1 = 0; // can start at step 0 or 1 free

    for (int i = 2; i <= n; i++) {
        int curr = Math.min(prev1 + cost[i-1], prev2 + cost[i-2]);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

---

## 2.2 House Robber — Non-Adjacent Selection (LC 198)

### Concept

Pick elements to MAXIMIZE sum, but CANNOT pick two adjacent elements.

```
At house i, decide:
  SKIP it  → carry forward best from house i-1
  ROB it   → take nums[i] + best from house i-2 (must skip i-1)

dp[i] = max(dp[i-1],  dp[i-2] + nums[i])
         ↑skip          ↑rob
```

```
houses = [2, 7, 9, 3, 1]

dp[0]=2
dp[1]=max(2,7)=7
dp[2]=max(7, 2+9)=11
dp[3]=max(11, 7+3)=11
dp[4]=max(11, 11+1)=12

Visualization:
house:  2   7   9   3   1
dp:     2   7   11  11  12
Answer = 12 (rob houses 0,2,4 → 2+9+1=12)
```

```java
int rob(int[] nums) {
    int prev2 = 0, prev1 = 0;

    for (int num : nums) {
        int curr = Math.max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

(House Robber II and III covered in Phase 7 — circular array and tree variants)

---

## 2.3 Kadane's Algorithm — Maximum Subarray (LC 53)

### Concept

Find the contiguous subarray with the LARGEST sum.

```
At every index, decide:
  START FRESH at current element, OR
  EXTEND the existing running subarray

dp[i] = max(nums[i],  dp[i-1] + nums[i])
```

```
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]

running sum: -2, 1, -2, 4, 3, 5, 6, 1, 5
max so far:  -2, 1,  1, 4, 4, 5, 6, 6, 6

Answer = 6 (subarray [4,-1,2,1])

ASCII trace:
idx:     0    1   2    3   4    5   6   7    8
nums:   -2    1  -3    4  -1    2   1  -5    4
sum:    -2    1  -2    4   3    5   6   1    5
         ↑reset  ↑reset
max:    -2    1   1    4   4    5   6   6    6
```

```java
int maxSubArray(int[] nums) {
    int sum = 0, maxSum = Integer.MIN_VALUE;

    for (int num : nums) {
        sum += num;
        maxSum = Math.max(maxSum, sum);  // update EVERY iteration
        if (sum < 0) sum = 0;            // reset only when negative
    }
    return maxSum;
}
```

**Why update `maxSum` EVERY time?** Handles the all-negative case correctly (e.g., `[-3,-1,-2]` → answer is `-1`, not 0).

---

## 2.4 Maximum Product Subarray (LC 152)

### Concept

Like Kadane's, but PRODUCT instead of sum. The twist: a negative number can flip the sign, so a very negative running product could become the BEST if multiplied by another negative.

**Track BOTH running max AND running min.**

```
nums = [2, 3, -2, 4]

i=0: maxP=2, minP=2
i=1: maxP=max(3, 2*3, 2*3)=6, minP=min(3,6,6)=3
i=2: maxP=max(-2, 6*-2, 3*-2)=-2, minP=min(-2,-12,-6)=-12
i=3: maxP=max(4, -2*4, -12*4)=4, minP=min(4,-8,-48)=-48

result = max(2,6,-2,4) = 6
```

```java
int maxProduct(int[] nums) {
    int maxP = nums[0], minP = nums[0], result = nums[0];

    for (int i = 1; i < nums.length; i++) {
        int temp = maxP; // save BEFORE overwriting

        maxP = Math.max(nums[i], Math.max(maxP * nums[i], minP * nums[i]));
        minP = Math.min(nums[i], Math.min(temp  * nums[i], minP * nums[i]));

        result = Math.max(result, maxP);
    }
    return result;
}
```

---

## 2.5 Decode Ways (LC 91)

### Concept

A string of digits can be decoded as letters (1='A', 2='B', ..., 26='Z'). Count the number of ways to decode.

```
dp[i] = number of ways to decode s[0..i-1]

If s[i-1] is NOT '0':            dp[i] += dp[i-1]   (single digit decode)
If s[i-2..i-1] is between 10-26: dp[i] += dp[i-2]   (two digit decode)
```

```
s = "226"

dp[0]=1 (empty string)
dp[1]: s[0]='2' (not 0) → dp[1] += dp[0] = 1

dp[2]: s[1]='2' (not 0) → dp[2] += dp[1] = 1
       s[0..1]="22" (10-26) → dp[2] += dp[0] = 1+1 = 2

dp[3]: s[2]='6' (not 0) → dp[3] += dp[2] = 2
       s[1..2]="26" (10-26) → dp[3] += dp[1] = 2+1 = 3

Answer = 3  ("2,2,6"→BBF, "22,6"→VF, "2,26"→BZ)
```

```java
int numDecodings(String s) {
    if (s.charAt(0) == '0') return 0;

    int n = s.length();
    int prev2 = 1, prev1 = 1; // dp[0]=1, dp[1]=1 (first char nonzero)

    for (int i = 2; i <= n; i++) {
        int curr = 0;

        // Single digit decode: s[i-1] != '0'
        if (s.charAt(i - 1) != '0') {
            curr += prev1;
        }

        // Two digit decode: s[i-2..i-1] is "10".."26"
        int twoDigit = (s.charAt(i - 2) - '0') * 10 + (s.charAt(i - 1) - '0');
        if (twoDigit >= 10 && twoDigit <= 26) {
            curr += prev2;
        }

        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### BYTS Problems — Phase 2

| # | Problem | Pattern |
|---|---------|---------|
| 70 | Climbing Stairs | Fibonacci |
| 746 | Min Cost Climbing Stairs | Fibonacci variant |
| 198 | House Robber | Non-adjacent select |
| 53 | Maximum Subarray | Kadane's |
| 152 | Maximum Product Subarray | Kadane's + min track |
| 91 | Decode Ways | String segmentation |
| 121 | Best Time to Buy/Sell Stock | Single pass min track |
| 122 | Best Time to Buy/Sell Stock II | Greedy/DP |

---

# 🟡 Phase 3 — 1D DP — Unbounded & Combinatorial

---

## 3.1 Coin Change — Minimum Coins (LC 322)

### Concept

Given coin denominations, find the MINIMUM number of coins to make a target amount.

```
dp[i] = minimum coins to make amount i
dp[0] = 0
dp[i] = min(dp[i - coin] + 1) for every coin where coin <= i
```

```
coins = [1, 5, 6], amount = 11

dp[0]=0
dp[1]=dp[0]+1=1
dp[2]=dp[1]+1=2
...
dp[5]=min(dp[4]+1, dp[0]+1)=1
dp[6]=min(dp[5]+1, dp[1]+1, dp[0]+1)=1
...
dp[11]=min(dp[10]+1, dp[6]+1, dp[5]+1) = min(?, 2, 2) = 2
       (6+5=11, using 2 coins)
```

```java
int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);  // "infinity" sentinel
    dp[0] = 0;

    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }

    return dp[amount] > amount ? -1 : dp[amount]; // still infinity = impossible
}
```

This is **Unbounded Knapsack** — each coin can be used UNLIMITED times (forward loop naturally handles this since `dp[i-coin]` might already use the same coin).

---

## 3.2 Coin Change II — Count Ways (LC 518)

### Concept

Count the NUMBER OF WAYS to make the amount (not minimum coins — total combinations).

⚠️ **CRITICAL:** outer loop = coins, inner loop = amount. This treats `{1,5}` and `{5,1}` as the SAME way (combination, not permutation).

```
coins = [1, 2, 5], amount = 5

dp[0]=1 (one way: use no coins)

Process coin=1: dp[1]+=dp[0]=1, dp[2]+=dp[1]=1, dp[3]+=dp[2]=1,
                dp[4]+=dp[3]=1, dp[5]+=dp[4]=1
                dp = [1,1,1,1,1,1]

Process coin=2: dp[2]+=dp[0]=1+1=2, dp[3]+=dp[1]=1+1=2,
                dp[4]+=dp[2]=1+2=3, dp[5]+=dp[3]=1+2=3
                dp = [1,1,2,2,3,3]

Process coin=5: dp[5]+=dp[0]=3+1=4
                dp = [1,1,2,2,3,4]

Answer = 4 ways: {1,1,1,1,1}, {1,1,1,2}, {1,2,2}, {5}
```

```java
int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1; // one way to make 0: use nothing

    for (int coin : coins) {           // OUTER: coins
        for (int i = coin; i <= amount; i++) {  // INNER: amount, FORWARD
            dp[i] += dp[i - coin];
        }
    }
    return dp[amount];
}
```

```
WHY outer=coins works for COMBINATIONS:
  Once we "move on" from coin=1 to coin=2, we never go back.
  This naturally avoids counting {1,2} and {2,1} as different.

If you swapped the loops (outer=amount, inner=coins), you'd be
counting PERMUTATIONS instead — a different problem!
```

---

## 3.3 Combination Sum IV (LC 377)

### Concept

⚠️ This LeetCode problem name is misleading — it's actually asking for PERMUTATIONS (order matters), unlike "Combination Sum".

```
nums = [1,2,3], target=4
Answer includes (1,1,1,1), (1,1,2), (1,2,1), (2,1,1), (1,3), (3,1), (2,2)
→ (1,1,2) and (1,2,1) and (2,1,1) are counted SEPARATELY
```

```java
int combinationSum4(int[] nums, int target) {
    int[] dp = new int[target + 1];
    dp[0] = 1;

    for (int i = 1; i <= target; i++) {       // OUTER: amount
        for (int num : nums) {                 // INNER: items
            if (num <= i) {
                dp[i] += dp[i - num];
            }
        }
    }
    return dp[target];
}
```

> **Compare to Coin Change II:** swapping the loop order changes combinations→permutations. Memorize this pair as a contrast example!

---

## 3.4 Word Break (LC 139)

### Concept

Can the string `s` be segmented into words from a dictionary?

```
dp[i] = true if s[0..i-1] can be fully segmented
dp[0] = true (empty string is trivially segmentable)
dp[i] = true if ANY split point j exists where
        dp[j] is true AND s[j..i-1] is a dictionary word
```

```
s = "leetcode", dict = ["leet", "code"]

dp[0]=true (base)
dp[1..3]=false (no valid word "l","le","lee")
dp[4]: check j=0, dp[0]=true AND s[0..3]="leet" in dict → dp[4]=true
dp[5..7]=false
dp[8]: check j=4, dp[4]=true AND s[4..7]="code" in dict → dp[8]=true

Answer = dp[8] = true
```

```java
boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;

    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && wordSet.contains(s.substring(j, i))) {
                dp[i] = true;
                break; // found one valid split, no need to check more
            }
        }
    }
    return dp[s.length()];
}
```

---

## 3.5 Longest Increasing Subsequence (LC 300)

### Concept

Find the length of the longest STRICTLY INCREASING subsequence (elements don't need to be contiguous).

```
dp[i] = length of LIS ENDING at index i
dp[i] = 1 + max(dp[j]) for all j < i where nums[j] < nums[i]
        (every element alone is at least an LIS of length 1)
```

```
nums = [10, 9, 2, 5, 3, 7, 101, 18]

dp[0]=1 (just [10])
dp[1]=1 (just [9], 9<10 fails)
dp[2]=1 (just [2])
dp[3]=2 (2<5, so [2,5])
dp[4]=2 (2<3, so [2,3])
dp[5]=3 (2<7 via dp[2]+1, or 5<7 via dp[3]+1, or 3<7 via dp[4]+1
        → max(dp[2],dp[3],dp[4])+1 = max(1,2,2)+1 = 3, [2,5,7] or [2,3,7])
dp[6]=4 ([2,5,7,101] or [2,3,7,101])
dp[7]=4 ([2,5,7,18] etc.)

dp array: [1,1,1,2,2,3,4,4]
Answer = max(dp) = 4
```

```java
// O(N²) APPROACH
int lengthOfLIS(int[] nums) {
    int[] dp = new int[nums.length];
    Arrays.fill(dp, 1);
    int result = 1;

    for (int i = 1; i < nums.length; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        result = Math.max(result, dp[i]);
    }
    return result;
}
```

### O(N log N) Approach — Patience Sorting

**Key insight:** maintain a `tails[]` array where `tails[k]` = the smallest possible tail value of an increasing subsequence of length `k+1`. Binary search for where each new number fits.

```java
int lengthOfLIS(int[] nums) {
    List<Integer> tails = new ArrayList<>();

    for (int num : nums) {
        int pos = Collections.binarySearch(tails, num);
        if (pos < 0) pos = -(pos + 1); // insertion point

        if (pos == tails.size()) {
            tails.add(num);          // extends the longest sequence
        } else {
            tails.set(pos, num);     // replaces — keeps tails minimal
        }
    }
    return tails.size();
}
```

```
nums = [10,9,2,5,3,7,101,18]

num=10: tails=[10]
num=9:  tails=[9]           (9 replaces 10, smaller tail)
num=2:  tails=[2]           (2 replaces 9)
num=5:  tails=[2,5]         (extends)
num=3:  tails=[2,3]         (3 replaces 5)
num=7:  tails=[2,3,7]       (extends)
num=101:tails=[2,3,7,101]   (extends)
num=18: tails=[2,3,7,18]    (18 replaces 101)

Final tails.size() = 4 ✅ (Note: tails itself isn't the actual LIS,
                            just used to track LENGTH correctly)
```

### BYTS Problems — Phase 3

| # | Problem | Pattern |
|---|---------|---------|
| 322 | Coin Change | Unbounded knapsack min |
| 518 | Coin Change II | Unbounded knapsack count |
| 377 | Combination Sum IV | Permutation count (loop swap) |
| 139 | Word Break | String segmentation |
| 300 | Longest Increasing Subsequence | Sequence DP |
| 509 | Fibonacci Number | Base recurrence |

---

# 🟠 Phase 4 — 2D DP — Grid Patterns

---

## 4.1 Unique Paths (LC 62)

### Concept

Count the number of ways to travel from top-left to bottom-right of an `m×n` grid, moving only RIGHT or DOWN.

```
dp[i][j] = number of ways to reach cell (i,j)
dp[i][j] = dp[i-1][j] + dp[i][j-1]
           (came from above)  (came from left)

Base cases:
  dp[i][0] = 1 for all i (only one way: go straight down)
  dp[0][j] = 1 for all j (only one way: go straight right)
```

```
3x3 grid:
┌───┬───┬───┐
│ 1 │ 1 │ 1 │
├───┼───┼───┤
│ 1 │ 2 │ 3 │
├───┼───┼───┤
│ 1 │ 3 │ 6 │
└───┴───┴───┘

dp[1][1] = dp[0][1] + dp[1][0] = 1+1 = 2
dp[1][2] = dp[0][2] + dp[1][1] = 1+2 = 3
dp[2][1] = dp[1][1] + dp[2][0] = 2+1 = 3
dp[2][2] = dp[1][2] + dp[2][1] = 3+3 = 6

Answer = 6
```

```java
int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];

    for (int i = 0; i < m; i++) dp[i][0] = 1; // left column
    for (int j = 0; j < n; j++) dp[0][j] = 1; // top row

    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    return dp[m-1][n-1];
}
```

---

## 4.2 Unique Paths II — With Obstacles (LC 63)

```java
int uniquePathsWithObstacles(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    int[][] dp = new int[m][n];

    for (int i = 0; i < m; i++) {
        if (grid[i][0] == 1) break;  // obstacle blocks the rest of column
        dp[i][0] = 1;
    }
    for (int j = 0; j < n; j++) {
        if (grid[0][j] == 1) break;  // obstacle blocks the rest of row
        dp[0][j] = 1;
    }

    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (grid[i][j] == 1) {
                dp[i][j] = 0;        // obstacle → unreachable
            } else {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
    }
    return dp[m-1][n-1];
}
```

---

## 4.3 Minimum Path Sum (LC 64)

### Concept

Find the path from top-left to bottom-right MINIMIZING the sum of values along the way.

```
dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
```

```
Grid:           dp (transformed in-place):
1 3 1           1  4  5
1 5 1    →      2  7  6
4 2 1           6  8  7

Answer = dp[2][2] = 7  (path 1→3→1→1→1)
```

```java
int minPathSum(int[][] grid) {
    int m = grid.length, n = grid[0].length;

    // Pre-fill first row and column (only one path option)
    for (int i = 1; i < m; i++) grid[i][0] += grid[i-1][0];
    for (int j = 1; j < n; j++) grid[0][j] += grid[0][j-1];

    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
        }
    }
    return grid[m-1][n-1];
}
```

---

## 4.4 Triangle (LC 120)

### Concept

Find the minimum path sum from top to bottom of a triangle, where you can move to adjacent numbers in the next row.

```
   2
  3 4
 6 5 7
4 1 8 3

Best approach: work BOTTOM-UP (avoids tracking which path you took)
dp[j] = triangle[i][j] + min(dp[j], dp[j+1])
```

```
Bottom row:      [4, 1, 8, 3]
Row 2 update:    dp[0]=6+min(4,1)=7, dp[1]=5+min(1,8)=6, dp[2]=7+min(8,3)=10
                 dp = [7, 6, 10, 3]
Row 1 update:    dp[0]=3+min(7,6)=9, dp[1]=4+min(6,10)=10
                 dp = [9, 10, 10, 3]
Row 0 update:    dp[0]=2+min(9,10)=11

Answer = 11
```

```java
int minimumTotal(List<List<Integer>> triangle) {
    int n = triangle.size();
    int[] dp = new int[n + 1]; // extra slot for boundary

    for (int i = n - 1; i >= 0; i--) {       // bottom to top
        for (int j = 0; j <= i; j++) {
            dp[j] = triangle.get(i).get(j) + Math.min(dp[j], dp[j+1]);
        }
    }
    return dp[0];
}
```

---

## 4.5 Minimum Falling Path Sum (LC 931)

### Concept

Like Triangle, but on a full square grid — you can move to the cell directly below, or diagonally below-left/below-right.

```java
int minFallingPathSum(int[][] matrix) {
    int n = matrix.length;
    int[] dp = matrix[0].clone(); // first row as starting point

    for (int i = 1; i < n; i++) {
        int[] newDp = new int[n];
        for (int j = 0; j < n; j++) {
            int best = dp[j]; // straight down
            if (j > 0)     best = Math.min(best, dp[j-1]); // below-left
            if (j < n - 1) best = Math.min(best, dp[j+1]); // below-right
            newDp[j] = matrix[i][j] + best;
        }
        dp = newDp;
    }
    return Arrays.stream(dp).min().getAsInt();
}
```

### BYTS Problems — Phase 4

| # | Problem | Pattern |
|---|---------|---------|
| 62 | Unique Paths | Grid count |
| 63 | Unique Paths II | Grid count + obstacles |
| 64 | Minimum Path Sum | Grid min |
| 120 | Triangle | Bottom-up grid |
| 931 | Minimum Falling Path Sum | 3-direction grid |

---

# 🔴 Phase 5 — 2D DP — String Matching

---

## 5.1 Longest Common Subsequence (LC 1143)

### Concept

Find the length of the longest subsequence present in BOTH strings (characters don't need to be contiguous, just in order).

```
dp[i][j] = LCS length using s1[0..i-1] and s2[0..j-1]

If s1[i-1] == s2[j-1]:
  dp[i][j] = 1 + dp[i-1][j-1]      (extend the match diagonally)
Else:
  dp[i][j] = max(dp[i-1][j], dp[i][j-1])  (skip one character)
```

```
s1 = "ace", s2 = "abcde"

      ""  a  b  c  d  e
  ""   0  0  0  0  0  0
  a    0  1  1  1  1  1
  c    0  1  1  2  2  2
  e    0  1  1  2  2  3

dp[1][1]: 'a'=='a' → dp[1][1] = 1 + dp[0][0] = 1
dp[2][3]: 'c'=='c' → dp[2][3] = 1 + dp[1][2] = 1+1 = 2
dp[3][5]: 'e'=='e' → dp[3][5] = 1 + dp[2][4] = 1+2 = 3

Answer = dp[3][5] = 3  (LCS = "ace")
```

```java
int longestCommonSubsequence(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i-1) == s2.charAt(j-1)) {
                dp[i][j] = 1 + dp[i-1][j-1];
            } else {
                dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}
```

---

## 5.2 Edit Distance (LC 72)

### Concept

Minimum operations (INSERT, DELETE, REPLACE) to convert string `s1` into `s2`.

```
dp[i][j] = min operations to convert s1[0..i-1] into s2[0..j-1]

If s1[i-1] == s2[j-1]:
  dp[i][j] = dp[i-1][j-1]                  (chars match, no op needed)
Else:
  dp[i][j] = 1 + min(
    dp[i-1][j-1],   ← REPLACE s1[i-1] with s2[j-1]
    dp[i-1][j],     ← DELETE s1[i-1]
    dp[i][j-1]      ← INSERT s2[j-1] into s1
  )

Base cases:
  dp[i][0] = i  (delete all i characters of s1)
  dp[0][j] = j  (insert all j characters of s2)
```

```
s1 = "horse", s2 = "ros"

      ""  r  o  s
  ""   0  1  2  3
  h    1  1  2  3
  o    2  2  1  2
  r    3  2  2  2
  s    4  3  3  2
  e    5  4  4  3

Answer = dp[5][3] = 3
(horse → rorse [replace h→r] → rose [delete r] → ros [delete e])
```

```java
int minDistance(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];

    for (int i = 0; i <= m; i++) dp[i][0] = i; // delete all of s1
    for (int j = 0; j <= n; j++) dp[0][j] = j; // insert all of s2

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i-1) == s2.charAt(j-1)) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = 1 + Math.min(dp[i-1][j-1],     // replace
                              Math.min(dp[i-1][j],          // delete
                                       dp[i][j-1]));        // insert
            }
        }
    }
    return dp[m][n];
}
```

---

## 5.3 Delete Operation for Two Strings (LC 583)

### Concept

Find the minimum DELETIONS (no inserts/replaces) to make two strings equal.

**Key insight:** this is just `LCS` in disguise! Keep the common subsequence, delete everything else from BOTH strings.

```
minDeletions = (len1 - LCS) + (len2 - LCS)
```

```java
int minDistance(String word1, String word2) {
    int lcs = longestCommonSubsequence(word1, word2);
    return (word1.length() - lcs) + (word2.length() - lcs);
}
```

```
word1="sea", word2="eat", LCS="ea" (length 2)
Deletions = (3-2) + (3-2) = 1 + 1 = 2
("sea"→"ea" delete s, "eat"→"ea" delete t)
```

---

## 5.4 Distinct Subsequences (LC 115)

### Concept

Count the number of distinct ways `s` can produce `t` as a subsequence (by deleting characters from `s`).

```
dp[i][j] = number of ways s[0..i-1] forms t[0..j-1]

If s[i-1] == t[j-1]:
  dp[i][j] = dp[i-1][j-1]   ← use this match
           + dp[i-1][j]      ← OR skip this char of s (don't use the match)
Else:
  dp[i][j] = dp[i-1][j]      ← must skip this char of s

Base case: dp[i][0] = 1 for all i (empty t can always be formed by deleting everything)
```

```java
int numDistinct(String s, String t) {
    int m = s.length(), n = t.length();
    int[][] dp = new int[m + 1][n + 1];

    for (int i = 0; i <= m; i++) dp[i][0] = 1; // empty t: 1 way

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            dp[i][j] = dp[i-1][j]; // always option to skip s[i-1]
            if (s.charAt(i-1) == t.charAt(j-1)) {
                dp[i][j] += dp[i-1][j-1]; // also option to use the match
            }
        }
    }
    return dp[m][n];
}
```

### BYTS Problems — Phase 5

| # | Problem | Pattern |
|---|---------|---------|
| 1143 | Longest Common Subsequence | Classic 2D string DP |
| 72 | Edit Distance | 3-way min 2D DP |
| 583 | Delete Operation for Two Strings | LCS-based |
| 115 | Distinct Subsequences | Counting 2D DP |
| 97 | Interleaving String | 2D string DP |

---

# 🟤 Phase 6 — Knapsack Family

---

## 6.1 0/1 Knapsack — Theory

### Concept

N items, each with a weight and value. Capacity W. Each item used AT MOST ONCE. Maximize total value.

```
dp[i][w] = max value using first i items, with capacity w

If weight[i-1] > w:
  dp[i][w] = dp[i-1][w]                              (can't fit, skip)
Else:
  dp[i][w] = max(
    dp[i-1][w],                                       (skip item i)
    value[i-1] + dp[i-1][w - weight[i-1]]              (take item i)
  )
```

```java
int knapsack(int W, int[] wt, int[] val, int n) {
    int[][] dp = new int[n + 1][W + 1];

    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            dp[i][w] = dp[i-1][w]; // default: skip item i
            if (wt[i-1] <= w) {
                dp[i][w] = Math.max(dp[i][w],
                            val[i-1] + dp[i-1][w - wt[i-1]]);
            }
        }
    }
    return dp[n][W];
}
```

```
weights=[1,3,4,5], values=[1,4,5,7], W=7

      w: 0  1  2  3  4  5  6  7
i=0:      0  0  0  0  0  0  0  0
i=1(w1,v1):0  1  1  1  1  1  1  1
i=2(w3,v4):0  1  1  4  5  5  5  5
i=3(w4,v5):0  1  1  4  5  6  6  9
i=4(w5,v7):0  1  1  4  5  7  8  9

Answer = dp[4][7] = 9  (items with weight 3+4=7, value 4+5=9)
```

---

## 6.2 Partition Equal Subset Sum (LC 416)

### Concept

Can the array be split into two subsets with EQUAL sum? This IS 0/1 Knapsack: "can we pick a subset summing to `totalSum/2`?"

```
nums = [1, 5, 11, 5], total=22, target=11

dp[j] = true if some subset sums to exactly j
dp[0] = true (empty subset)

Process 1:  dp[1]=true
Process 5:  dp[5]=true, dp[6]=true (1+5)
Process 11: dp[11]=true ← TARGET REACHED!
Process 5:  (continues, but we already know answer)
```

```java
boolean canPartition(int[] nums) {
    int sum = Arrays.stream(nums).sum();
    if (sum % 2 != 0) return false;  // odd sum can't split evenly
    int target = sum / 2;

    boolean[] dp = new boolean[target + 1];
    dp[0] = true;

    for (int num : nums) {
        // ⚠️ REVERSE loop — 0/1 knapsack, no reuse
        for (int j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }
    return dp[target];
}
```

---

## 6.3 Target Sum (LC 494)

### Concept

Assign `+` or `-` to each number to reach a target sum. Count the ways.

**Key transformation:** let `P` = sum of positive-assigned numbers, `N` = sum of negative-assigned numbers.
```
P - N = target
P + N = totalSum
→ 2P = target + totalSum
→ P = (target + totalSum) / 2
```

This becomes: **count subsets that sum to `P`** — a counting 0/1 Knapsack!

```java
int findTargetSumWays(int[] nums, int target) {
    int sum = Arrays.stream(nums).sum();
    if (Math.abs(target) > sum || (sum + target) % 2 != 0) return 0;

    int P = (sum + target) / 2; // subset that should sum to P

    int[] dp = new int[P + 1];
    dp[0] = 1; // one way: empty subset sums to 0

    for (int num : nums) {
        for (int j = P; j >= num; j--) {  // reverse — 0/1 knapsack
            dp[j] += dp[j - num];
        }
    }
    return dp[P];
}
```

---

## 6.4 Maximal Square (LC 221)

### Concept

Find the largest square of all-1s in a binary matrix.

```
dp[i][j] = side length of the LARGEST SQUARE
           with bottom-right corner at (i,j)

If matrix[i][j] == '1':
  dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
             (limited by the SMALLEST of the three neighbors)
```

```
Matrix:          dp:
1 0 1 0 0        1 0 1 0 0
1 0 1 1 1        1 0 1 1 1
1 1 1 1 1        1 1 2 2 2
1 0 0 1 0        1 0 0 1 0

dp[2][2]=1+min(dp[1][2],dp[2][1],dp[1][1])=1+min(1,1,0)=1
dp[2][3]=1+min(dp[1][3],dp[2][2],dp[1][2])=1+min(1,1,1)=2
dp[2][4]=1+min(dp[1][4],dp[2][3],dp[1][3])=1+min(1,2,1)=2

maxSide = 2, answer = 2² = 4
```

```java
int maximalSquare(char[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] dp = new int[m + 1][n + 1];
    int maxSide = 0;

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (matrix[i-1][j-1] == '1') {
                dp[i][j] = 1 + Math.min(dp[i-1][j],
                            Math.min(dp[i][j-1], dp[i-1][j-1]));
                maxSide = Math.max(maxSide, dp[i][j]);
            }
        }
    }
    return maxSide * maxSide;
}
```

### BYTS Problems — Phase 6

| # | Problem | Pattern |
|---|---------|---------|
| 416 | Partition Equal Subset Sum | 0/1 Knapsack boolean |
| 494 | Target Sum | 0/1 Knapsack count |
| 221 | Maximal Square | Grid DP, 3-way min |
| 1277 | Count Square Submatrices | Grid DP, sum variant |

---

# ⚫ Phase 7 — Tree DP & Special DP

---

## 7.1 House Robber II — Circular Array (LC 213)

### Concept

Same as House Robber, but houses are arranged in a CIRCLE — house 0 and house n-1 are ADJACENT.

**Key insight:** since house 0 and house n-1 can't BOTH be robbed, run House Robber TWICE:
1. Excluding the LAST house (range `[0, n-2]`)
2. Excluding the FIRST house (range `[1, n-1]`)

Take the MAX of both results.

```
houses = [2, 3, 2]  (circular: house2 and house0 are adjacent)

Case A (exclude last, range [0,1]): rob([2,3]) = max(2,3) = 3
Case B (exclude first, range [1,2]): rob([3,2]) = max(3,2) = 3

Answer = max(3, 3) = 3
```

```java
int rob(int[] nums) {
    int n = nums.length;
    if (n == 1) return nums[0];

    return Math.max(
        robRange(nums, 0, n - 2),  // exclude last house
        robRange(nums, 1, n - 1)   // exclude first house
    );
}

int robRange(int[] nums, int start, int end) {
    int prev2 = 0, prev1 = 0;
    for (int i = start; i <= end; i++) {
        int curr = Math.max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

---

## 7.2 House Robber III — Tree (LC 337)

### Concept

Houses are arranged as a BINARY TREE. Cannot rob two DIRECTLY CONNECTED houses (parent-child).

**Key insight:** at each node, return a PAIR of values:
- `robbed`  = max money if we ROB this node (children can't be robbed)
- `skipped` = max money if we DON'T rob this node (children CAN be robbed or skipped — take whichever is better for each)

```java
int rob(TreeNode root) {
    int[] result = dfs(root);
    return Math.max(result[0], result[1]);
}

// returns {maxIfRobbed, maxIfSkipped}
int[] dfs(TreeNode node) {
    if (node == null) return new int[]{0, 0};

    int[] left  = dfs(node.left);
    int[] right = dfs(node.right);

    int robbed  = node.val + left[1] + right[1]; // children must be skipped
    int skipped = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

    return new int[]{robbed, skipped};
}
```

```
Tree:        3
            / \
           2   3
            \   \
             3   1

Leaf "3"(under 2): {robbed=3, skipped=0}
Leaf "1":          {robbed=1, skipped=0}
Node 2:  robbed=2+0+0=2, skipped=max(0,0)+max(3,0)=3 → {2,3}
Node 3(right): robbed=3+0+0=3, skipped=max(0,0)+max(1,0)=1 → {3,1}
Root 3:  robbed=3+3+1=7, skipped=max(2,3)+max(3,1)=3+3=6 → {7,6}

Answer = max(7,6) = 7
```

---

## 7.3 Delete and Earn (LC 740)

### Concept

Pick a number, earn `num × count(num)` points, but ALL instances of `num-1` and `num+1` get deleted (can't pick them).

**Key insight:** this is secretly HOUSE ROBBER on a transformed array! Group by value, build a "points per value" array, then apply House Robber logic (since you can't pick adjacent VALUES).

```
nums = [3, 4, 2]

Step 1: sum points by value
  value:  2   3   4
  points: 2   3   4

Step 2: this is House Robber on [2, 3, 4]
  (can't pick adjacent values 2&3, or 3&4)

dp[2]=2
dp[3]=max(2,3)=3
dp[4]=max(3,2+4)=6

Answer = 6 (pick value 2 and value 4 → 2+4=6)
```

```java
int deleteAndEarn(int[] nums) {
    int maxVal = Arrays.stream(nums).max().getAsInt();
    int[] points = new int[maxVal + 1];

    for (int num : nums) {
        points[num] += num; // accumulate total points for this value
    }

    // Now apply House Robber on the points[] array
    int prev2 = 0, prev1 = 0;
    for (int p : points) {
        int curr = Math.max(prev1, prev2 + p);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

---

## 7.4 Russian Doll Envelopes (LC 354) — Hard

### Concept

Envelopes nest inside each other if BOTH width and height are strictly smaller. Find the maximum number of envelopes you can nest (like Russian dolls).

**Key insight:** this is LIS in 2 dimensions! Sort by width ASCENDING. For TIES in width, sort by height DESCENDING (this prevents counting same-width envelopes as nestable). Then find LIS on the heights array.

```
envelopes = [[5,4],[6,4],[6,7],[2,3]]

Sort: width asc, height desc for ties
  [2,3], [5,4], [6,7], [6,4]
                         ↑ same width 6, height desc: 7 before 4

Extract heights: [3, 4, 7, 4]
Find LIS:        [3, 4, 7] → length 3 (the duplicate-width 4 can't extend past 7)
```

```java
int maxEnvelopes(int[][] envelopes) {
    // Sort: width ascending, height DESCENDING for ties
    Arrays.sort(envelopes, (a, b) ->
        a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);

    // LIS on heights (using O(N log N) approach)
    List<Integer> tails = new ArrayList<>();
    for (int[] env : envelopes) {
        int h = env[1];
        int pos = Collections.binarySearch(tails, h);
        if (pos < 0) pos = -(pos + 1);

        if (pos == tails.size()) tails.add(h);
        else tails.set(pos, h);
    }
    return tails.size();
}
```

### BYTS Problems — Phase 7

| # | Problem | Pattern |
|---|---------|---------|
| 213 | House Robber II | Circular, run twice |
| 337 | House Robber III | Tree DP, return pair |
| 740 | Delete and Earn | Transform to House Robber |
| 354 | Russian Doll Envelopes | Sort + LIS in 2D |

---

# 🔵 Phase 8 — Hard / Interval DP

---

## 8.1 Burst Balloons (LC 312) — Hard

### Concept

Burst all balloons to maximize coins. Bursting balloon `i` gives `nums[left] × nums[i] × nums[right]` coins, where left/right are the CURRENT neighbors (after previous bursts).

**Key insight — think BACKWARDS:** instead of "which balloon do I burst first", think "which balloon do I burst LAST in range [left, right]". This makes the subproblems independent.

```
dp[left][right] = max coins from bursting all balloons strictly between left and right

For each k in (left, right), assume k is burst LAST:
  dp[left][right] = max over k of:
    dp[left][k] + dp[k][right] + nums[left]*nums[k]*nums[right]

Pad the array with 1 on both sides (virtual boundary balloons)
```

```java
int maxCoins(int[] nums) {
    int n = nums.length;
    int[] balloons = new int[n + 2];
    balloons[0] = balloons[n + 1] = 1; // virtual boundary balloons
    for (int i = 0; i < n; i++) balloons[i + 1] = nums[i];

    int[][] dp = new int[n + 2][n + 2];

    // length = size of the gap between left and right
    for (int length = 2; length <= n + 1; length++) {
        for (int left = 0; left + length <= n + 1; left++) {
            int right = left + length;
            for (int k = left + 1; k < right; k++) {
                int coins = balloons[left] * balloons[k] * balloons[right]
                          + dp[left][k] + dp[k][right];
                dp[left][right] = Math.max(dp[left][right], coins);
            }
        }
    }
    return dp[0][n + 1];
}
```

```
nums = [3,1,5,8], padded = [1,3,1,5,8,1]

Think: which balloon to burst LAST in range (0,5)?
If balloon 8 (index 4) is burst last:
  coins = balloons[0]*balloons[4]*balloons[5] + dp[0][4] + dp[4][5]
        = 1*8*1 + (best of left part) + (best of right part)

This recursive structure builds up from small ranges to the full range.
```

---

## 8.2 Remove Boxes (LC 546) — Hard

### Concept

Remove boxes of the same color in groups. Removing `k` consecutive same-color boxes gives `k²` points. Maximize total points.

This uses 3D DP: `dp[i][j][k]` = max points for boxes `[i,j]` where there are `k` extra boxes of `boxes[i]`'s color attached to the LEFT of `i`.

```java
int removeBoxes(int[] boxes) {
    int n = boxes.length;
    int[][][] dp = new int[n][n][n]; // dp[i][j][k]
    return calculatePoints(boxes, dp, 0, n - 1, 0);
}

int calculatePoints(int[] boxes, int[][][] dp, int i, int j, int k) {
    if (i > j) return 0;
    if (dp[i][j][k] != 0) return dp[i][j][k];

    // Option 1: remove boxes[i] along with the k attached same-color boxes now
    int origI = i, origK = k;
    while (i + 1 <= j && boxes[i + 1] == boxes[i]) {
        i++; k++; // extend the same-color group, merge into k count
    }

    int result = (k + 1) * (k + 1) + calculatePoints(boxes, dp, i + 1, j, 0);

    // Option 2: don't remove boxes[i] yet — find a LATER same-color box
    //           to merge with, removing what's between them first
    for (int m = i + 1; m <= j; m++) {
        if (boxes[m] == boxes[i]) {
            result = Math.max(result,
                calculatePoints(boxes, dp, i + 1, m - 1, 0)
              + calculatePoints(boxes, dp, m, j, k + 1));
        }
    }

    dp[origI][j][origK] = result;
    return result;
}
```

> This is one of the hardest DP problems on LeetCode — understand Burst Balloons thoroughly first before attempting this.

---

## 8.3 Count Square Submatrices with All Ones (LC 1277)

### Concept

Count the TOTAL number of square submatrices made entirely of 1s.

**Key insight:** same `dp[i][j]` formula as Maximal Square — but instead of tracking the MAX side, SUM every `dp[i][j]` value (because `dp[i][j]` = number of distinct squares ENDING at that cell, from size 1×1 up to size `dp[i][j]`×`dp[i][j]`).

```
Matrix:
1 0 1
1 1 0
1 1 0

dp (same computation as Maximal Square):
1 0 1
1 1 0
1 2 0

Sum of all dp values = 1+0+1+1+1+0+1+2+0 = 7
Answer = 7
```

```java
int countSquares(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] dp = new int[m][n];
    int count = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == 1) {
                if (i == 0 || j == 0) {
                    dp[i][j] = 1; // first row/col: can only be 1x1
                } else {
                    dp[i][j] = 1 + Math.min(dp[i-1][j],
                                Math.min(dp[i][j-1], dp[i-1][j-1]));
                }
                count += dp[i][j]; // ⭐ ADD, don't just track max
            }
        }
    }
    return count;
}
```

### BYTS Problems — Phase 8

| # | Problem | Pattern |
|---|---------|---------|
| 312 | Burst Balloons | Interval DP, burst last |
| 546 | Remove Boxes | 3D interval DP |
| 1277 | Count Square Submatrices | Grid DP, sum variant |

---

# 📋 Master Java Syntax Sheet — Dynamic Programming

```java
// ── ARRAY SETUP ────────────────────────────────────────────────
int[] dp = new int[n + 1];
int[][] dp = new int[m + 1][n + 1];
boolean[] dp = new boolean[n + 1];
Arrays.fill(dp, Integer.MAX_VALUE);     // infinity for MIN problems
Arrays.fill(dp, amount + 1);            // safe infinity for coin change

// ── BASE CASES (set explicitly, don't rely on default 0) ──────
dp[0] = 1;          // often: "one way to do nothing"
for (int i = 0; i <= m; i++) dp[i][0] = i;  // edit distance style
for (int j = 0; j <= n; j++) dp[0][j] = j;

// ── 1D RECURRENCE TEMPLATES ─────────────────────────────────────
dp[i] = dp[i-1] + dp[i-2];                          // Fibonacci
dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);       // House Robber
dp[i] = Math.min(dp[i], dp[i-coin] + 1);            // Coin Change (min)
dp[i] += dp[i-coin];                                 // Coin Change (count)

// ── KADANE'S TEMPLATE ─────────────────────────────────────────
int sum = 0, maxSum = Integer.MIN_VALUE;
for (int num : nums) {
    sum += num;
    maxSum = Math.max(maxSum, sum);
    if (sum < 0) sum = 0;
}

// ── 2D STRING DP TEMPLATE ───────────────────────────────────────
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (s1.charAt(i-1) == s2.charAt(j-1)) {
            dp[i][j] = dp[i-1][j-1] + 1;            // LCS-style match
        } else {
            dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]); // skip one
        }
    }
}

// ── GRID DP TEMPLATE ──────────────────────────────────────────
for (int i = 1; i < m; i++) {
    for (int j = 1; j < n; j++) {
        dp[i][j] = dp[i-1][j] + dp[i][j-1];          // count paths
        // OR: dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1]); // min sum
    }
}

// ── 0/1 KNAPSACK (REVERSE LOOP) ───────────────────────────────
for (int num : nums) {
    for (int j = target; j >= num; j--) {
        dp[j] = dp[j] || dp[j - num];   // boolean version
        // OR: dp[j] = Math.max(dp[j], val + dp[j - wt]); // value version
    }
}

// ── UNBOUNDED KNAPSACK (FORWARD LOOP) ──────────────────────────
for (int coin : coins) {
    for (int j = coin; j <= amount; j++) {
        dp[j] += dp[j - coin];          // count ways
        // OR: dp[j] = Math.min(dp[j], dp[j-coin]+1); // min coins
    }
}

// ── TREE DP — RETURN PAIR PATTERN ─────────────────────────────
int[] dfs(TreeNode node) {
    if (node == null) return new int[]{0, 0};
    int[] left = dfs(node.left);
    int[] right = dfs(node.right);
    int option1 = /* ... */;
    int option2 = /* ... */;
    return new int[]{option1, option2};
}

// ── SPACE OPTIMIZATION (rolling variables) ──────────────────────
int prev2 = base0, prev1 = base1;
for (int i = 2; i <= n; i++) {
    int curr = /* recurrence using prev1, prev2 */;
    prev2 = prev1;
    prev1 = curr;
}
return prev1;

// ── BINARY SEARCH FOR O(N LOG N) LIS ───────────────────────────
List<Integer> tails = new ArrayList<>();
for (int num : nums) {
    int pos = Collections.binarySearch(tails, num);
    if (pos < 0) pos = -(pos + 1);
    if (pos == tails.size()) tails.add(num);
    else tails.set(pos, num);
}
return tails.size();

// ── INTERVAL DP TEMPLATE (burst balloons style) ────────────────
for (int length = 2; length <= n; length++) {
    for (int left = 0; left + length <= n; left++) {
        int right = left + length;
        for (int k = left + 1; k < right; k++) {
            dp[left][right] = Math.max(dp[left][right],
                dp[left][k] + dp[k][right] + cost(left, k, right));
        }
    }
}
```

---

# 🧭 Pattern Decision Table

```
SIGNAL IN PROBLEM                          → PATTERN
────────────────────────────────────────────────────────────────
"Number of ways to reach/climb..."         → Fibonacci-style 1D DP
"Max sum, no adjacent elements"            → House Robber
"Max subarray sum"                         → Kadane's
"Max product subarray"                     → Kadane's + track min
"String segmentation (word break)"         → 1D boolean DP
"Min coins to make amount"                 → Unbounded knapsack (min)
"Number of ways to make amount"            → Unbounded knapsack (count)
"Longest increasing subsequence"           → 1D DP or O(N log N) tails
"Grid, only right/down moves"              → 2D grid DP
"Two strings, common subsequence"          → 2D LCS-style DP
"Two strings, transform one to other"      → 2D edit distance DP
"Can partition into equal subsets"         → 0/1 Knapsack boolean
"Assign +/- to reach target"               → 0/1 Knapsack count (transform)
"Largest square in binary matrix"          → Grid DP, 3-way min
"Circular array, no adjacent"              → House Robber, run twice
"Tree, rob non-adjacent (parent-child)"    → Tree DP, return [rob,skip]
"Envelopes/boxes nesting (2D LIS)"         → Sort + LIS
"Burst/remove items, neighbor-dependent"   → Interval DP (think LAST step)
"Count submatrices/subsquares"             → Grid DP, sum instead of max
```

---

# 📚 All BYTS DP Problems by Phase

**Phase 2 (1D Linear):** 53, 70, 91, 121, 122, 152, 198, 746

**Phase 3 (1D Unbounded):** 139, 300, 322, 377, 509, 518

**Phase 4 (2D Grid):** 62, 63, 64, 120, 931

**Phase 5 (2D String):** 72, 97, 115, 583, 1143

**Phase 6 (Knapsack):** 221, 416, 494, 1277

**Phase 7 (Tree DP & Special):** 213, 337, 354, 740

**Phase 8 (Hard/Interval):** 312, 546, 1277

---

## Full BYTS DP Problem List

| # | Problem | Pattern | Difficulty |
|---|---------|---------|------------|
| 45 | Jump Game II | Greedy/DP | Med |
| 53 | Maximum Subarray | Kadane's | Med |
| 55 | Jump Game | Greedy/DP | Med |
| 62 | Unique Paths | Grid DP | Med |
| 63 | Unique Paths II | Grid DP | Med |
| 64 | Minimum Path Sum | Grid DP | Med |
| 70 | Climbing Stairs | Fibonacci | Easy |
| 72 | Edit Distance | 2D String DP | Med |
| 91 | Decode Ways | 1D String DP | Med |
| 97 | Interleaving String | 2D String DP | Med |
| 115 | Distinct Subsequences | 2D Counting DP | Hard |
| 120 | Triangle | Grid DP | Med |
| 121 | Best Time to Buy/Sell Stock | 1D DP | Easy |
| 122 | Best Time to Buy/Sell Stock II | Greedy/DP | Med |
| 139 | Word Break | 1D String DP | Med |
| 152 | Maximum Product Subarray | Kadane's variant | Med |
| 198 | House Robber | Non-adjacent | Med |
| 213 | House Robber II | Circular | Med |
| 221 | Maximal Square | Grid DP | Med |
| 300 | Longest Increasing Subsequence | Sequence DP | Med |
| 312 | Burst Balloons | Interval DP | Hard |
| 322 | Coin Change | Unbounded Knapsack | Med |
| 337 | House Robber III | Tree DP | Med |
| 338 | Counting Bits | Bit DP | Easy |
| 354 | Russian Doll Envelopes | Sort + LIS | Hard |
| 377 | Combination Sum IV | Permutation count | Med |
| 416 | Partition Equal Subset Sum | 0/1 Knapsack | Med |
| 494 | Target Sum | 0/1 Knapsack count | Med |
| 509 | Fibonacci Number | Base recurrence | Easy |
| 518 | Coin Change II | Unbounded count | Med |
| 546 | Remove Boxes | 3D Interval DP | Hard |
| 583 | Delete Operation for Two Strings | LCS-based | Med |
| 740 | Delete and Earn | Transform to House Robber | Med |
| 746 | Min Cost Climbing Stairs | Fibonacci variant | Easy |
| 931 | Minimum Falling Path Sum | Grid DP | Med |
| 1143 | Longest Common Subsequence | 2D String DP | Med |
| 1277 | Count Square Submatrices | Grid DP sum | Med |

---

*Updated: 2026-06-18 | Java | BYTS Sheet*
*All DP patterns: 1D Linear · Unbounded · Grid · String Matching · Knapsack · Tree DP · Interval DP*
