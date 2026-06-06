# 📚 Java Arrays & Strings Handbook: Mastering Patterns for FAANG/Unicorn SDE Interviews

**Language:** Java | **Level:** Complete Beginner → SDE Ready
**Style:** Concept → Visual → Code → Practice Problems

> You have minimal LeetCode practice.
> This handbook teaches you every essential array/string concept from scratch,
> in the exact order an SDE interview expects you to know them.

---

# 📌 Contents

**Phase 1 — Arrays & Strings Foundations**
- 1.1 What are Arrays & Strings?
- 1.2 Common Operations & Java Syntax
- 1.3 Java Syntax Reference

**Phase 2 — Two Pointers Technique**
- 2.1 Two Pointers (Basic)
- 2.2 Two Pointers (Sorted Array)
- 2.3 Two Pointers (Fast & Slow for Loops)
- 2.4 Two Pointers (Trapping Rainwater, Container With Most Water)

**Phase 3 — Sliding Window**
- 3.1 Fixed Size Sliding Window
- 3.2 Variable Size Sliding Window
- 3.3 Sliding Window Applications (Anagrams, Substrings)

**Phase 4 — Modified Binary Search**
- 4.1 Binary Search on Sorted Array
- 4.2 Search in Rotated Sorted Array
- 4.3 Find Peak Element
- 4.4 Lower/Upper Bound

**Phase 5 — Prefix Sums & Advanced Patterns**
- 5.1 Prefix Sum
- 5.2 Difference Array
- 5.3 KMP for Strings
- 5.4 Bit Manipulation for Arrays/Strings

---

# 🟢 Phase 1 — Arrays & Strings Foundations

---

## 1.1 What are Arrays & Strings?

### Real-World Analogy

Think of an array like a row of lockers:
- **Index:** The locker number (0, 1, 2, ...)
- **Element:** The item stored in the locker

A string is like a special kind of array where each "locker" holds a character.

```
Array Example: [10, 20, 30, 40]
Index:          0   1   2   3
Lockers:        [10][20][30][40]

String Example: "hello"
Index:          0  1  2  3  4
Lockers:        [h][e][l][l][o]
```

Arrays and strings are the most fundamental data structures in programming and form the basis for a significant portion of coding interview questions, especially at companies like Amazon and Google.

### Core Vocabulary

```
Array Length/Size  → Total number of elements (e.g., arr.length in Java)
Index            → Position of an element (starts from 0)
Element          → Value stored at an index
Subarray         → Contiguous sequence of elements within an array
Substring        → Contiguous sequence of characters within a string
Mutable          → Can be changed after creation (Java arrays are mutable, Strings are immutable)
Immutable        → Cannot be changed after creation (Java String is immutable)
```

### Visual Example

```
Original Array: [5, 2, 8, 1, 9]
Indices:         0  1  2  3  4

Subarray from index 1 to 3: [2, 8, 1]
Length of subarray: 3

Original String: "algorithm"
Indices:           0 1 2 3 4 5 6 7 8

Substring from index 2 to 5: "gori"
Length of substring: 4
```

---

## 1.2 Common Operations & Java Syntax

### Array Operations

```java
// Creating an array
int[] arr = new int[5]; // Creates array of 5 integers, all initialized to 0
int[] arr2 = {1, 2, 3, 4, 5}; // Creates and initializes

// Accessing elements
int firstElement = arr2[0]; // Gets value at index 0
arr2[2] = 100; // Sets value at index 2 to 100

// Getting length
int length = arr.length; // Note: no parentheses for length

// Iterating through an array
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

// Enhanced for loop (for-each)
for (int num : arr) {
    System.out.println(num);
}
```

### String Operations

```java
String s1 = "Hello";
String s2 = "World";

// Concatenation
String s3 = s1 + " " + s2; // "Hello World"
String s4 = s1.concat(" ").concat(s2); // Alternative way

// Length
int len = s1.length(); // 5

// Character access
char c = s1.charAt(0); // 'H'

// Substring
String sub = s1.substring(1, 4); // "ell" (from index 1, up to but not including 4)
String sub2 = s1.substring(1); // "ello" (from index 1 to end)

// Searching
int pos = s1.indexOf('e'); // 1 (first occurrence)
int pos2 = s1.indexOf("ll"); // 2
boolean contains = s1.contains("el"); // true

// Converting
char[] charArray = s1.toCharArray(); // Convert to char array
String fromArray = new String(charArray); // Convert back from char array

// Immutability: s1 itself is unchanged by operations like concat, substring
```

### StringBuilder for Efficiency

When concatenating strings in a loop, use `StringBuilder` to avoid creating many temporary string objects.

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" ");
sb.append("World");
String result = sb.toString(); // "Hello World"

// Or initialize with capacity for better performance
StringBuilder sb2 = new StringBuilder(initialCapacity);
```

---

## 1.3 Java Syntax Reference — Arrays & Strings

```java
// ── ARRAY CREATION ──────────────────────────────────────────────
int[] arr = new int[n]; // Array of n integers, initialized to 0
int[] arr2 = {1, 2, 3}; // Initialize with values

// ── ARRAY LENGTH ────────────────────────────────────────────────
int len = arr.length; // No parentheses!

// ── ITERATION ───────────────────────────────────────────────────
for (int i = 0; i < arr.length; i++) { /* ... */ }
for (int val : arr) { /* ... */ } // Enhanced for loop

