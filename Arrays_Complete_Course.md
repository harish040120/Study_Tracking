# 📦 Arrays — Complete Course
**Language:** Java | **Level:** SDE Interview Ready
**Based on:** BYTS Sheet Problems
**Style:** Concept → Visual → Why it works → Java Code → Problems

---

# 📌 Contents

**Phase 1 — Array Foundations**
- 1.1 What is an Array?
- 1.2 Time Complexity Reference
- 1.3 Building Blocks — Java Array Syntax

**Phase 2 — HashMap & Frequency Patterns**
- 2.1 HashMap for O(1) Lookup
- 2.2 Frequency Counting
- 2.3 Grouping / Bucketing
- 2.4 Consecutive Sequence

**Phase 3 — Two Pointer Patterns**
- 3.1 Opposite Ends (Sorted Array)
- 3.2 Same Direction (Fast / Slow)
- 3.3 Three Pointer (Dutch Flag)
- 3.4 Fixed Gap Pointer
- 3.5 3Sum / 4Sum Pattern

**Phase 4 — Sliding Window**
- 4.1 Fixed Size Window
- 4.2 Dynamic Window — Longest Valid
- 4.3 Dynamic Window — Shortest Valid
- 4.4 Counting Subarrays (atMost trick)
- 4.5 Deque Window (Sliding Maximum)

**Phase 5 — Prefix Sum & Subarray Sum**
- 5.1 Prefix Sum Array
- 5.2 Prefix Sum + HashMap (Subarray = K)
- 5.3 2D Prefix Sum

**Phase 6 — Sorting-Based Patterns**
- 6.1 Sort + Two Pointers
- 6.2 Sort + Greedy (Intervals, Scheduling)
- 6.3 Bucket Sort / Counting Sort
- 6.4 Custom Comparator Sort