// ── STRING CREATION ─────────────────────────────────────────────
String s = "literal";
String s2 = new String("object");

// ── STRING OPERATIONS ───────────────────────────────────────────
int lenStr = s.length();
char c = s.charAt(index);
String sub = s.substring(start, end_exclusive);
String sub2 = s.substring(start); // To end
boolean contains = s.contains(substring);
int indexOf = s.indexOf(char_or_string);
String upper = s.toUpperCase();
String lower = s.toLowerCase();

// ── STRINGBUILDER ───────────────────────────────────────────────
StringBuilder sb = new StringBuilder();
sb.append(value);
String result = sb.toString();

// ── CHARACTER OPERATIONS ────────────────────────────────────────
char ch = 'a';
boolean isDigit = Character.isDigit(ch);
boolean isLetter = Character.isLetter(ch);
boolean isUpper = Character.isUpperCase(ch);
char upperCh = Character.toUpperCase(ch);
char lowerCh = Character.toLowerCase(ch);

// ── ARRAYLIST (dynamic array) ───────────────────────────────────
List<Integer> list = new ArrayList<>();
list.add(element);
list.get(index);
list.size();
list.remove(index);
```

---

# 🔵 Phase 2 — Two Pointers Technique

---

## 2.1 Two Pointers (Basic)

### Concept

The Two Pointers technique uses two indices (pointers) to traverse an array or string simultaneously, often from different directions or at different speeds. This avoids the need for nested loops in many scenarios.

**Use Cases:**
- Finding pairs that sum to a target
- Removing duplicates in-place
- Reversing an array/string

### Visual Trace

```
Array: [1, 2, 3, 4, 5, 6]
Goal:  Reverse the array in-place

Initial: [1, 2, 3, 4, 5, 6]
         ↑              ↑
       left           right

Step 1:  Swap arr[left] and arr[right]. Increment left, Decrement right.
         [6, 2, 3, 4, 5, 1]
            ↑        ↑
          left    right

Step 2:  Swap again.
         [6, 5, 3, 4, 2, 1]
               ↑  ↑
             left right

Step 3:  left >= right, stop.
         [6, 5, 4, 3, 2, 1] - Reversed!
```

### Basic Template

```java
// Two pointers moving towards each other
int left = 0;
int right = array.length - 1;

while (left < right) {
    // Process elements at left and right
    // Usually swap or compare

    // Move pointers
    left++;
    right--;
}
```

### Example: Reverse String In-Place

```java
public void reverseString(char[] s) {
    int left = 0;
    int right = s.length - 1;

    while (left < right) {
        // Swap characters
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;

        left++;
        right--;
    }
}
```

---

## 2.2 Two Pointers (Sorted Array)

### Concept

When the input array is sorted, two pointers can efficiently find pairs that meet a specific condition (e.g., sum equals a target). This is often faster than a brute-force O(n²) approach.

### Visual Trace

```
Array: [2, 7, 11, 15], Target = 9

Initial: [2, 7, 11, 15]
         ↑  ↑
       left right
Sum = 2 + 7 = 9. Found!

If sum was less than target, move left pointer right to increase sum.
If sum was greater than target, move right pointer left to decrease sum.
```

### Two Sum Sorted Template

```java
// Find two numbers that sum to target in a sorted array
// Returns indices (1-indexed) or [-1, -1] if not found
public int[] twoSum(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;

    while (left < right) {
        int currentSum = numbers[left] + numbers[right];

        if (currentSum == target) {
            return new int[]{left + 1, right + 1}; // 1-indexed result
        } else if (currentSum < target) {
            left++; // Need a larger sum
        } else {
            right--; // Need a smaller sum
        }
    }

    return new int[]{-1, -1}; // Not found
}
```

### Three Sum (Extension)

```java
// Find all unique triplets that sum to zero
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(nums); // Sort first

    for (int i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for the first number
        if (i > 0 && nums[i] == nums[i - 1]) continue;

        int left = i + 1;
        int right = nums.length - 1;

        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];

            if (sum == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                // Skip duplicates for left and right
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;

                left++;
                right--;
            } else if (sum < 0) {
                left++; // Need a larger sum
            } else {
                right--; // Need a smaller sum
            }
        }
    }

    return result;
}
```

---

## 2.3 Two Pointers (Fast & Slow for Loops)

### Concept

Also known as the Floyd's Cycle-Finding Algorithm or Tortoise and Hare algorithm. Used to detect cycles in linked lists or arrays, but the principle applies to arrays representing linked structures implicitly.

### Visual Trace (Conceptual for Array as Linked List)

```
Imagine array [1, 3, 4, 2, 2] where value at index i points to nums[i].
Index: 0 1 2 3 4
Value: 1 3 4 2 2
Start at index 0 -> 1 -> 3 -> 2 -> 4 -> 2 (cycle!)

Fast pointer moves 2 steps, slow moves 1.
They will meet inside the cycle if one exists.
```

### Cycle Detection Template (Applied to Array Values as Indices)

```java
// Find the duplicate number in an array of size n+1 with numbers 1 to n
// Treat array values as pointers/indexes. Duplicate means a cycle exists.
public int findDuplicate(int[] nums) {
    // Phase 1: Detect if cycle exists
    int slow = nums[0];
    int fast = nums[0];

    do {
        slow = nums[slow]; // Move 1 step
        fast = nums[nums[fast]]; // Move 2 steps
    } while (slow != fast);

    // Phase 2: Find the start of the cycle (the duplicate number)
    slow = nums[0]; // Reset one pointer to start
    while (slow != fast) {
        slow = nums[slow]; // Move both 1 step
        fast = nums[fast];
    }

    return slow; // Meeting point is the duplicate number
}
```

---

## 2.4 Two Pointers (Trapping Rainwater, Container With Most Water)

### Container With Most Water

**Problem:** Given an array of heights, find two lines that form a container holding the most water.

**Concept:** Use two pointers from both ends. The area is determined by the shorter line and the distance between them. Move the pointer pointing to the shorter line inward, hoping to find a taller line that might yield a larger area.

```java
// Container With Most Water
public int maxArea(int[] height) {
    int left = 0;
    int right = height.length - 1;
    int maxWater = 0;

    while (left < right) {
        int h = Math.min(height[left], height[right]);
        int w = right - left;
        int currentWater = h * w;
        maxWater = Math.max(maxWater, currentWater);

        // Move the pointer with the smaller height
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return maxWater;
}
```

**Visual Trace:**
```
Heights: [1, 8, 6, 2, 5, 4, 8, 3, 7]
Indices:  0  1  2  3  4  5  6  7  8
Initial: left=0 (height=1), right=8 (height=7)
Area = min(1,7) * (8-0) = 1 * 8 = 8
Move left (1 < 7) -> left=1 (height=8)
New Area = min(8,7) * (8-1) = 7 * 7 = 49...
Continue until left >= right.
```

### Trapping Rainwater (Bonus - Related)

```java
// Trapping Rainwater - Uses two pointers from outside moving in
public int trap(int[] height) {
    if (height == null || height.length <= 2) return 0;

    int left = 0, right = height.length - 1;
    int leftMax = 0, rightMax = 0;
    int totalWater = 0;

    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                totalWater += leftMax - height[left];
            }
            left++;
        } else {
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                totalWater += rightMax - height[right];
            }
            right--;
        }
    }

    return totalWater;
}
```

### LeetCode Practice — Phase 2

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 344 | Reverse String | Basic Two Pointers | Easy |
| 125 | Valid Palindrome | Two Pointers (skip non-alphanumeric) | Easy |
| 167 | Two Sum II - Input Array Is Sorted | Sorted Array Two Sum | Medium |
| 15 | 3Sum | Three Pointers (sorted array) | Medium |
| 11 | Container With Most Water | Area Optimization | Medium |
| 141 | Linked List Cycle | Fast & Slow Pointers (Cycle Detection) | Easy |
| 287 | Find the Duplicate Number | Fast & Slow Pointers (Array Cycle) | Medium |
| 1 | Two Sum | Hash Map (but good to contrast) | Easy |

---

# 🟡 Phase 3 — Sliding Window

---

## 3.1 Fixed Size Sliding Window

### Concept

Maintain a window of a specific, constant size `k` that slides across the array or string. Useful for problems like finding the maximum/minimum sum of a subarray of size `k`, or checking if any subarray matches a specific pattern of length `k`.

### Visual Trace

```
Array: [1, 4, 2, 10, 23, 3, 1, 0, 20], k = 4
Goal: Find maximum sum of subarray of size k

Initial Window [1, 4, 2, 10]: Sum = 17
Slide to [4, 2, 10, 23]: Sum = old_sum - 1 + 23 = 17 - 1 + 23 = 39
Slide to [2, 10, 23, 3]: Sum = 39 - 4 + 3 = 38
Slide to [10, 23, 3, 1]: Sum = 38 - 2 + 1 = 37
Slide to [23, 3, 1, 0]: Sum = 37 - 10 + 0 = 27
Slide to [3, 1, 0, 20]: Sum = 27 - 23 + 20 = 24

Maximum sum found: 39
```

### Fixed Size Template

```java
// Sliding Window - Fixed Size k
public int maxSumSubarrayOfSizeK(int[] arr, int k) {
    if (arr == null || arr.length < k) return -1; // Or throw exception

    int windowSum = 0;
    // Calculate sum of first window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    int maxSum = windowSum;

    // Slide the window: remove element going out, add element coming in
    for (int i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }

    return maxSum;
}
```

---

## 3.2 Variable Size Sliding Window

### Concept

Maintain a window that can grow and shrink based on certain conditions. Typically used for finding the smallest/largest subarray/substring that satisfies a constraint (e.g., longest substring without repeating characters, smallest subarray with sum >= target).

### Visual Trace

```
Array: [2, 1, 2, 4, 3, 1], Target Sum = 7
Goal: Find smallest subarray with sum >= 7