**Phase 7 — Kadane's & Subarray Extremes**
- 7.1 Maximum Subarray (Kadane's)
- 7.2 Maximum Product Subarray
- 7.3 Circular Subarray Maximum

**Phase 8 — Cyclic Sort & In-Place**
- 8.1 Cyclic Sort (1 to N problems)
- 8.2 In-Place Negation Trick
- 8.3 In-Place Array Modification

**Phase 9 — Binary Search on Arrays**
- 9.1 Classic Binary Search
- 9.2 Left / Right Boundary Search
- 9.3 Rotated Sorted Array
- 9.4 Binary Search on Answer

**Phase 10 — Product & XOR Tricks**
- 10.1 Product Except Self
- 10.2 XOR Tricks (Missing, Single)

**Master Reference**
- Java Array Syntax Sheet
- Pattern Decision Table
- All BYTS Problems by Pattern

---

# 🟢 Phase 1 — Array Foundations

---

## 1.1 What is an Array?

An array stores elements at **contiguous memory locations**.
Each element accessed by **index** in O(1).

```
index:  0    1    2    3    4
value: [10,  20,  30,  40,  50]
        ↑                    ↑
     first                 last (index = length-1)
```

**Core properties:**
- Fixed size (Java arrays)
- Index-based O(1) access
- Search without sorting: O(N)
- Search with sorting: O(log N) binary search

---

## 1.2 Time Complexity Reference

```
Operation               Time
───────────────────────────────
Access by index         O(1)
Search (unsorted)       O(N)
Search (sorted)         O(log N)
Insert at end           O(1) amortized
Insert at middle        O(N) shift
Delete from middle      O(N) shift
Sort                    O(N log N)
```

---

## 1.3 Building Blocks — Java Array Syntax

```java
// ── DECLARE & INIT ────────────────────────────────────────────
int[] arr = new int[5];              // [0, 0, 0, 0, 0]
int[] arr = {1, 2, 3, 4, 5};
int[][] matrix = new int[3][4];      // 3 rows, 4 cols

// ── SIZE ──────────────────────────────────────────────────────
arr.length                           // NOT arr.length()
matrix.length                        // number of rows
matrix[0].length                     // number of cols

// ── FILL ──────────────────────────────────────────────────────
Arrays.fill(arr, 0);
Arrays.fill(arr, Integer.MAX_VALUE);
Arrays.fill(arr, -1);

// ── SORT ──────────────────────────────────────────────────────
Arrays.sort(arr);                    // ascending
Arrays.sort(arr, (a, b) -> b - a);  // descending (Integer[] only)
Arrays.sort(matrix, (a, b) -> a[0] - b[0]); // sort 2D by col 0

// ── COPY ──────────────────────────────────────────────────────
int[] copy = arr.clone();
int[] copy = Arrays.copyOf(arr, arr.length);
int[] sub  = Arrays.copyOfRange(arr, 2, 5); // [2, 5)

// ── SEARCH ────────────────────────────────────────────────────
int idx = Arrays.binarySearch(arr, target); // sorted array only

// ── COMPARE ───────────────────────────────────────────────────
Arrays.equals(arr1, arr2);

// ── STREAM OPERATIONS ─────────────────────────────────────────
int max = Arrays.stream(arr).max().getAsInt();
int min = Arrays.stream(arr).min().getAsInt();
int sum = Arrays.stream(arr).sum();
long count = Arrays.stream(arr).filter(x -> x > 0).count();

// ── SWAP ──────────────────────────────────────────────────────
int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;

// ── ITERATE ───────────────────────────────────────────────────
for (int i = 0; i < arr.length; i++) { }
for (int num : arr) { }  // enhanced for loop

// ── LIST ↔ ARRAY ──────────────────────────────────────────────
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3));
Integer[] boxed = list.toArray(new Integer[0]);
int[] primitive = list.stream().mapToInt(Integer::intValue).toArray();
```

---

# 🔵 Phase 2 — HashMap & Frequency Patterns

---

## 2.1 HashMap for O(1) Lookup

### Concept

Store things you've SEEN before so you can find them in O(1) instead of scanning O(N).

**Three uses:**
1. **Complement check** — store values, check if `target - current` is in map
2. **Index storage** — store index of seen value for range calculations
3. **Frequency count** — count occurrences

```
Without HashMap: check all pairs → O(N²)
With HashMap:    for each element, check if complement exists → O(N)
```

### Two Sum (LC 1)

```java
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    // map stores: {value → index}

    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];

        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i); // store AFTER checking
    }
    return new int[]{};
}
```

### Contains Duplicate (LC 217)

```java
boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (!seen.add(num)) return true; // add returns false if already exists
    }
    return false;
}
```

### Contains Duplicate II (LC 219)
Duplicate within distance k:

```java
boolean containsNearbyDuplicate(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>(); // {value → last index}
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(nums[i])
                && i - map.get(nums[i]) <= k) return true;
        map.put(nums[i], i);
    }
    return false;
}
```

---

## 2.2 Frequency Counting

### Concept

Count how many times each element appears.

```java
// METHOD 1: HashMap (for any values)
Map<Integer, Integer> freq = new HashMap<>();
for (int num : nums) {
    freq.merge(num, 1, Integer::sum);
    // OR: freq.put(num, freq.getOrDefault(num, 0) + 1);
}

// METHOD 2: int[] array (only for 0..n-1 or 'a'..'z')
int[] count = new int[26];
for (char c : s.toCharArray()) count[c - 'a']++;
```

### Valid Anagram (LC 242)

```java
boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] count = new int[26];
    for (char c : s.toCharArray()) count[c - 'a']++;
    for (char c : t.toCharArray()) count[c - 'a']--;
    for (int n : count) if (n != 0) return false;
    return true;
}
```

### Top K Frequent Elements (LC 347) — Bucket Sort

```java
int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int n : nums) freq.merge(n, 1, Integer::sum);

    // Bucket: index = frequency, value = list of nums
    List<Integer>[] bucket = new List[nums.length + 1];

    for (int key : freq.keySet()) {
        int f = freq.get(key);
        if (bucket[f] == null) bucket[f] = new ArrayList<>();
        bucket[f].add(key);
    }

    int[] result = new int[k];
    int idx = 0;
    // Walk buckets from HIGH frequency to LOW
    for (int i = bucket.length - 1; i >= 0 && idx < k; i--) {
        if (bucket[i] != null) {
            for (int num : bucket[i]) {
                result[idx++] = num;
                if (idx == k) break;
            }
        }
    }
    return result;
}
```

---

## 2.3 Grouping / Bucketing

### Group Anagrams (LC 49)

**Key insight:** sort each string → same sorted string = same anagram group.

```java
List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();

    for (String s : strs) {
        char[] arr = s.toCharArray();
        Arrays.sort(arr);
        String key = new String(arr); // sorted string as key

        map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
    }

    return new ArrayList<>(map.values());
}
```

---

## 2.4 Consecutive Sequence

### Longest Consecutive Sequence (LC 128)

**Key insight:** only START counting from sequence beginners (num - 1 not in set).

```java
int longestConsecutive(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int n : nums) set.add(n);

    int longest = 0;

    for (int n : set) {
        // Only start counting if n is sequence start
        if (!set.contains(n - 1)) {
            int len = 1;
            while (set.contains(n + len)) len++;
            longest = Math.max(longest, len);
        }
    }
    return longest;
}
```

### Intersection of Two Arrays (LC 349)

```java
int[] intersection(int[] nums1, int[] nums2) {
    Set<Integer> set = new HashSet<>();
    for (int n : nums1) set.add(n);

    Set<Integer> result = new HashSet<>();
    for (int n : nums2) if (set.contains(n)) result.add(n);

    return result.stream().mapToInt(Integer::intValue).toArray();
}
```

### Intersection of Two Arrays II (LC 350) — with duplicates

```java
int[] intersect(int[] nums1, int[] nums2) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int n : nums1) freq.merge(n, 1, Integer::sum);

    List<Integer> result = new ArrayList<>();
    for (int n : nums2) {
        if (freq.getOrDefault(n, 0) > 0) {
            result.add(n);
            freq.merge(n, -1, Integer::sum);
        }
    }
    return result.stream().mapToInt(Integer::intValue).toArray();
}
```

### BYTS Problems — Phase 2

| # | Problem | Pattern |
|---|---------|---------|
| 1 | Two Sum | Complement HashMap |
| 217 | Contains Duplicate | HashSet |
| 219 | Contains Duplicate II | HashMap + index |
| 242 | Valid Anagram | Frequency array |
| 49 | Group Anagrams | Sort key HashMap |
| 347 | Top K Frequent | Bucket sort |
| 128 | Longest Consecutive Sequence | HashSet |
| 349 | Intersection of Two Arrays | HashSet |
| 350 | Intersection Two Arrays II | Frequency count |
| 268 | Missing Number | HashSet / XOR |

---

# 🟡 Phase 3 — Two Pointer Patterns

---

## 3.1 Opposite Ends (Sorted Array)

### Concept

Start `i` at LEFT, `j` at RIGHT. Move toward each other based on condition.
Works when array is sorted or has "squeeze from both ends" logic.

```
i=0 →                ← j=n-1
     move based on sum vs target
```

### Two Sum II — Sorted (LC 167)

```java
int[] twoSum(int[] numbers, int target) {
    int i = 0, j = numbers.length - 1;

    while (i < j) {
        int sum = numbers[i] + numbers[j];
        if (sum == target) return new int[]{i+1, j+1};
        else if (sum < target) i++;  // need bigger
        else j--;                     // need smaller
    }
    return new int[]{};
}
```

### Container With Most Water (LC 11)

**Key insight:** always move the SHORTER wall inward (that's the bottleneck).

```java
int maxArea(int[] height) {
    int i = 0, j = height.length - 1;
    int maxWater = 0;

    while (i < j) {
        int water = Math.min(height[i], height[j]) * (j - i);
        maxWater = Math.max(maxWater, water);

        // Move shorter wall — it's the bottleneck
        if (height[i] < height[j]) i++;
        else j--;
    }
    return maxWater;
}
```

### Trapping Rain Water (LC 42)

**Key insight:** water at position i = min(maxLeft, maxRight) - height[i].
Track max from left and right as you go.

```java
int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int leftMax = 0, rightMax = 0;
    int water = 0;

    while (left < right) {
        if (height[left] < height[right]) {
            leftMax = Math.max(leftMax, height[left]);
            water += leftMax - height[left]; // water here
            left++;
        } else {
            rightMax = Math.max(rightMax, height[right]);
            water += rightMax - height[right];
            right--;
        }
    }
    return water;
}
```

### Boats to Save People (LC 881) — Greedy Two Pointer

```java
int numRescueBoats(int[] people, int limit) {
    Arrays.sort(people);
    int i = 0, j = people.length - 1;
    int boats = 0;

    while (i <= j) {
        if (people[i] + people[j] <= limit) i++; // pair lightest with heaviest
        j--;      // heaviest always takes one boat
        boats++;
    }
    return boats;
}
```

---

## 3.2 Same Direction (Fast / Slow Write Pointer)

### Concept

`slow` marks where to WRITE next valid element.
`fast` scans through the array.

```
slow → write position (next valid slot)
fast → current scanning position
```

### Remove Duplicates — Sorted (LC 26)

```java
int removeDuplicates(int[] nums) {
    int slow = 1; // position to write next unique
    for (int fast = 1; fast < nums.length; fast++) {
        if (nums[fast] != nums[fast - 1]) {
            nums[slow++] = nums[fast];
        }
    }
    return slow;
}
```

### Remove Duplicates II — At Most 2 (LC 80)

```java
int removeDuplicates(int[] nums) {
    int slow = 2; // first 2 are always valid
    for (int fast = 2; fast < nums.length; fast++) {
        // valid if current != element 2 positions behind write pointer
        if (nums[fast] != nums[slow - 2]) {
            nums[slow++] = nums[fast];
        }
    }
    return slow;
}
```

### Remove Element (LC 27)

```java
int removeElement(int[] nums, int val) {
    int slow = 0;
    for (int fast = 0; fast < nums.length; fast++) {
        if (nums[fast] != val) nums[slow++] = nums[fast];
    }
    return slow;
}
```

### Move Zeroes (LC 283)

```java
void moveZeroes(int[] nums) {
    int slow = 0; // next position for non-zero
    for (int fast = 0; fast < nums.length; fast++) {
        if (nums[fast] != 0) nums[slow++] = nums[fast];
    }
    while (slow < nums.length) nums[slow++] = 0;
}
```

### Squares of Sorted Array (LC 977)

**Key insight:** largest squares come from BOTH ENDS (negatives² can be large too).

```java
int[] sortedSquares(int[] nums) {
    int[] result = new int[nums.length];
    int i = 0, j = nums.length - 1;
    int pos = nums.length - 1; // fill result from RIGHT

    while (i <= j) {
        int left  = nums[i] * nums[i];
        int right = nums[j] * nums[j];
        if (left > right) { result[pos--] = left;  i++; }
        else              { result[pos--] = right; j--; }
    }
    return result;
}
```

---

## 3.3 Three Pointer — Dutch National Flag (LC 75)

### Concept

Sort array with exactly 3 distinct values (0, 1, 2) in ONE pass.

Three pointers: `low` (boundary of 0s), `mid` (current), `high` (boundary of 2s).

```
[0...low-1] = all 0s
[low...mid-1] = all 1s
[high+1...n-1] = all 2s
mid...high = unsorted
```

```java
void sortColors(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;

    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums, low++, mid++); // 0 goes left
        } else if (nums[mid] == 1) {
            mid++;                    // 1 stays in middle
        } else {
            swap(nums, mid, high--);  // 2 goes right
            // DON'T increment mid — new value unexamined
        }
    }
}

void swap(int[] a, int i, int j) {
    int t = a[i]; a[i] = a[j]; a[j] = t;
}
```

---

## 3.4 Fixed Gap Pointer

Both pointers move same direction but maintain a fixed distance k.

**Use case:** any problem where you compare elements k apart, or need an element k positions back.

```java
// Is Subsequence (LC 392)
boolean isSubsequence(String s, String t) {
    int i = 0; // pointer into s
    for (int j = 0; j < t.length() && i < s.length(); j++) {
        if (s.charAt(i) == t.charAt(j)) i++;
    }
    return i == s.length();
}
```

---

## 3.5 3Sum / 4Sum Pattern

### 3Sum (LC 15)

Fix one element. Two-pointer on the rest.
Must SORT first. Skip duplicates carefully.

```java
List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();

    for (int k = 0; k < nums.length - 2; k++) {
        if (k > 0 && nums[k] == nums[k-1]) continue; // skip outer dup

        int i = k + 1, j = nums.length - 1;
        while (i < j) {
            int sum = nums[k] + nums[i] + nums[j];
            if (sum == 0) {
                result.add(Arrays.asList(nums[k], nums[i], nums[j]));
                while (i < j && nums[i] == nums[i+1]) i++; // skip inner dup
                while (i < j && nums[j] == nums[j-1]) j--;
                i++; j--;
            } else if (sum < 0) i++;
            else j--;
        }
    }
    return result;
}
```

### 3Sum Closest (LC 16)

```java
int threeSumClosest(int[] nums, int target) {
    Arrays.sort(nums);
    int closest = nums[0] + nums[1] + nums[2];

    for (int k = 0; k < nums.length - 2; k++) {
        int i = k + 1, j = nums.length - 1;
        while (i < j) {
            int sum = nums[k] + nums[i] + nums[j];
            if (Math.abs(sum - target) < Math.abs(closest - target))
                closest = sum;
            if (sum < target) i++;
            else if (sum > target) j--;
            else return sum; // exact match
        }
    }
    return closest;
}
```

### BYTS Problems — Phase 3

| # | Problem | Pattern |
|---|---------|---------|
| 167 | Two Sum II | Opposite ends |
| 11 | Container With Most Water | Opposite ends |
| 42 | Trapping Rain Water | Opposite ends |
| 881 | Boats to Save People | Greedy two pointer |
| 15 | 3Sum | Sort + two pointer |
| 16 | 3Sum Closest | Sort + two pointer |
| 18 | 4Sum | Sort + two pointer |
| 259 | 3Sum Smaller | Sort + count |
| 26 | Remove Duplicates | Fast/slow |
| 80 | Remove Duplicates II | Fast/slow |
| 27 | Remove Element | Fast/slow |
| 283 | Move Zeroes | Fast/slow |
| 977 | Squares of Sorted Array | Fill from ends |
| 75 | Sort Colors | Dutch flag |
| 905 | Sort Array By Parity | Two pointer |
| 392 | Is Subsequence | Fixed gap |
| 2337 | Move Pieces to Obtain String | Two pointer |
| 2938 | Separate Black and White Balls | Two pointer |

---

# 🟠 Phase 4 — Sliding Window

---

## 4.1 Fixed Size Window

### Concept

Window is ALWAYS exactly k elements. Build first window, then slide:
add right element, remove left element.

```
[1, 3, 2, 6, 4, 8]  k=3
[1,3,2] → sum=6
  [3,2,6] → sum=11 (add 6, remove 1)
    [2,6,4] → sum=12 (add 4, remove 3)
      [6,4,8] → sum=18 (add 8, remove 2)
```

### Maximum Average Subarray I (LC 643)

```java
double findMaxAverage(int[] nums, int k) {
    double windowSum = 0;
    for (int i = 0; i < k; i++) windowSum += nums[i]; // first window

    double maxSum = windowSum;
    for (int j = k; j < nums.length; j++) {
        windowSum += nums[j];        // add new right
        windowSum -= nums[j - k];   // remove old left
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum / k;
}
```

### Maximum Sum of Distinct Subarrays Length K (LC 2461)

```java
long maximumSubarraySum(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    long windowSum = 0, maxSum = 0;

    for (int j = 0; j < nums.length; j++) {
        windowSum += nums[j];
        freq.merge(nums[j], 1, Integer::sum);

        if (j >= k) {
            int left = nums[j - k];
            windowSum -= left;
            freq.merge(left, -1, Integer::sum);
            if (freq.get(left) == 0) freq.remove(left);
        }

        if (j >= k - 1 && freq.size() == k) {
            maxSum = Math.max(maxSum, windowSum);
        }
    }
    return maxSum;
}
```

---

## 4.2 Dynamic Window — Longest Valid

### Concept

Window GROWS when condition holds, SHRINKS when it's violated.
Move `j` right to expand, move `i` right to shrink.

**Template — Longest:**
```
for j = 0 to n-1:
    add nums[j] to window
    while condition VIOLATED:
        remove nums[i], i++
    update maxLen = j - i + 1
```

### Longest Substring Without Repeating (LC 3)

```java
int lengthOfLongestSubstring(String s) {
    Set<Character> window = new HashSet<>();
    int i = 0, maxLen = 0;

    for (int j = 0; j < s.length(); j++) {
        while (window.contains(s.charAt(j))) {
            window.remove(s.charAt(i++)); // shrink
        }
        window.add(s.charAt(j));
        maxLen = Math.max(maxLen, j - i + 1);
    }
    return maxLen;
}
```

### Longest Repeating Character Replacement (LC 424)

**Key insight:** window is valid if `(window size - max frequency) ≤ k`.
We can replace at most k characters to make all the same.

```java
int characterReplacement(String s, int k) {
    int[] count = new int[26];
    int i = 0, maxFreq = 0, maxLen = 0;

    for (int j = 0; j < s.length(); j++) {
        maxFreq = Math.max(maxFreq, ++count[s.charAt(j) - 'A']);

        // window invalid when replacements needed > k
        while ((j - i + 1) - maxFreq > k) {
            count[s.charAt(i++) - 'A']--;
            // Note: maxFreq may be stale but that's OK —
            // it only shrinks valid windows
        }
        maxLen = Math.max(maxLen, j - i + 1);
    }
    return maxLen;
}
```

### Max Consecutive Ones III (LC 1004)

Flip at most k zeros. Find longest subarray with at most k zeros.

```java
int longestOnes(int[] nums, int k) {
    int i = 0, zeros = 0, maxLen = 0;

    for (int j = 0; j < nums.length; j++) {
        if (nums[j] == 0) zeros++;

        while (zeros > k) {
            if (nums[i++] == 0) zeros--;
        }
        maxLen = Math.max(maxLen, j - i + 1);
    }
    return maxLen;
}
```

### Longest Subarray of 1s After Deleting One (LC 1493)

```java
int longestSubarray(int[] nums) {
    int i = 0, zeros = 0, maxLen = 0;

    for (int j = 0; j < nums.length; j++) {
        if (nums[j] == 0) zeros++;
        while (zeros > 1) {
            if (nums[i++] == 0) zeros--;
        }
        maxLen = Math.max(maxLen, j - i); // -1 for deleted element
    }
    return maxLen;
}
```

---

## 4.3 Dynamic Window — Shortest Valid

### Concept

Shrink window AS SOON AS condition is satisfied. Record minimum.

**Template — Shortest:**
```
for j = 0 to n-1:
    add nums[j] to window
    while condition SATISFIED:
        record minLen = j - i + 1
        remove nums[i], i++   (shrink to try smaller)
```

### Minimum Size Subarray Sum (LC 209)

```java
int minSubArrayLen(int target, int[] nums) {
    int i = 0, sum = 0, minLen = Integer.MAX_VALUE;

    for (int j = 0; j < nums.length; j++) {
        sum += nums[j];

        while (sum >= target) {
            minLen = Math.min(minLen, j - i + 1);
            sum -= nums[i++]; // shrink
        }
    }
    return minLen == Integer.MAX_VALUE ? 0 : minLen;
}
```

### Minimum Window Substring (LC 76)

```java
String minWindow(String s, String t) {
    Map<Character, Integer> need = new HashMap<>();
    for (char c : t.toCharArray()) need.merge(c, 1, Integer::sum);

    int have = 0, total = need.size();
    int i = 0, minLen = Integer.MAX_VALUE, resL = 0;
    Map<Character, Integer> window = new HashMap<>();

    for (int j = 0; j < s.length(); j++) {
        char c = s.charAt(j);
        window.merge(c, 1, Integer::sum);
        if (need.containsKey(c) && window.get(c).equals(need.get(c)))
            have++;

        while (have == total) { // window satisfies condition — shrink
            if (j - i + 1 < minLen) { minLen = j - i + 1; resL = i; }
            char left = s.charAt(i);
            window.merge(left, -1, Integer::sum);
            if (need.containsKey(left) && window.get(left) < need.get(left))
                have--;
            i++;
        }
    }
    return minLen == Integer.MAX_VALUE ? "" : s.substring(resL, resL + minLen);
}
```

---

## 4.4 Counting Subarrays — atMost Trick

### Concept

**Exactly k = atMost(k) - atMost(k-1)**

Many "count subarrays with EXACTLY k condition" can be solved by:
counting subarrays with AT MOST k, minus AT MOST k-1.

```java
// SUBARRAY PRODUCT LESS THAN K (LC 713)
int numSubarrayProductLessThanK(int[] nums, int k) {
    if (k <= 1) return 0;
    int prod = 1, i = 0, count = 0;

    for (int j = 0; j < nums.length; j++) {
        prod *= nums[j];
        while (prod >= k) prod /= nums[i++];
        count += j - i + 1; // all subarrays ending at j with prod < k
    }
    return count;
}

// FRUIT INTO BASKETS (LC 904) — atMost 2 distinct types
int totalFruit(int[] fruits) {
    Map<Integer, Integer> basket = new HashMap<>();
    int i = 0, maxLen = 0;

    for (int j = 0; j < fruits.length; j++) {
        basket.merge(fruits[j], 1, Integer::sum);

        while (basket.size() > 2) {
            basket.merge(fruits[i], -1, Integer::sum);
            if (basket.get(fruits[i]) == 0) basket.remove(fruits[i]);
            i++;
        }
        maxLen = Math.max(maxLen, j - i + 1);
    }
    return maxLen;
}
```

---

## 4.5 Deque Window — Sliding Maximum

### Concept

Keep a deque (double-ended queue) of indices in DECREASING order of values.
Front of deque = index of current window maximum.

```
nums = [1,3,-1,-3,5,3,6,7], k=3
Window [1,3,-1]: deque=[1(3),-1] → max=3
Window [3,-1,-3]: deque=[1(3),-1,-3] → max=3
Window [-1,-3,5]: deque=[5] → max=5
```

### Sliding Window Maximum (LC 239)

```java
int[] maxSlidingWindow(int[] nums, int k) {
    int[] result = new int[nums.length - k + 1];
    Deque<Integer> dq = new ArrayDeque<>(); // stores indices

    for (int j = 0; j < nums.length; j++) {
        // Remove indices outside window
        while (!dq.isEmpty() && dq.peekFirst() < j - k + 1)
            dq.pollFirst();

        // Remove indices with smaller values (useless)
        while (!dq.isEmpty() && nums[dq.peekLast()] < nums[j])
            dq.pollLast();

        dq.offerLast(j);

        if (j >= k - 1) result[j - k + 1] = nums[dq.peekFirst()];
    }
    return result;
}
```

### BYTS Problems — Phase 4

| # | Problem | Pattern |
|---|---------|---------|
| 643 | Maximum Average Subarray I | Fixed window |
| 2461 | Max Sum Distinct Subarrays | Fixed window |
| 3 | Longest Substring No Repeat | Dynamic longest |
| 424 | Longest Repeating Char Replacement | Dynamic longest |
| 1004 | Max Consecutive Ones III | Dynamic longest |
| 1493 | Longest Subarray After Deleting | Dynamic longest |
| 1438 | Longest Subarray Abs Diff ≤ Limit | Dynamic longest |
| 1838 | Frequency of Most Frequent Element | Dynamic longest |
| 2779 | Maximum Beauty | Dynamic longest |
| 209 | Minimum Size Subarray Sum | Dynamic shortest |
| 76 | Minimum Window Substring | Dynamic shortest |
| 713 | Subarray Product Less Than K | atMost count |
| 904 | Fruit Into Baskets | atMost 2 distinct |
| 1658 | Min Operations to Reduce X to Zero | Dynamic window |
| 2516 | Take K of Each Character | Dynamic window |
| 239 | Sliding Window Maximum | Deque |
| 862 | Shortest Subarray Sum ≥ K | Deque |
| 438 | Find All Anagrams in String | Fixed + freq |
| 567 | Permutation in String | Fixed + freq |

---

# 🔴 Phase 5 — Prefix Sum & Subarray Sum

---

## 5.1 Prefix Sum Array

### Concept

`prefix[i]` = sum of all elements from index 0 to i.
Any subarray sum `[i, j]` = `prefix[j] - prefix[i-1]` in O(1).

```
nums   = [1, 2, 3, 4, 5]
prefix = [1, 3, 6, 10, 15]

sum(1, 3) = prefix[3] - prefix[0] = 10 - 1 = 9 ✓
(nums[1] + nums[2] + nums[3] = 2 + 3 + 4 = 9)
```

```java
// Build prefix sum (with +1 padding for clean indexing)
int[] prefix = new int[nums.length + 1];
for (int i = 0; i < nums.length; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}

// Query sum of nums[l..r] (0-indexed)
int sum = prefix[r + 1] - prefix[l];
```

### Product of Array Except Self (LC 238)

Build prefix products from LEFT and suffix products from RIGHT.

```java
int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];

    // LEFT pass: result[i] = product of nums[0..i-1]
    result[0] = 1;
    for (int i = 1; i < n; i++) result[i] = result[i-1] * nums[i-1];

    // RIGHT pass: multiply by product of nums[i+1..n-1]
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] *= right;
        right *= nums[i];
    }
    return result;
}
```

---

## 5.2 Prefix Sum + HashMap (Subarray = K)

### Concept

`prefix[j] - prefix[i] = k` means subarray `(i, j]` has sum k.
Rearranged: `prefix[i] = prefix[j] - k`.
So: as you compute running sum, check how many times `(sum - k)` was seen before.

```
nums = [1, 2, 3], k = 3
i=0: sum=1, map={0:1}, check (1-3)=-2 → 0 times
i=1: sum=3, map={0:1,1:1}, check (3-3)=0 → 1 time ← [1,2]
i=2: sum=6, map={0:1,1:1,3:1}, check (6-3)=3 → 1 time ← [3]
Total: 2
```

### Subarray Sum Equals K (LC 560)

```java
int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, 1); // empty prefix seen once — CRITICAL

    int sum = 0, count = 0;
    for (int num : nums) {
        sum += num;
        count += map.getOrDefault(sum - k, 0);
        map.merge(sum, 1, Integer::sum);
    }
    return count;
}
```

### Contiguous Array — Equal 0s and 1s (LC 525)

**Trick:** replace 0 with -1. Now find longest subarray with sum = 0.

```java
int findMaxLength(int[] nums) {
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, -1); // sum=0 seen at index -1

    int sum = 0, maxLen = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += (nums[i] == 0) ? -1 : 1;
        if (map.containsKey(sum)) {
            maxLen = Math.max(maxLen, i - map.get(sum));
        } else {
            map.put(sum, i); // store FIRST occurrence only
        }
    }
    return maxLen;
}
```

### Subarray Sums Divisible by K (LC 974)

```java
int subarraysDivByK(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, 1);

    int sum = 0, count = 0;
    for (int num : nums) {
        sum = ((sum + num) % k + k) % k; // handle negative mod
        count += map.getOrDefault(sum, 0);
        map.merge(sum, 1, Integer::sum);
    }
    return count;
}
```

---

## 5.3 2D Prefix Sum

```java
// Build 2D prefix sum
int[][] p = new int[m + 1][n + 1];
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        p[i][j] = matrix[i-1][j-1]
                + p[i-1][j] + p[i][j-1] - p[i-1][j-1];

// Query sum in rectangle (r1,c1) to (r2,c2) — 0-indexed input
int sum = p[r2+1][c2+1] - p[r1][c2+1] - p[r2+1][c1] + p[r1][c1];
```

### BYTS Problems — Phase 5

| # | Problem | Pattern |
|---|---------|---------|
| 238 | Product of Array Except Self | Prefix × suffix |
| 560 | Subarray Sum Equals K | Prefix + HashMap |
| 525 | Contiguous Array | Prefix + HashMap |
| 974 | Subarray Sums Divisible K | Prefix mod + HashMap |
| 523 | Continuous Subarray Sum | Prefix mod |
| 1277 | Count Square Submatrices | 2D DP / prefix |

---

# 🟤 Phase 6 — Sorting-Based Patterns

---

## 6.1 Sort + Two Pointers

After sorting, two-pointer logic works because elements are ordered.

```java
// Already covered in Phase 3 (3Sum, 4Sum, Boats)
// Key pattern: sort first, then apply greedy two-pointer
```

---

## 6.2 Sort + Greedy — Intervals / Scheduling

### Merge Intervals (LC 56)

```java
int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]); // sort by start
    List<int[]> result = new ArrayList<>();

    for (int[] iv : intervals) {
        if (result.isEmpty() || result.get(result.size()-1)[1] < iv[0]) {
            result.add(iv); // no overlap
        } else {
            // overlap → extend end
            result.get(result.size()-1)[1] =
                Math.max(result.get(result.size()-1)[1], iv[1]);
        }
    }
    return result.toArray(new int[0][]);
}
```

### Non-overlapping Intervals (LC 435) — Min Removals

```java
int eraseOverlapIntervals(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]); // sort by END
    int count = 0, prevEnd = intervals[0][1];

    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] < prevEnd) {
            count++; // overlap → remove current
        } else {
            prevEnd = intervals[i][1]; // no overlap → update end
        }
    }
    return count;
}
```

---

## 6.3 Bucket Sort / Counting Sort

When values are in a known small range, sort in O(N) using counts.

```java
// SORT COLORS (LC 75) → Dutch flag (covered above)

// COUNTING SORT for small integer range
void countingSort(int[] arr, int maxVal) {
    int[] count = new int[maxVal + 1];
    for (int n : arr) count[n]++;

    int idx = 0;
    for (int i = 0; i <= maxVal; i++) {
        while (count[i]-- > 0) arr[idx++] = i;
    }
}
```

---

## 6.4 Custom Comparator Sort

### Largest Number (LC 179)

```java
String largestNumber(int[] nums) {
    String[] strs = new String[nums.length];
    for (int i = 0; i < nums.length; i++) strs[i] = String.valueOf(nums[i]);

    // Compare "ab" vs "ba" — whichever concatenation is bigger
    Arrays.sort(strs, (a, b) -> (b + a).compareTo(a + b));

    if (strs[0].equals("0")) return "0";
    return String.join("", strs);
}
```

### K Closest Points to Origin (LC 973)

```java
int[][] kClosest(int[][] points, int k) {
    Arrays.sort(points, (a, b) ->
        (a[0]*a[0] + a[1]*a[1]) - (b[0]*b[0] + b[1]*b[1]));
    return Arrays.copyOfRange(points, 0, k);
}
```

---

# 🟣 Phase 7 — Kadane's & Subarray Extremes

---

## 7.1 Maximum Subarray — Kadane's (LC 53)

### Concept

At each index, decide: start fresh or extend existing subarray?

```
dp[i] = max(nums[i], dp[i-1] + nums[i])
      = "start fresh OR extend"

Trace: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
curr:  [-2, 1, -2, 4,  3, 5, 6,  1, 5]
max at each step tracks global answer = 6
```

```java
int maxSubArray(int[] nums) {
    int sum = 0, maxSum = Integer.MIN_VALUE;

    for (int num : nums) {
        sum += num;
        maxSum = Math.max(maxSum, sum); // update EVERY time
        if (sum < 0) sum = 0;           // reset when negative
    }
    return maxSum;
}
```

---

## 7.2 Maximum Product Subarray (LC 152)

**Key insight:** negative × negative = positive. Track BOTH max and min.

```java
int maxProduct(int[] nums) {
    int maxP = nums[0], minP = nums[0], result = nums[0];

    for (int i = 1; i < nums.length; i++) {
        int temp = maxP;
        maxP = Math.max(nums[i], Math.max(maxP*nums[i], minP*nums[i]));
        minP = Math.min(nums[i], Math.min(temp*nums[i], minP*nums[i]));
        result = Math.max(result, maxP);
    }
    return result;
}
```

---

## 7.3 Maximum Sum Circular Subarray (LC 918)

Two cases:
- Case 1: max subarray does NOT wrap → standard Kadane's
- Case 2: max subarray WRAPS → total sum - min subarray

```java
int maxSubarraySumCircular(int[] nums) {
    int totalSum = 0;
    int maxSum = nums[0], curMax = 0;
    int minSum = nums[0], curMin = 0;

    for (int num : nums) {
        curMax = Math.max(curMax + num, num);
        maxSum = Math.max(maxSum, curMax);

        curMin = Math.min(curMin + num, num);
        minSum = Math.min(minSum, curMin);

        totalSum += num;
    }
    // If all negative, maxSum handles it (wrap would give 0)
    return maxSum > 0 ? Math.max(maxSum, totalSum - minSum) : maxSum;
}
```

---

# ⚪ Phase 8 — Cyclic Sort & In-Place

---

## 8.1 Cyclic Sort (LC 268, 448, 442, 41)

### Concept

When array contains values 1 to N, each value has a "home" index: value v belongs at index v-1.
Walk and swap every element to its home. Then scan for anomalies.

```
nums = [3, 1, 4, 2]
i=0: nums[0]=3, home=2, swap → [4,1,3,2]
i=0: nums[0]=4, home=3, swap → [2,1,3,4]
i=0: nums[0]=2, home=1, swap → [1,2,3,4]
i=0: nums[0]=1, home=0 ✓, i++
i=1: nums[1]=2, home=1 ✓, i++
... sorted!
```

```java
// GENERIC CYCLIC SORT
void cyclicSort(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int home = nums[i] - 1; // where nums[i] belongs
        if (nums[i] != nums[home]) {
            int tmp = nums[i]; nums[i] = nums[home]; nums[home] = tmp;
        } else {
            i++;
        }
    }
}
```

### Missing Number (LC 268)

```java
int missingNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int home = nums[i];
        if (nums[i] < nums.length && nums[i] != nums[home]) {
            int tmp = nums[i]; nums[i] = nums[home]; nums[home] = tmp;
        } else i++;
    }
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != j) return j;
    }
    return nums.length;
}
```

### Find All Disappeared Numbers (LC 448)

```java
List<Integer> findDisappearedNumbers(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int home = nums[i] - 1;
        if (nums[i] != nums[home]) {
            int tmp = nums[i]; nums[i] = nums[home]; nums[home] = tmp;
        } else i++;
    }
    List<Integer> result = new ArrayList<>();
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != j + 1) result.add(j + 1);
    }
    return result;
}
```

---

## 8.2 In-Place Negation Trick

**Use sign of value at index as a "visited" marker.**
Negate nums[abs(nums[i])-1] to mark that value was seen.

```java
// FIND ALL DUPLICATES (LC 442)
List<Integer> findDuplicates(int[] nums) {
    List<Integer> result = new ArrayList<>();

    for (int i = 0; i < nums.length; i++) {
        int idx = Math.abs(nums[i]) - 1;
        if (nums[idx] < 0) {
            result.add(Math.abs(nums[i])); // already negative = seen before
        } else {
            nums[idx] = -nums[idx]; // mark as seen
        }
    }
    return result;
}
```

---

## 8.3 First Missing Positive (LC 41)

```java
int firstMissingPositive(int[] nums) {
    int n = nums.length;
    // Cyclic sort for valid range 1..n
    int i = 0;
    while (i < n) {
        int home = nums[i] - 1;
        if (nums[i] > 0 && nums[i] <= n && nums[i] != nums[home]) {
            int tmp = nums[i]; nums[i] = nums[home]; nums[home] = tmp;
        } else i++;
    }
    for (int j = 0; j < n; j++) {
        if (nums[j] != j + 1) return j + 1;
    }
    return n + 1;
}
```

### BYTS Problems — Phase 8

| # | Problem | Pattern |
|---|---------|---------|
| 268 | Missing Number | Cyclic sort |
| 448 | Find All Disappeared Numbers | Cyclic sort |
| 442 | Find All Duplicates | In-place negate |
| 41 | First Missing Positive | Cyclic sort |
| 287 | Find Duplicate Number | Floyd's cycle |
| 73 | Set Matrix Zeroes | In-place marker |
| 48 | Rotate Image | In-place transform |

---

# 🔵 Phase 9 — Binary Search on Arrays

---

## 9.1 Classic Binary Search (LC 704)

```java
int search(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2; // avoids overflow
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;
}
```

---

## 9.2 Left / Right Boundary Search

```java
// FIRST OCCURRENCE (left boundary)
int findFirst(int[] nums, int target) {
    int l = 0, r = nums.length - 1, ans = -1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) { ans = mid; r = mid - 1; } // go LEFT
        else if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return ans;
}

// LAST OCCURRENCE (right boundary)
int findLast(int[] nums, int target) {
    int l = 0, r = nums.length - 1, ans = -1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) { ans = mid; l = mid + 1; } // go RIGHT
        else if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return ans;
}
```

### Find First and Last Position (LC 34)

```java
int[] searchRange(int[] nums, int target) {
    return new int[]{findFirst(nums, target), findLast(nums, target)};
}
```

---

## 9.3 Rotated Sorted Array

### Find Minimum (LC 153)

```java
int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] > nums[r]) l = mid + 1; // min is in right half
        else r = mid;                          // min is in left half (or mid)
    }
    return nums[l];
}
```

### Search in Rotated Array (LC 33)

```java
int search(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;

        if (nums[l] <= nums[mid]) { // LEFT half sorted
            if (nums[l] <= target && target < nums[mid]) r = mid - 1;
            else l = mid + 1;
        } else {                    // RIGHT half sorted
            if (nums[mid] < target && target <= nums[r]) l = mid + 1;
            else r = mid - 1;
        }
    }
    return -1;
}
```

---

## 9.4 Binary Search on Answer

**When:** problem asks for minimum/maximum value satisfying a condition.
**Template:** binary search over the ANSWER range, check if `mid` is feasible.

```java
// KOKO EATING BANANAS (LC 875)
int minEatingSpeed(int[] piles, int h) {
    int l = 1, r = Arrays.stream(piles).max().getAsInt();

    while (l < r) {
        int mid = l + (r - l) / 2;
        if (canEat(piles, mid, h)) r = mid; // mid works, try smaller
        else l = mid + 1;
    }
    return l;
}

boolean canEat(int[] piles, int speed, int h) {
    int hours = 0;
    for (int p : piles) hours += (p + speed - 1) / speed; // ceil(p/speed)
    return hours <= h;
}
```

### BYTS Problems — Phase 9

| # | Problem | Pattern |
|---|---------|---------|
| 704 | Binary Search | Classic |
| 35 | Search Insert Position | Classic |
| 34 | Find First and Last Position | Boundary search |
| 153 | Find Minimum in Rotated | Rotated |
| 33 | Search in Rotated Sorted | Rotated |
| 81 | Search in Rotated II (dups) | Rotated |
| 162 | Find Peak Element | Binary search |
| 540 | Single Element in Sorted | Binary search |
| 875 | Koko Eating Bananas | Search on answer |
| 1011 | Capacity to Ship Packages | Search on answer |
| 1482 | Min Days to Make Bouquets | Search on answer |
| 2064 | Minimized Maximum Products | Search on answer |
| 410 | Split Array Largest Sum | Search on answer |

---

# ⚫ Phase 10 — Product & XOR Tricks

---

## 10.1 Product Except Self (LC 238)

Already covered in Phase 5.

```java
// LEFT prefix × RIGHT suffix in two passes
// No division needed, O(1) extra space
```

---

## 10.2 XOR Tricks

**XOR properties:**
```
x ^ x = 0   (same number cancels)
x ^ 0 = x   (identity)
Commutative + Associative
```

```java
// MISSING NUMBER (LC 268)
int missingNumber(int[] nums) {
    int xor = 0;
    for (int i = 0; i <= nums.length; i++) xor ^= i;
    for (int n : nums) xor ^= n;
    return xor; // all pairs cancel, missing remains
}

// SINGLE NUMBER (LC 136) — all appear twice except one
int singleNumber(int[] nums) {
    int result = 0;
    for (int n : nums) result ^= n;
    return result;
}

// FIND THE DIFFERENCE (LC 389) — one extra char added
char findTheDifference(String s, String t) {
    char result = 0;
    for (char c : s.toCharArray()) result ^= c;
    for (char c : t.toCharArray()) result ^= c;
    return result;
}
```

### BYTS Problems — Phase 10

| # | Problem | Pattern |
|---|---------|---------|
| 238 | Product of Array Except Self | Prefix × suffix |
| 268 | Missing Number | XOR |
| 136 | Single Number | XOR |
| 389 | Find the Difference | XOR |
| 137 | Single Number II | Bit counting |
| 260 | Single Number III | XOR split |

---

# 📋 Master Java Syntax Sheet — Arrays

```java
// ── ARRAY CREATION ────────────────────────────────────────────
int[] a = new int[n];
int[] a = {1,2,3};
int[][] m = new int[rows][cols];

// ── SORT ──────────────────────────────────────────────────────
Arrays.sort(a);
Arrays.sort(a, (x,y) -> y - x);           // desc (Integer[] only)
Arrays.sort(mat, (x,y) -> x[0] - y[0]);   // 2D by col 0

// ── FILL / COPY ───────────────────────────────────────────────
Arrays.fill(a, 0);
int[] c = a.clone();
int[] c = Arrays.copyOfRange(a, l, r);    // [l, r)

// ── SWAP ──────────────────────────────────────────────────────
int t = a[i]; a[i] = a[j]; a[j] = t;

// ── TWO POINTER STARTERS ──────────────────────────────────────
int i = 0, j = a.length - 1;              // opposite ends
int slow = 0;                              // fast/slow write
int low = 0, mid = 0, high = a.length-1;  // Dutch flag

// ── SLIDING WINDOW ────────────────────────────────────────────
Map<Integer,Integer> window = new HashMap<>();
window.merge(x, 1, Integer::sum);         // add to window
window.merge(x, -1, Integer::sum);        // remove from window
if (window.get(x) == 0) window.remove(x);

// ── PREFIX SUM ────────────────────────────────────────────────
int[] pre = new int[n+1];
pre[i+1] = pre[i] + nums[i];
int sum = pre[r+1] - pre[l];             // sum of [l..r]

// ── BINARY SEARCH ─────────────────────────────────────────────
int l = 0, r = n-1;
while (l <= r) {
    int mid = l + (r-l)/2;
    if (nums[mid] == target) return mid;
    else if (nums[mid] < target) l = mid+1;
    else r = mid-1;
}

// ── KADANE'S ──────────────────────────────────────────────────
int sum = 0, maxSum = Integer.MIN_VALUE;
for (int n : nums) {
    sum += n;
    maxSum = Math.max(maxSum, sum);
    if (sum < 0) sum = 0;
}

// ── FREQUENCY ─────────────────────────────────────────────────
int[] freq = new int[26];
freq[c - 'a']++;
Map<Integer,Integer> map = new HashMap<>();
map.merge(n, 1, Integer::sum);
map.getOrDefault(n, 0);
```

---

# 🧭 Pattern Decision Table

```
SIGNAL IN PROBLEM                    → PATTERN
──────────────────────────────────────────────────────
"Find pair summing to target"        → HashMap complement
"Count frequencies"                  → int[26] or HashMap
"Group by property"                  → HashMap + List
"Consecutive sequence"               → HashSet, start-only
Sorted + find pair                   → Two pointers opposite ends
Remove/move in-place                 → Fast/slow write pointer
Sort 3 categories                    → Dutch national flag
Fixed subarray/substring size        → Fixed sliding window
Longest subarray with condition      → Dynamic window (longest)
Shortest subarray with condition     → Dynamic window (shortest)
"Subarray sum = k" — count           → Prefix sum + HashMap
"Product/sum of range"               → Prefix sum array
Max subarray sum                     → Kadane's
Max product subarray                 → Kadane's + track min
Numbers in range 1..N               → Cyclic sort
Find missing/duplicate in 1..N      → Cyclic sort
Mark visited without extra space     → In-place negation
Sorted array, find target            → Binary search
Find first/last occurrence           → Binary search boundary
"Minimum X that satisfies cond"      → Binary search on answer
"Product except self"                → Prefix × suffix product
Find single unique/missing           → XOR trick
```

---

# 📚 All BYTS Array Problems by Phase

**Phase 2 (HashMap):** 1, 49, 128, 217, 219, 242, 347, 349, 350, 389

**Phase 3 (Two Pointers):** 11, 15, 16, 18, 26, 27, 42, 75, 80, 167, 259, 283, 349, 392, 881, 905, 977, 2337, 2938

**Phase 4 (Sliding Window):** 3, 76, 209, 239, 424, 438, 567, 643, 713, 862, 904, 1004, 1438, 1493, 1658, 1838, 2461, 2516, 2779, 2981

**Phase 5 (Prefix Sum):** 238, 523, 525, 560, 974, 1277

**Phase 6 (Sort):** 56, 75, 179, 253, 435, 973

**Phase 7 (Kadane's):** 53, 121, 152, 918

**Phase 8 (Cyclic/In-Place):** 41, 48, 54, 73, 268, 287, 442, 448

**Phase 9 (Binary Search):** 33, 34, 35, 81, 153, 162, 540, 704, 875, 1011, 1482, 2064, 410

**Phase 10 (XOR/Product):** 136, 137, 238, 260, 268, 389

---

*Updated: 2026-06-10 | Java | BYTS Sheet*
*All array patterns: HashMap · Two Pointers · Sliding Window · Prefix Sum · Kadane · Cyclic Sort · Binary Search · XOR*