Start with window [2] (sum=2, < 7) -> Expand right -> [2, 1] (sum=3) -> ...
Eventually reach [2, 1, 2, 4] (sum=9 >= 7) -> Record size 4.
Now try to shrink from left: [1, 2, 4] (sum=7 >= 7) -> Record size 3.
Shrink again: [2, 4] (sum=6 < 7) -> Stop shrinking, expand right again.
Next valid window might be [4, 3] (sum=7, size=2).
Keep expanding and shrinking, tracking the minimum valid size.
```

### Variable Size Template

```java
// Sliding Window - Variable Size (Find Minimum Window)
// Example: Smallest subarray with sum >= target
public int minSubArrayLen(int target, int[] nums) {
    int minLength = Integer.MAX_VALUE;
    int windowSum = 0;
    int left = 0;

    for (int right = 0; right < nums.length; right++) {
        windowSum += nums[right]; // Expand window by moving right

        // Shrink window from left while condition is satisfied
        while (windowSum >= target) {
            minLength = Math.min(minLength, right - left + 1);
            windowSum -= nums[left]; // Remove element from left
            left++; // Shrink window
        }
    }

    return minLength == Integer.MAX_VALUE ? 0 : minLength;
}
```

### Longest Substring Without Repeating Characters (Variable Size)

```java
// Longest Substring Without Repeating Characters
public int lengthOfLongestSubstring(String s) {
    Set<Character> windowChars = new HashSet<>();
    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {
        char rightChar = s.charAt(right);

        // Shrink window until no duplicate
        while (windowChars.contains(rightChar)) {
            windowChars.remove(s.charAt(left));
            left++;
        }

        windowChars.add(rightChar);
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

---

## 3.3 Sliding Window Applications (Anagrams, Substrings)

### Find All Anagrams in a String

**Problem:** Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`.

**Concept:** Use a fixed-size sliding window of length `p.length()`. Maintain a character frequency map for the window and compare it with the frequency map of `p`.

```java
import java.util.*;

public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();
    if (s.length() < p.length()) return result;

    Map<Character, Integer> pMap = new HashMap<>();
    Map<Character, Integer> windowMap = new HashMap<>();

    // Build frequency map for p
    for (char c : p.toCharArray()) {
        pMap.put(c, pMap.getOrDefault(c, 0) + 1);
    }

    int k = p.length();
    // Initialize window
    for (int i = 0; i < k; i++) {
        char c = s.charAt(i);
        windowMap.put(c, windowMap.getOrDefault(c, 0) + 1);
    }

    if (windowMap.equals(pMap)) {
        result.add(0);
    }

    // Slide window
    for (int right = k; right < s.length(); right++) {
        int left = right - k + 1;
        char leftChar = s.charAt(left - 1);
        char rightChar = s.charAt(right);

        // Add new character
        windowMap.put(rightChar, windowMap.getOrDefault(rightChar, 0) + 1);
        // Remove old character
        windowMap.put(leftChar, windowMap.getOrDefault(leftChar, 0) - 1);
        if (windowMap.get(leftChar) == 0) {
            windowMap.remove(leftChar);
        }

        if (windowMap.equals(pMap)) {
            result.add(left);
        }
    }

    return result;
}
```

### Minimum Window Substring (Hard)

```java
// Minimum Window Substring - Variable Size, Needs All Chars
public String minWindow(String s, String t) {
    if (s.isEmpty() || t.isEmpty() || s.length() < t.length()) return "";

    Map<Character, Integer> tMap = new HashMap<>();
    for (char c : t.toCharArray()) {
        tMap.put(c, tMap.getOrDefault(c, 0) + 1);
    }

    Map<Character, Integer> windowMap = new HashMap<>();
    int left = 0, right = 0;
    int formed = 0; // Number of unique characters in window with desired frequency
    int required = tMap.size();

    int minLen = Integer.MAX_VALUE;
    int minLeft = 0;

    while (right < s.length()) {
        char rightChar = s.charAt(right);
        windowMap.put(rightChar, windowMap.getOrDefault(rightChar, 0) + 1);

        if (tMap.containsKey(rightChar) &&
            windowMap.get(rightChar).intValue() == tMap.get(rightChar).intValue()) {
            formed++;
        }

        while (left <= right && formed == required) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minLeft = left;
            }

            char leftChar = s.charAt(left);
            windowMap.put(leftChar, windowMap.get(leftChar) - 1);
            if (tMap.containsKey(leftChar) &&
                windowMap.get(leftChar).intValue() < tMap.get(leftChar).intValue()) {
                formed--;
            }
            left++;
        }
        right++;
    }

    return minLen == Integer.MAX_VALUE ? "" : s.substring(minLeft, minLeft + minLen);
}
```

### LeetCode Practice — Phase 3

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 209 | Minimum Size Subarray Sum | Variable Size (Sum Condition) | Medium |
| 3 | Longest Substring Without Repeating Characters | Variable Size (Unique Chars) | Medium |
| 76 | Minimum Window Substring | Variable Size (Contains All) | Hard |
| 438 | Find All Anagrams in a String | Fixed Size (Frequency Match) | Medium |
| 567 | Permutation in String | Fixed Size (Frequency Match) | Medium |
| 424 | Longest Repeating Character Replacement | Variable Size (With K Changes) | Medium |
| 1004 | Max Consecutive Ones III | Variable Size (With K Flips) | Medium |

---

# 🟠 Phase 4 — Modified Binary Search

---

## 4.1 Binary Search on Sorted Array

### Concept

Classic binary search finds a target value in a sorted array in O(log n) time. It works by repeatedly comparing the target with the middle element and eliminating half of the remaining search space.

### Visual Trace

```
Array: [1, 3, 5, 7, 9, 11, 13, 15], Target = 7
Indices: 0  1  2  3  4   5   6   7

L=0, R=7 -> Mid=(0+7)/2=3, arr[3]=7 -> Found! Return 3.

If Target was 6:
L=0, R=7 -> Mid=3, arr[3]=7 (6<7) -> Search Left -> R=Mid-1=2
L=0, R=2 -> Mid=1, arr[1]=3 (6>3) -> Search Right -> L=Mid+1=2
L=2, R=2 -> Mid=2, arr[2]=5 (6>5) -> Search Right -> L=Mid+1=3
L=3, R=2 -> L>R -> Not Found, Return -1
```

### Classic Binary Search Template

```java
// Standard Binary Search
public int binarySearch(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;

    while (left <= right) { // <= to handle single element
        int mid = left + (right - left) / 2; // Prevents overflow compared to (left+right)/2

        if (arr[mid] == target) {
            return mid; // Found
        } else if (arr[mid] < target) {
            left = mid + 1; // Search right half
        } else { // arr[mid] > target
            right = mid - 1; // Search left half
        }
    }

    return -1; // Not found
}
```

---

## 4.2 Search in Rotated Sorted Array

### Concept

An array is sorted but then rotated at some pivot point (e.g., `[4, 5, 6, 7, 0, 1, 2]`). Find the target efficiently. The key is that at least one half of the array from `mid` is always sorted.

### Visual Trace

```
Array: [4, 5, 6, 7, 0, 1, 2], Target = 0
L=0, R=6 -> Mid=3, arr[Mid]=7

Check if left half [L=0, Mid=3] is sorted: arr[0]=4 <= arr[3]=7 -> Yes.
Is target 0 in [4, 7]? No (0 < 4). So search right half [Mid+1=4, R=6].

L=4, R=6 -> Mid=5, arr[Mid]=1
Check if left half [L=4, Mid=5] is sorted: arr[4]=0 <= arr[5]=1 -> Yes.
Is target 0 in [0, 1]? Yes (0 >= 0 and 0 <= 1). Search left half [L=4, Mid=5].

L=4, R=5 -> Mid=4, arr[Mid]=0 -> Found!
```

### Rotated Array Search Template

```java
// Search in Rotated Sorted Array
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] == target) {
            return mid;
        }

        // Check if left half is sorted
        if (nums[left] <= nums[mid]) {
            // Left half is sorted
            if (nums[left] <= target && target < nums[mid]) {
                // Target is in the sorted left half
                right = mid - 1;
            } else {
                // Target is in the right half
                left = mid + 1;
            }
        } else {
            // Right half must be sorted (since array was originally sorted)
            if (nums[mid] < target && target <= nums[right]) {
                // Target is in the sorted right half
                left = mid + 1;
            } else {
                // Target is in the left half
                right = mid - 1;
            }
        }
    }

    return -1; // Not found
}
```

---

## 4.3 Find Peak Element

### Concept

A peak element is an element that is strictly greater than its neighbors. Find any peak element efficiently. Since the problem guarantees a peak exists (by assuming `nums[-1] = nums[n] = -∞`), we can use binary search by comparing `mid` with `mid+1`.

### Visual Trace

```
Array: [1, 2, 3, 1], Peak = 3 at index 2
L=0, R=3 -> Mid=1, arr[1]=2, arr[2]=3 -> arr[1] < arr[2]
Since arr[1] < arr[2], a peak must exist on the right side (increasing trend).
L=Mid+1=2, R=3 -> Mid=2, arr[2]=3, arr[3]=1 -> arr[2] > arr[3]
Since arr[2] > arr[3], a peak might be at 2 or on the left side.
R=Mid=2 -> L=2, R=2 -> Mid=2 -> Found peak candidate.
Check neighbors: arr[1]=2 < arr[2]=3 and arr[3]=1 < arr[3] -> Peak confirmed at index 2.
```

### Peak Element Template

```java
// Find Peak Element
public int findPeakElement(int[] nums) {
    int left = 0;
    int right = nums.length - 1;

    while (left < right) { // Loop until left == right
        int mid = left + (right - left) / 2;

        // Compare mid with its right neighbor
        if (nums[mid] < nums[mid + 1]) {
            // The peak is definitely on the right side
            left = mid + 1;
        } else {
            // The peak is mid or on the left side
            right = mid; // Keep mid as a candidate
        }
    }

    // When loop ends, left == right, which is the peak index
    return left;
}
```

---

## 4.4 Lower/Upper Bound

### Concept

- **Lower Bound:** The first element that is greater than or equal to the target.
- **Upper Bound:** The first element that is strictly greater than the target.
These are useful for finding ranges or inserting elements while maintaining order.

### Lower Bound Template

```java
// Lower Bound: Index of first element >= target
public int lowerBound(int[] arr, int target) {
    int left = 0;
    int right = arr.length; // Note: right is exclusive

    while (left < right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] < target) {
            left = mid + 1; // Current mid is too small, search right
        } else { // arr[mid] >= target
            right = mid; // Current mid might be the answer, keep it in search space
        }
    }

    return left; // Index of lower bound, or arr.length if target is larger than all elements
}
```

### Upper Bound Template

```java
// Upper Bound: Index of first element > target
public int upperBound(int[] arr, int target) {
    int left = 0;
    int right = arr.length; // Note: right is exclusive

    while (left < right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] <= target) {
            left = mid + 1; // Current mid is <= target, search right
        } else { // arr[mid] > target
            right = mid; // Current mid might be the answer, keep it in search space
        }
    }

    return left; // Index of upper bound, or arr.length if target is >= all elements
}
```

### Using Bounds: Find First/Last Occurrence

```java
// Find First and Last Position of Element in Sorted Array
public int[] searchRange(int[] nums, int target) {
    int first = lowerBound(nums, target);
    int last = upperBound(nums, target) - 1; // upper_bound gives index *after* last occurrence

    if (first < nums.length && nums[first] == target) {
        return new int[]{first, last};
    } else {
        return new int[]{-1, -1};
    }
}

// Lower Bound helper (as defined above)
private int lowerBound(int[] arr, int target) {
    int left = 0;
    int right = arr.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] < target) left = mid + 1;
        else right = mid;
    }
    return left;
}

// Upper Bound helper (as defined above)
private int upperBound(int[] arr, int target) {
    int left = 0;
    int right = arr.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] <= target) left = mid + 1;
        else right = mid;
    }
    return left;
}
```

### LeetCode Practice — Phase 4

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 704 | Binary Search | Classic Binary Search | Easy |
| 33 | Search in Rotated Sorted Array | Modified Binary Search | Medium |
| 153 | Find Minimum in Rotated Sorted Array | Modified Binary Search | Medium |
| 162 | Find Peak Element | Modified Binary Search | Medium |
| 34 | Find First and Last Position of Element in Sorted Array | Lower/Upper Bound | Medium |
| 35 | Search Insert Position | Lower Bound | Easy |
| 74 | Search a 2D Matrix | Binary Search on 2D (Flattened) | Medium |
| 240 | Search a 2D Matrix II | Different Pattern (Not Binary Search) | Medium |

---

# 🔴 Phase 5 — Prefix Sums & Advanced Patterns

---

## 5.1 Prefix Sum

### Concept

A prefix sum array `prefix[i]` stores the sum of all elements from the start of the original array up to index `i-1`. This allows for O(1) calculation of the sum of any subarray `arr[i...j]` using `prefix[j+1] - prefix[i]`. This is a powerful optimization for problems involving repeated range sum queries.

### Visual Trace

```
Original Array:  [2, 1, -3, 4, 1, -2, 1, 5, -3]
Indices:          0  1   2  3  4   5  6  7   8

Prefix Sum Array (size n+1):
prefix[0] = 0
prefix[1] = 0 + 2 = 2
prefix[2] = 2 + 1 = 3
prefix[3] = 3 + (-3) = 0
prefix[4] = 0 + 4 = 4
prefix[5] = 4 + 1 = 5
prefix[6] = 5 + (-2) = 3
prefix[7] = 3 + 1 = 4
prefix[8] = 4 + 5 = 9
prefix[9] = 9 + (-3) = 6

Prefix Array: [0, 2, 3, 0, 4, 5, 3, 4, 9, 6]

Sum of subarray [2, 4] (original indices) = arr[2] + arr[3] + arr[4] = -3 + 4 + 1 = 2
Calculated using prefix: prefix[5] - prefix[2] = 5 - 3 = 2. ✅
```

### Prefix Sum Template

```java
// Build Prefix Sum Array
public int[] buildPrefixSum(int[] arr) {
    int n = arr.length;
    int[] prefix = new int[n + 1]; // Size n+1 to handle edge case easily

    for (int i = 0; i < n; i++) {
        prefix[i + 1] = prefix[i] + arr[i];
    }

    return prefix;
}

// Query Range Sum using Prefix Array
public int queryRangeSum(int[] prefix, int left, int right) {
    // Sum of arr[left..right] = prefix[right+1] - prefix[left]
    return prefix[right + 1] - prefix[left];
}

// Example Usage
public void example() {
    int[] nums = {2, 1, -3, 4, 1, -2, 1, 5, -3};
    int[] prefix = buildPrefixSum(nums);

    int sum1to3 = queryRangeSum(prefix, 1, 3); // Sum of nums[1..3] = 1 + (-3) + 4 = 2
    int sum0to8 = queryRangeSum(prefix, 0, 8); // Sum of nums[0..8] = entire array sum
    System.out.println(sum1to3); // Output: 2
    System.out.println(sum0to8); // Output: 6
}
```

### Subarray Sum Equals K (Using HashMap with Prefix Sum)

```java
// Find number of subarrays that sum to K
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixCount = new HashMap<>();
    prefixCount.put(0, 1); // Handle subarrays starting from index 0

    int currentSum = 0;
    int count = 0;

    for (int num : nums) {
        currentSum += num;

        // Check if (currentSum - k) exists in map
        // If yes, it means there are subarrays ending at current index with sum k
        int complement = currentSum - k;
        count += prefixCount.getOrDefault(complement, 0);

        // Add current prefix sum to map
        prefixCount.put(currentSum, prefixCount.getOrDefault(currentSum, 0) + 1);
    }

    return count;
}
```

---

## 5.2 Difference Array (Advanced)

### Concept

The difference array `diff` is used for efficiently applying range updates (adding a value to all elements in a subarray `arr[i...j]`) and then querying the final state of the original array. It's the inverse of the prefix sum concept. `diff[i] = arr[i] - arr[i-1]`. Applying an update `val` to range `[i, j]` involves `diff[i] += val` and `diff[j+1] -= val`. Finally, `arr` is reconstructed using `arr[i] = diff[0] + diff[1] + ... + diff[i]` (which is a prefix sum of `diff`).

### Difference Array Template

```java
// Apply range updates efficiently using difference array
public int[] applyRangeUpdates(int n, int[][] updates) {
    int[] diff = new int[n + 1]; // Extra space to handle j+1 safely

    for (int[] update : updates) {
        int i = update[0]; // Start index of range
        int j = update[1]; // End index of range
        int val = update[2]; // Value to add

        diff[i] += val;
        if (j + 1 < n) { // Only if j+1 is within bounds
            diff[j + 1] -= val;
        }
    }

    // Reconstruct the final array from the difference array
    int[] result = new int[n];
    result[0] = diff[0];
    for (int i = 1; i < n; i++) {
        result[i] = result[i - 1] + diff[i];
    }

    return result;
}

// Example Usage
public void example() {
    int n = 5;
    int[][] updates = {{1, 3, 2}, {2, 4, 3}, {0, 2, -1}};
    // Initial array: [0, 0, 0, 0, 0]
    // Update [1,3] by +2: [0, 2, 2, 2, 0]
    // Update [2,4] by +3: [0, 2, 5, 5, 3]
    // Update [0,2] by -1: [-1, 1, 4, 5, 3]
    int[] result = applyRangeUpdates(n, updates);
    System.out.println(Arrays.toString(result)); // [-1, 1, 4, 5, 3]
}
```

---

## 5.3 KMP for Strings (Advanced)

### Concept

The Knuth-Morris-Pratt (KMP) algorithm finds occurrences of a "pattern" string within a "text" string in O(n + m) time, where n is the text length and m is the pattern length. It avoids unnecessary comparisons by preprocessing the pattern to create an LPS (Longest Proper Prefix which is also Suffix) array.

### LPS Array Example

```
Pattern: "ABABCABABA"
Indices:  0 1 2 3 4 5 6 7 8 9
LPS:      0 0 1 2 0 1 2 3 4 3

LPS[i] = Length of the longest proper prefix of pattern[0..i] that is also a suffix of pattern[0..i].
LPS[0] = 0 (single char has no proper prefix/suffix)
LPS[1] = 0 ("AB": prefix "A", suffix "B", no match)
LPS[2] = 1 ("ABA": prefix "A", suffix "A", match length 1)
LPS[3] = 2 ("ABAB": prefix "AB", suffix "AB", match length 2)
LPS[4] = 0 ("ABABC": no matching proper prefix/suffix)
... and so on.
```

### KMP Template

```java
// KMP Algorithm - Find all occurrences of pattern in text
public List<Integer> kmpSearch(String text, String pattern) {
    List<Integer> result = new ArrayList<>();
    if (pattern.isEmpty()) return result;

    int[] lps = computeLPS(pattern);
    int i = 0; // Index for text
    int j = 0; // Index for pattern

    while (i < text.length()) {
        if (text.charAt(i) == pattern.charAt(j)) {
            i++;
            j++;
        }

        if (j == pattern.length()) {
            // Found match at index (i - j)
            result.add(i - j);
            j = lps[j - 1]; // Look for next possible match
        } else if (i < text.length() && text.charAt(i) != pattern.charAt(j)) {
            if (j != 0) {
                j = lps[j - 1]; // Backtrack using LPS
            } else {
                i++; // Move to next character in text
            }
        }
    }

    return result;
}

// Compute LPS (Longest Proper Prefix which is also Suffix) array
private int[] computeLPS(String pattern) {
    int m = pattern.length();
    int[] lps = new int[m];
    int len = 0; // Length of previous longest prefix suffix
    int i = 1;

    while (i < m) {
        if (pattern.charAt(i) == pattern.charAt(len)) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1]; // Fallback in LPS array
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }

    return lps;
}
```

---

## 5.4 Bit Manipulation for Arrays/Strings (Advanced)

### Concept

Bit manipulation can be used for specific problems involving arrays or strings, often offering elegant and efficient solutions. Common techniques include using XOR properties (`a ^ a = 0`, `a ^ 0 = a`) and bit masking.

### Examples

#### Find Single Number (XOR)

```java
// Every element appears twice except one. Find the single one.
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num; // XOR all numbers
    }
    return result; // Paired numbers cancel out, leaving the single number
}
```

#### Count Set Bits (Hamming Weight)

```java
// Count the number of '1' bits in an integer
public int hammingWeight(int n) {
    int count = 0;
    while (n != 0) {
        count += n & 1; // Check if last bit is 1
        n >>>= 1; // Unsigned right shift (fill with 0)
    }
    return count;
}

// Alternative: Brian Kernighan's algorithm (faster)
public int hammingWeightBK(int n) {
    int count = 0;
    while (n != 0) {
        n &= (n - 1); // Removes the rightmost set bit
        count++;
    }
    return count;
}
```

#### Subsets Generation (Bit Masking)

```java
// Generate all possible subsets of an array using bit manipulation
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    int n = nums.length;
    int totalSubsets = 1 << n; // 2^n subsets

    for (int mask = 0; mask < totalSubsets; mask++) {
        List<Integer> subset = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            // Check if i-th bit is set in the mask
            if ((mask & (1 << i)) != 0) {
                subset.add(nums[i]);
            }
        }
        result.add(subset);
    }

    return result;
}
```

### LeetCode Practice — Phase 5

| # | Problem | Concept | Difficulty |
|---|---------|---------|------------|
| 303 | Range Sum Query - Immutable | Prefix Sum | Easy |
| 304 | Range Sum Query 2D - Immutable | 2D Prefix Sum | Medium |
| 560 | Subarray Sum Equals K | Prefix Sum + HashMap | Medium |
| 1109 | Corporate Flight Bookings | Difference Array | Medium |
| 209 | Minimum Size Subarray Sum | Sliding Window (can also use Prefix + Binary Search) | Medium |
| 28 | Find the Index of the First Occurrence in a String | KMP | Medium |
| 136 | Single Number | Bit Manipulation (XOR) | Easy |
| 191 | Number of 1 Bits | Bit Manipulation (Counting) | Easy |
| 78 | Subsets | Bit Manipulation (Masking) | Medium |

---

# 🧠 Master Decision Table

```
READ PROBLEM → MATCH SIGNAL → PICK ALGORITHM
──────────────────────────────────────────────────────────────
Signal                        Algorithm
──────────────────────────────────────────────────────────────
"Reverse array/string in-place" → Two Pointers (Opposite Ends)
"Pair in sorted array = target" → Two Pointers (Sorted Array)
"Cycle detection in array"      → Two Pointers (Fast & Slow)
"Container with most water"     → Two Pointers (Optimization)
"Max/Min subarray sum of size k" → Sliding Window (Fixed Size)
"Longest/Shortest subarray/substring with condition" → Sliding Window (Variable Size)
"Search in sorted rotated array" → Modified Binary Search (Rotated)
"Find peak element"             → Modified Binary Search (Peak)
"Find first/last occurrence"    → Modified Binary Search (Lower/Upper Bound)
"Range sum queries"             → Prefix Sum
"Range updates"                 → Difference Array
"Pattern matching in string"    → KMP (or built-in indexOf for simple cases)
"Find unique element among duplicates" → Bit Manipulation (XOR)
"Generate all subsets"          → Bit Manipulation (Masking)
```

---

# 📋 Java Syntax Master Reference — Arrays & Strings

```java
// ── ARRAY BASICS ──────────────────────────────────────────────
int[] arr = new int[n];
int len = arr.length; // No parentheses!
for (int i = 0; i < arr.length; i++) { }
for (int val : arr) { }

// ── STRING BASICS ─────────────────────────────────────────────
String s = "example";
int lenStr = s.length();
char c = s.charAt(i);
String sub = s.substring(start, end_exclusive);
boolean contains = s.contains(substring);
int indexOf = s.indexOf(char_or_string);

// ── STRINGBUILDER ─────────────────────────────────────────────
StringBuilder sb = new StringBuilder();
sb.append(value);
String result = sb.toString();

// ── CHARACTER CHECKS ──────────────────────────────────────────
char ch = 'a';
Character.isDigit(ch); Character.isLetter(ch);
Character.isUpperCase(ch); Character.toLowerCase(ch);

// ── SORTING ───────────────────────────────────────────────────
Arrays.sort(arr);
Arrays.sort(arr, Collections.reverseOrder()); // For Integer[], etc.

// ── PREFIX SUM ────────────────────────────────────────────────
int[] prefix = new int[arr.length + 1];
for (int i = 0; i < arr.length; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}
// Range sum [left, right]: prefix[right + 1] - prefix[left]

// ── HASHMAP/FOR SUBARRAY PROBLEMS ─────────────────────────────
Map<Integer, Integer> map = new HashMap<>();
map.put(key, map.getOrDefault(key, 0) + 1); // Increment count
map.containsKey(key);
map.get(key);

// ── SET FOR UNIQUENESS ────────────────────────────────────────
Set<Integer> set = new HashSet<>();
set.add(value);
set.contains(value);

// ── PRIORITYQUEUE (HEAP) ──────────────────────────────────────
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
minHeap.offer(value);
int top = minHeap.peek();
int extracted = minHeap.poll();
```

---

# 🗂️ Complete Problem List by Phase

## Phase 2 — Two Pointers

| # | Problem | Technique |
|---|---------|-----------|
| 344 | Reverse String | Basic |
| 125 | Valid Palindrome | Basic (skip non-alphanumeric) |
| 167 | Two Sum II - Input Array Is Sorted | Sorted Array |
| 15 | 3Sum | Three Pointers |
| 11 | Container With Most Water | Optimization |
| 287 | Find the Duplicate Number | Fast & Slow |

## Phase 3 — Sliding Window

| # | Problem | Technique |
|---|---------|-----------|
| 209 | Minimum Size Subarray Sum | Variable Size (Sum Condition) |
| 3 | Longest Substring Without Repeating Characters | Variable Size (Unique Chars) |
| 76 | Minimum Window Substring | Variable Size (Contains All) |
| 438 | Find All Anagrams in a String | Fixed Size (Frequency Match) |
| 424 | Longest Repeating Character Replacement | Variable Size (With K Changes) |

## Phase 4 — Modified Binary Search

| # | Problem | Technique |
|---|---------|-----------|
| 704 | Binary Search | Classic |
| 33 | Search in Rotated Sorted Array | Rotated Array |
| 153 | Find Minimum in Rotated Sorted Array | Rotated Array Min |
| 162 | Find Peak Element | Peak Finding |
| 34 | Find First and Last Position of Element | Lower/Upper Bound |

## Phase 5 — Prefix Sums & Advanced

| # | Problem | Technique |
|---|---------|-----------|
| 303 | Range Sum Query - Immutable | Prefix Sum |
| 560 | Subarray Sum Equals K | Prefix Sum + HashMap |
| 28 | Find the Index of the First Occurrence in a String | KMP |
| 136 | Single Number | Bit Manipulation (XOR) |
| 78 | Subsets | Bit Manipulation (Masking) |

---

*Updated: 2026-06-06 | Java | Arrays & Strings: Complete Beginner → SDE Ready*
*5 Phases: Foundations → Two Pointers → Sliding Window → Modified Binary Search → Prefix Sums & Advanced*