# 🔤 Strings — Complete Course
**Language:** Java | **Level:** SDE Interview Ready
**Based on:** BYTS Sheet Problems
**Style:** Concept → Visual → Why it works → Java Code → Problems

---

# 📌 Contents

**Phase 1 — String Foundations**
- 1.1 Strings in Java — How They Work
- 1.2 String vs StringBuilder vs char[]
- 1.3 Essential Java String Syntax

**Phase 2 — Character Frequency & Anagram**
- 2.1 Frequency Array (int[26])
- 2.2 HashMap Frequency
- 2.3 Anagram Check
- 2.4 Group Anagrams
- 2.5 Find All Anagrams (Sliding Window)

**Phase 3 — Two Pointer on Strings**
- 3.1 Palindrome Check
- 3.2 Valid Palindrome with Filtering
- 3.3 Valid Palindrome II (One Delete)
- 3.4 Reverse Patterns
- 3.5 Is Subsequence

**Phase 4 — Sliding Window on Strings**
- 4.1 Longest Substring Without Repeating
- 4.2 Minimum Window Substring
- 4.3 Longest Repeating Character Replacement
- 4.4 Permutation in String
- 4.5 Find All Anagrams

**Phase 5 — String Manipulation & Building**
- 5.1 Reverse Techniques
- 5.2 String Compression
- 5.3 Encode & Decode
- 5.4 Decode String (Stack)
- 5.5 StringBuilder Patterns

**Phase 6 — Palindrome Patterns**
- 6.1 Expand from Center
- 6.2 Longest Palindromic Substring
- 6.3 Count Palindromic Substrings
- 6.4 Palindrome Number

**Phase 7 — Stack-Based String Problems**
- 7.1 Valid Parentheses
- 7.2 Basic Calculator
- 7.3 Remove Invalid Parentheses
- 7.4 Simplify Path
- 7.5 Asteroid Collision

**Phase 8 — String Search & Matching**
- 8.1 Find First Occurrence (KMP Concept)
- 8.2 Repeated Substring Pattern
- 8.3 Rotate String
- 8.4 Backspace String Compare

**Phase 9 — Number ↔ String Conversion**
- 9.1 Reverse Integer
- 9.2 String to Integer (atoi)
- 9.3 Roman to Integer / Integer to Roman
- 9.4 Multiply Strings
- 9.5 Plus One

**Phase 10 — Trie (Prefix Tree)**
- 10.1 Trie Structure
- 10.2 Insert & Search
- 10.3 Design Add and Search Words
- 10.4 Word Search II

**Master Reference**
- Java String Syntax Sheet
- Pattern Decision Table
- All BYTS Problems by Pattern

---

# 🟢 Phase 1 — String Foundations

---

## 1.1 Strings in Java — How They Work

### Immutability

Strings in Java are **IMMUTABLE**. Every modification creates a NEW string.

```java
String s = "hello";
s = s + " world"; // creates a NEW string, doesn't modify original

// This is O(N²) in a loop — NEVER do this:
for (int i = 0; i < n; i++) result += chars[i]; // 🚫 slow

// DO THIS instead:
StringBuilder sb = new StringBuilder();
for (int i = 0; i < n; i++) sb.append(chars[i]); // ✅ O(N)
```

### Memory Model

```
String pool:
"hello" → stored once, shared by reference
String s1 = "hello";  → points to pool
String s2 = "hello";  → points to SAME pool entry
s1 == s2  → true (same reference)

new String("hello") → creates NEW object on heap
s1 == new String("hello") → false!

Always use .equals() for content comparison
```

### String Interning

```java
// ✅ ALWAYS use .equals() for string content comparison
s1.equals(s2)
s1.equalsIgnoreCase(s2)

// ❌ Don't use == for strings (compares reference, not content)
```

---

## 1.2 String vs StringBuilder vs char[]

```
String          → immutable, safe, slow for modification
StringBuilder   → mutable, fast for append/delete/reverse
char[]          → raw array, good for in-place character operations
```

### When to use which?

```
Building result string in loop   → StringBuilder
Reverse a string                 → char[] or StringBuilder.reverse()
Count/modify characters          → char[]
Check content equality           → String.equals()
Return as String                 → new String(charArray) or sb.toString()
```

---

## 1.3 Essential Java String Syntax

```java
// ── LENGTH & ACCESS ───────────────────────────────────────────
s.length()                         // NOT s.length (unlike arrays)
s.charAt(i)                        // character at index i
s.indexOf('c')                     // first index of char
s.lastIndexOf('c')                 // last index of char
s.indexOf("sub")                   // first index of substring

// ── SUBSTRING ─────────────────────────────────────────────────
s.substring(i)                     // from i to end
s.substring(i, j)                  // [i, j) — j is exclusive

// ── COMPARISON ────────────────────────────────────────────────
s.equals(t)
s.equalsIgnoreCase(t)
s.compareTo(t)                     // lexicographic
s.startsWith("pre")
s.endsWith("suf")
s.contains("mid")

// ── TRANSFORM ─────────────────────────────────────────────────
s.toLowerCase()
s.toUpperCase()
s.trim()                           // remove leading/trailing whitespace
s.strip()                          // like trim but Unicode-aware
s.replace('a', 'b')               // char replacement
s.replace("old", "new")           // substring replacement
s.replaceAll("regex", "new")

// ── SPLIT ─────────────────────────────────────────────────────
s.split(" ")                       // split by space
s.split("\\s+")                    // split by any whitespace
s.split(",")                       // split by comma
String[] parts = "a:b:c".split(":");

// ── JOIN ──────────────────────────────────────────────────────
String.join(", ", "a", "b", "c")  // "a, b, c"
String.join(".", parts)

// ── CONVERT ───────────────────────────────────────────────────
char[] arr = s.toCharArray()
String s = new String(arr)
String s = String.valueOf(num)
int n = Integer.parseInt(s)
char c = s.charAt(0)

// ── CHARACTER OPERATIONS ──────────────────────────────────────
Character.isLetter(c)
Character.isDigit(c)
Character.isLetterOrDigit(c)
Character.isWhitespace(c)
Character.toLowerCase(c)
Character.toUpperCase(c)
c - 'a'                            // char to 0-25 index
(char)('a' + i)                    // index to char

// ── STRINGBUILDER ─────────────────────────────────────────────
StringBuilder sb = new StringBuilder();
sb.append(c)                       // add char
sb.append(str)                     // add string
sb.deleteCharAt(sb.length() - 1)  // remove last
sb.reverse()                       // reverse in place
sb.toString()                      // convert to String
sb.length()
sb.charAt(i)
sb.setLength(n)                    // truncate to length n
sb.insert(i, c)                    // insert at index
sb.delete(i, j)                    // delete [i, j)
```

---

# 🔵 Phase 2 — Character Frequency & Anagram

---

## 2.1 Frequency Array (int[26])

### Concept

For lowercase English letters only. Index 0-25 maps to 'a'-'z'.
Faster than HashMap — O(1) for any char, O(26) to compare.

```java
// Count frequencies
int[] freq = new int[26];
for (char c : s.toCharArray()) freq[c - 'a']++;

// Check if all zeros (anagram or empty)
boolean allZero(int[] freq) {
    for (int n : freq) if (n != 0) return false;
    return true;
}

// Compare two frequency arrays
Arrays.equals(freq1, freq2);
```

---

## 2.2 HashMap Frequency

For arbitrary characters (Unicode, digits, etc.):

```java
Map<Character, Integer> freq = new HashMap<>();
for (char c : s.toCharArray()) freq.merge(c, 1, Integer::sum);
freq.getOrDefault(c, 0);
```

---

## 2.3 Anagram Check

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

---

## 2.4 Group Anagrams (LC 49)

**Key:** sort each string → same sorted result = same anagram group.

```java
List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();

    for (String s : strs) {
        char[] arr = s.toCharArray();
        Arrays.sort(arr);
        String key = new String(arr); // sorted = canonical form

        map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
    }

    return new ArrayList<>(map.values());
}
```

**Alternative key — frequency string** (when sorting is too slow):

```java
// Encode frequency as "#count#count..." (26 numbers)
String getKey(String s) {
    int[] count = new int[26];
    for (char c : s.toCharArray()) count[c - 'a']++;
    StringBuilder sb = new StringBuilder();
    for (int n : count) sb.append('#').append(n);
    return sb.toString();
}
```

---

## 2.5 Find All Anagrams in String (LC 438)

Use sliding window of size `p.length()` with frequency comparison.

```java
List<Integer> findAnagrams(String s, String p) {
    int[] pCount = new int[26];
    int[] wCount = new int[26];
    for (char c : p.toCharArray()) pCount[c - 'a']++;

    List<Integer> result = new ArrayList<>();
    int k = p.length();

    for (int i = 0; i < s.length(); i++) {
        wCount[s.charAt(i) - 'a']++;     // add right

        if (i >= k) {
            wCount[s.charAt(i - k) - 'a']--; // remove left
        }

        if (Arrays.equals(pCount, wCount)) {
            result.add(i - k + 1); // window start index
        }
    }
    return result;
}
```

### BYTS Problems — Phase 2

| # | Problem | Pattern |
|---|---------|---------|
| 242 | Valid Anagram | Frequency array |
| 49 | Group Anagrams | Sort key map |
| 438 | Find All Anagrams | Fixed window + freq |
| 567 | Permutation in String | Fixed window + freq |
| 389 | Find the Difference | XOR / freq |
| 383 | Ransom Note | Frequency count |
| 451 | Sort Characters By Frequency | Freq + sort |

---

# 🟡 Phase 3 — Two Pointers on Strings

---

## 3.1 Palindrome Check

A palindrome reads the same forwards and backwards.

```java
boolean isPalindrome(String s) {
    int l = 0, r = s.length() - 1;
    while (l < r) {
        if (s.charAt(l) != s.charAt(r)) return false;
        l++; r--;
    }
    return true;
}

// For char array
boolean isPalindrome(char[] arr) {
    int l = 0, r = arr.length - 1;
    while (l < r) {
        if (arr[l++] != arr[r--]) return false;
    }
    return true;
}
```

---

## 3.2 Valid Palindrome with Filtering (LC 125)

Only consider alphanumeric characters. Case-insensitive.

```java
boolean isPalindrome(String s) {
    int l = 0, r = s.length() - 1;

    while (l < r) {
        // skip non-alphanumeric
        while (l < r && !Character.isLetterOrDigit(s.charAt(l))) l++;
        while (l < r && !Character.isLetterOrDigit(s.charAt(r))) r--;

        if (Character.toLowerCase(s.charAt(l))
                != Character.toLowerCase(s.charAt(r))) return false;
        l++; r--;
    }
    return true;
}
```

---

## 3.3 Valid Palindrome II — One Delete Allowed (LC 680)

If mismatch: try skipping either left or right char and check rest.

```java
boolean validPalindrome(String s) {
    int l = 0, r = s.length() - 1;

    while (l < r) {
        if (s.charAt(l) != s.charAt(r)) {
            // try skipping l or skipping r
            return isPalin(s, l+1, r) || isPalin(s, l, r-1);
        }
        l++; r--;
    }
    return true;
}

boolean isPalin(String s, int l, int r) {
    while (l < r) {
        if (s.charAt(l++) != s.charAt(r--)) return false;
    }
    return true;
}
```

---

## 3.4 Reverse Patterns

### Reverse String (LC 344) — in-place on char[]

```java
void reverseString(char[] s) {
    int l = 0, r = s.length - 1;
    while (l < r) {
        char t = s[l]; s[l] = s[r]; s[r] = t;
        l++; r--;
    }
}
```

### Reverse Words in String (LC 151)

```java
String reverseWords(String s) {
    String[] words = s.trim().split("\\s+"); // split on any whitespace
    StringBuilder sb = new StringBuilder();
    for (int i = words.length - 1; i >= 0; i--) {
        sb.append(words[i]);
        if (i > 0) sb.append(' ');
    }
    return sb.toString();
}
```

### Reverse Vowels of String (LC 345)

```java
String reverseVowels(String s) {
    char[] arr = s.toCharArray();
    String vowels = "aeiouAEIOU";
    int l = 0, r = arr.length - 1;

    while (l < r) {
        while (l < r && vowels.indexOf(arr[l]) == -1) l++;
        while (l < r && vowels.indexOf(arr[r]) == -1) r--;
        char t = arr[l]; arr[l] = arr[r]; arr[r] = t;
        l++; r--;
    }
    return new String(arr);
}
```

### Reverse String II (LC 541) — Reverse every 2k chars, first k

```java
String reverseStr(String s, int k) {
    char[] arr = s.toCharArray();
    for (int i = 0; i < arr.length; i += 2 * k) {
        int l = i, r = Math.min(i + k - 1, arr.length - 1);
        while (l < r) {
            char t = arr[l]; arr[l] = arr[r]; arr[r] = t;
            l++; r--;
        }
    }
    return new String(arr);
}
```

---

## 3.5 Is Subsequence (LC 392)

```java
boolean isSubsequence(String s, String t) {
    int i = 0; // pointer for s
    for (int j = 0; j < t.length() && i < s.length(); j++) {
        if (s.charAt(i) == t.charAt(j)) i++;
    }
    return i == s.length();
}
```

### BYTS Problems — Phase 3

| # | Problem | Pattern |
|---|---------|---------|
| 125 | Valid Palindrome | Two pointer + filter |
| 680 | Valid Palindrome II | Two pointer + branch |
| 344 | Reverse String | Two pointer in-place |
| 345 | Reverse Vowels | Two pointer filter |
| 541 | Reverse String II | Block reverse |
| 151 | Reverse Words in String | Split + reverse |
| 392 | Is Subsequence | Fixed gap pointer |

---

# 🟠 Phase 4 — Sliding Window on Strings

---

## 4.1 Longest Substring Without Repeating (LC 3)

```java
int lengthOfLongestSubstring(String s) {
    Set<Character> window = new HashSet<>();
    int i = 0, maxLen = 0;

    for (int j = 0; j < s.length(); j++) {
        // shrink window until no duplicate
        while (window.contains(s.charAt(j))) {
            window.remove(s.charAt(i++));
        }
        window.add(s.charAt(j));
        maxLen = Math.max(maxLen, j - i + 1);
    }
    return maxLen;
}
```

**Optimized** — jump directly to the conflicting character's position + 1:

```java
int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> lastIdx = new HashMap<>();
    int i = 0, maxLen = 0;

    for (int j = 0; j < s.length(); j++) {
        char c = s.charAt(j);
        if (lastIdx.containsKey(c) && lastIdx.get(c) >= i) {
            i = lastIdx.get(c) + 1; // jump past duplicate
        }
        lastIdx.put(c, j);
        maxLen = Math.max(maxLen, j - i + 1);
    }
    return maxLen;
}
```

---

## 4.2 Minimum Window Substring (LC 76)

```java
String minWindow(String s, String t) {
    if (s.isEmpty() || t.isEmpty()) return "";

    Map<Character, Integer> need = new HashMap<>();
    for (char c : t.toCharArray()) need.merge(c, 1, Integer::sum);

    int have = 0, total = need.size();
    int i = 0, minLen = Integer.MAX_VALUE, resL = 0;
    Map<Character, Integer> window = new HashMap<>();

    for (int j = 0; j < s.length(); j++) {
        char c = s.charAt(j);
        window.merge(c, 1, Integer::sum);

        if (need.containsKey(c)
                && window.get(c).equals(need.get(c))) have++;

        while (have == total) {
            if (j - i + 1 < minLen) {
                minLen = j - i + 1;
                resL = i;
            }
            char left = s.charAt(i);
            window.merge(left, -1, Integer::sum);
            if (need.containsKey(left)
                    && window.get(left) < need.get(left)) have--;
            i++;
        }
    }
    return minLen == Integer.MAX_VALUE ? ""
                                       : s.substring(resL, resL + minLen);
}
```

---

## 4.3 Longest Repeating Character Replacement (LC 424)

```java
int characterReplacement(String s, int k) {
    int[] count = new int[26];
    int i = 0, maxFreq = 0, maxLen = 0;

    for (int j = 0; j < s.length(); j++) {
        maxFreq = Math.max(maxFreq, ++count[s.charAt(j) - 'A']);

        // window is valid if we need <= k replacements
        while ((j - i + 1) - maxFreq > k) {
            count[s.charAt(i++) - 'A']--;
        }
        maxLen = Math.max(maxLen, j - i + 1);
    }
    return maxLen;
}
```

---

## 4.4 Permutation in String (LC 567)

Does s1's permutation appear as substring in s2?

```java
boolean checkInclusion(String s1, String s2) {
    if (s1.length() > s2.length()) return false;

    int[] need = new int[26], have = new int[26];
    for (char c : s1.toCharArray()) need[c - 'a']++;

    for (int i = 0; i < s2.length(); i++) {
        have[s2.charAt(i) - 'a']++;  // add right

        if (i >= s1.length()) {
            have[s2.charAt(i - s1.length()) - 'a']--; // remove left
        }
        if (Arrays.equals(need, have)) return true;
    }
    return false;
}
```

### BYTS Problems — Phase 4

| # | Problem | Pattern |
|---|---------|---------|
| 3 | Longest Substring No Repeat | Dynamic longest |
| 76 | Minimum Window Substring | Dynamic shortest |
| 424 | Longest Repeating Char Replacement | Dynamic longest |
| 567 | Permutation in String | Fixed + freq |
| 438 | Find All Anagrams | Fixed + freq |
| 904 | Fruit Into Baskets | Dynamic atMost 2 |
| 2981 | Find Longest Substring | Dynamic window |

---

# 🔴 Phase 5 — String Manipulation & Building

---

## 5.1 Reverse Array — Triple Reverse Pattern

Rotate array left by k: reverse all → reverse first n-k → reverse last k.

```java
// ROTATE ARRAY (LC 189) — same idea applies to strings
void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}

// ROTATE STRING (LC 796) — is s2 a rotation of s1?
boolean rotateString(String s, String goal) {
    return s.length() == goal.length()
        && (s + s).contains(goal); // doubled string contains all rotations
}
```

---

## 5.2 String Compression (LC 443)

```java
int compress(char[] chars) {
    int write = 0, i = 0;

    while (i < chars.length) {
        char curr = chars[i];
        int count = 0;

        while (i < chars.length && chars[i] == curr) {
            i++; count++;
        }

        chars[write++] = curr;

        if (count > 1) {
            for (char c : String.valueOf(count).toCharArray()) {
                chars[write++] = c;
            }
        }
    }
    return write;
}
```

---

## 5.3 Encode & Decode Strings (LC 271)

Design a codec where length# prefix allows unambiguous decoding.

```java
// Encode: prepend each string with "length#"
String encode(List<String> strs) {
    StringBuilder sb = new StringBuilder();
    for (String s : strs) {
        sb.append(s.length()).append('#').append(s);
    }
    return sb.toString();
}

// Decode: read length, jump to start of string
List<String> decode(String str) {
    List<String> result = new ArrayList<>();
    int i = 0;
    while (i < str.length()) {
        int j = str.indexOf('#', i);         // find separator
        int len = Integer.parseInt(str.substring(i, j));
        result.add(str.substring(j + 1, j + 1 + len));
        i = j + 1 + len;
    }
    return result;
}
```

---

## 5.4 Decode String (LC 394)

`k[encoded_string]` = repeat encoded_string k times.

**Key:** use two stacks — one for counts, one for strings built so far.

```java
String decodeString(String s) {
    Deque<Integer> countStack = new ArrayDeque<>();
    Deque<StringBuilder> strStack = new ArrayDeque<>();
    StringBuilder curr = new StringBuilder();
    int k = 0;

    for (char c : s.toCharArray()) {
        if (Character.isDigit(c)) {
            k = k * 10 + (c - '0'); // handle multi-digit numbers
        } else if (c == '[') {
            countStack.push(k);        // save count
            strStack.push(curr);       // save current string
            curr = new StringBuilder(); // start fresh
            k = 0;
        } else if (c == ']') {
            int times = countStack.pop();
            StringBuilder prev = strStack.pop();
            for (int i = 0; i < times; i++) prev.append(curr);
            curr = prev;
        } else {
            curr.append(c);
        }
    }
    return curr.toString();
}
```

---

## 5.5 StringBuilder Patterns

```java
// BUILD STRING FROM PARTS
StringBuilder sb = new StringBuilder();
for (String word : words) {
    if (sb.length() > 0) sb.append(' ');
    sb.append(word);
}
return sb.toString();

// REVERSE STRING
String reversed = new StringBuilder(s).reverse().toString();

// REMOVE LAST CHAR (backtrack pattern)
sb.deleteCharAt(sb.length() - 1);

// CHECK IF EMPTY
sb.length() == 0

// SNAPSHOT
String snapshot = sb.toString();

// SET LENGTH (trim to n chars)
sb.setLength(n);
```

---

# 🟣 Phase 6 — Palindrome Patterns

---

## 6.1 Expand from Center

For each index, try to expand outward as long as characters match.
Handle both ODD-length and EVEN-length palindromes.

```
ODD:  "racecar"  → center at 'e' (index 3)
EVEN: "abba"     → center between b and b (index 1,2)
```

```java
int expand(String s, int l, int r) {
    while (l >= 0 && r < s.length()
               && s.charAt(l) == s.charAt(r)) {
        l--; r++;
    }
    return r - l - 1; // palindrome length
}
```

---

## 6.2 Longest Palindromic Substring (LC 5)

```java
String longestPalindrome(String s) {
    int start = 0, maxLen = 1;

    for (int i = 0; i < s.length(); i++) {
        int len1 = expand(s, i, i);     // odd-length
        int len2 = expand(s, i, i + 1); // even-length
        int len = Math.max(len1, len2);

        if (len > maxLen) {
            maxLen = len;
            start = i - (len - 1) / 2;
        }
    }
    return s.substring(start, start + maxLen);
}

int expand(String s, int l, int r) {
    while (l >= 0 && r < s.length()
               && s.charAt(l) == s.charAt(r)) l--; r++;
    return r - l - 1;
}
```

---

## 6.3 Count Palindromic Substrings (LC 647)

```java
int countSubstrings(String s) {
    int count = 0;
    for (int i = 0; i < s.length(); i++) {
        count += countExpand(s, i, i);     // odd
        count += countExpand(s, i, i + 1); // even
    }
    return count;
}

int countExpand(String s, int l, int r) {
    int count = 0;
    while (l >= 0 && r < s.length()
               && s.charAt(l--) == s.charAt(r++)) {
        count++;
    }
    return count;
}
```

---

## 6.4 Palindrome Number (LC 9)

```java
boolean isPalindrome(int x) {
    if (x < 0) return false;
    if (x != 0 && x % 10 == 0) return false;

    int reversed = 0;
    while (x > reversed) {
        reversed = reversed * 10 + x % 10;
        x /= 10;
    }
    // x == reversed (even length) OR x == reversed/10 (odd length)
    return x == reversed || x == reversed / 10;
}
```

### BYTS Problems — Phase 6

| # | Problem | Pattern |
|---|---------|---------|
| 5 | Longest Palindromic Substring | Expand center |
| 647 | Palindromic Substrings | Expand center |
| 125 | Valid Palindrome | Two pointer |
| 680 | Valid Palindrome II | Two pointer + branch |
| 9 | Palindrome Number | Math reverse |
| 131 | Palindrome Partitioning | Backtracking + isPalin |

---

# ⚫ Phase 7 — Stack-Based String Problems

---

## 7.1 Valid Parentheses (LC 20)

**Rule:** every closing bracket must match the most recent unmatched opening bracket.

```java
boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    Map<Character, Character> map = Map.of(
        ')', '(', '}', '{', ']', '['
    );

    for (char c : s.toCharArray()) {
        if (map.containsKey(c)) {            // closing bracket
            if (stack.isEmpty() || stack.peek() != map.get(c))
                return false;
            stack.pop();
        } else {
            stack.push(c);                   // opening bracket
        }
    }
    return stack.isEmpty();
}
```

---

## 7.2 Basic Calculator II (LC 227)

Handle `+`, `-`, `*`, `/` with proper operator precedence.

```java
int calculate(String s) {
    Deque<Integer> stack = new ArrayDeque<>();
    int num = 0;
    char op = '+'; // start with +

    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);

        if (Character.isDigit(c)) {
            num = num * 10 + (c - '0'); // build multi-digit number
        }

        if ((!Character.isDigit(c) && c != ' ') || i == s.length()-1) {
            switch (op) {
                case '+': stack.push(num); break;
                case '-': stack.push(-num); break;
                case '*': stack.push(stack.pop() * num); break;
                case '/': stack.push(stack.pop() / num); break;
            }
            op = c;   // save current operator for next number
            num = 0;
        }
    }

    int result = 0;
    while (!stack.isEmpty()) result += stack.pop();
    return result;
}
```

---

## 7.3 Minimum Add to Make Parentheses Valid (LC 921)

Count unmatched opening and closing brackets.

```java
int minAddToMakeValid(String s) {
    int openNeeded = 0;  // unmatched ')' needing '('
    int closeNeeded = 0; // unmatched '(' needing ')'

    for (char c : s.toCharArray()) {
        if (c == '(') {
            closeNeeded++;    // need a matching ')'
        } else {
            if (closeNeeded > 0) closeNeeded--; // matched!
            else openNeeded++;                   // unmatched ')'
        }
    }
    return openNeeded + closeNeeded;
}
```

---

## 7.4 Simplify Path (LC 71)

```java
String simplifyPath(String path) {
    Deque<String> stack = new ArrayDeque<>();
    String[] parts = path.split("/");

    for (String p : parts) {
        if (p.equals("..")) {
            if (!stack.isEmpty()) stack.pop(); // go up one dir
        } else if (!p.isEmpty() && !p.equals(".")) {
            stack.push(p);                     // valid dir name
        }
    }

    StringBuilder sb = new StringBuilder();
    for (String dir : stack) sb.insert(0, "/" + dir);
    return sb.length() == 0 ? "/" : sb.toString();
}
```

---

## 7.5 Asteroid Collision (LC 735)

```java
int[] asteroidCollision(int[] asteroids) {
    Deque<Integer> stack = new ArrayDeque<>();

    for (int a : asteroids) {
        boolean alive = true;

        while (alive && a < 0 && !stack.isEmpty() && stack.peek() > 0) {
            int top = stack.peek();
            if (top < -a) {
                stack.pop();            // top destroyed
            } else if (top == -a) {
                stack.pop();            // both destroyed
                alive = false;
            } else {
                alive = false;          // a destroyed
            }
        }

        if (alive) stack.push(a);
    }

    int[] result = new int[stack.size()];
    for (int i = result.length - 1; i >= 0; i--)
        result[i] = stack.pop();
    return result;
}
```

### BYTS Problems — Phase 7

| # | Problem | Pattern |
|---|---------|---------|
| 20 | Valid Parentheses | Stack matching |
| 155 | Min Stack | Stack + min tracking |
| 150 | Evaluate Reverse Polish Notation | Stack evaluation |
| 227 | Basic Calculator II | Stack + operators |
| 224 | Basic Calculator | Stack + recursion |
| 394 | Decode String | Two stacks |
| 921 | Min Add to Make Valid | Parentheses count |
| 1249 | Min Remove to Make Valid | Stack + index |
| 71 | Simplify Path | Stack path |
| 735 | Asteroid Collision | Stack simulation |
| 32 | Longest Valid Parentheses | Stack + DP |

---

# 🔵 Phase 8 — String Search & Matching

---

## 8.1 Find First Occurrence (LC 28)

```java
// Built-in (use in interviews unless asked to implement)
int strStr(String haystack, String needle) {
    return haystack.indexOf(needle);
}

// Manual sliding window
int strStr(String haystack, String needle) {
    int h = haystack.length(), n = needle.length();
    for (int i = 0; i <= h - n; i++) {
        if (haystack.substring(i, i + n).equals(needle)) return i;
    }
    return -1;
}
```

---

## 8.2 Repeated Substring Pattern (LC 459)

**Key trick:** if s has a repeated pattern, then `(s + s)` without first and last characters still contains s.

```java
boolean repeatedSubstringPattern(String s) {
    String doubled = s + s;
    // Remove first and last char, check if s is still inside
    return doubled.substring(1, doubled.length() - 1).contains(s);
}
```

---

## 8.3 Rotate String (LC 796)

```java
boolean rotateString(String s, String goal) {
    return s.length() == goal.length() && (s + s).contains(goal);
}
```

---

## 8.4 Backspace String Compare (LC 844)

**Two pointer from the END** — process backspaces directly.

```java
boolean backspaceCompare(String s, String t) {
    int i = s.length() - 1, j = t.length() - 1;
    int skipS = 0, skipT = 0;

    while (i >= 0 || j >= 0) {
        while (i >= 0) {
            if (s.charAt(i) == '#') { skipS++; i--; }
            else if (skipS > 0) { skipS--; i--; }
            else break;
        }
        while (j >= 0) {
            if (t.charAt(j) == '#') { skipT++; j--; }
            else if (skipT > 0) { skipT--; j--; }
            else break;
        }
        if (i >= 0 && j >= 0 && s.charAt(i) != t.charAt(j)) return false;
        if ((i >= 0) != (j >= 0)) return false;
        i--; j--;
    }
    return true;
}
```

### BYTS Problems — Phase 8

| # | Problem | Pattern |
|---|---------|---------|
| 28 | Find First Occurrence | String search |
| 459 | Repeated Substring Pattern | Doubled string trick |
| 796 | Rotate String | Doubled string trick |
| 844 | Backspace String Compare | Two pointer from end |
| 686 | Repeated String Match | String repetition |

---

# 🟤 Phase 9 — Number ↔ String Conversion

---

## 9.1 Reverse Integer (LC 7)

```java
int reverse(int x) {
    long result = 0;
    while (x != 0) {
        result = result * 10 + x % 10;
        x /= 10;
    }
    // Check 32-bit overflow
    if (result > Integer.MAX_VALUE || result < Integer.MIN_VALUE) return 0;
    return (int) result;
}
```

---

## 9.2 String to Integer — atoi (LC 8)

Handle: leading spaces, sign, digits, non-digit stop, overflow.

```java
int myAtoi(String s) {
    int i = 0, n = s.length();
    long result = 0;

    // Step 1: skip leading spaces
    while (i < n && s.charAt(i) == ' ') i++;

    // Step 2: read sign
    int sign = 1;
    if (i < n && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
        sign = (s.charAt(i) == '-') ? -1 : 1;
        i++;
    }

    // Step 3: read digits
    while (i < n && Character.isDigit(s.charAt(i))) {
        result = result * 10 + (s.charAt(i) - '0');
        if (result * sign > Integer.MAX_VALUE) return Integer.MAX_VALUE;
        if (result * sign < Integer.MIN_VALUE) return Integer.MIN_VALUE;
        i++;
    }

    return (int)(result * sign);
}
```

---

## 9.3 Roman to Integer (LC 13)

```java
int romanToInt(String s) {
    Map<Character, Integer> map = Map.of(
        'I', 1, 'V', 5, 'X', 10, 'L', 50,
        'C', 100, 'D', 500, 'M', 1000
    );

    int result = 0;
    for (int i = 0; i < s.length(); i++) {
        int val = map.get(s.charAt(i));
        // If smaller value before larger → subtract
        if (i + 1 < s.length() && val < map.get(s.charAt(i + 1))) {
            result -= val;
        } else {
            result += val;
        }
    }
    return result;
}
```

---

## 9.4 Multiply Strings (LC 43)

```java
String multiply(String num1, String num2) {
    int m = num1.length(), n = num2.length();
    int[] pos = new int[m + n]; // result can have at most m+n digits

    // Multiply each digit
    for (int i = m - 1; i >= 0; i--) {
        for (int j = n - 1; j >= 0; j--) {
            int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
            int p1 = i + j, p2 = i + j + 1;
            int sum = mul + pos[p2];

            pos[p2] = sum % 10;
            pos[p1] += sum / 10; // carry
        }
    }

    StringBuilder sb = new StringBuilder();
    for (int p : pos) if (!(sb.length() == 0 && p == 0)) sb.append(p);
    return sb.length() == 0 ? "0" : sb.toString();
}
```

---

## 9.5 Plus One (LC 66)

```java
int[] plusOne(int[] digits) {
    for (int i = digits.length - 1; i >= 0; i--) {
        if (digits[i] < 9) {
            digits[i]++;
            return digits; // no carry, done
        }
        digits[i] = 0; // carry over
    }
    // All 9s case: need extra digit
    int[] result = new int[digits.length + 1];
    result[0] = 1;
    return result;
}
```

### BYTS Problems — Phase 9

| # | Problem | Pattern |
|---|---------|---------|
| 7 | Reverse Integer | Math + overflow |
| 8 | String to Integer (atoi) | Parse with rules |
| 9 | Palindrome Number | Math reverse |
| 13 | Roman to Integer | Map lookup |
| 43 | Multiply Strings | Column multiply |
| 66 | Plus One | Carry propagation |
| 271 | Encode and Decode Strings | Length prefix |

---

# 🔵 Phase 10 — Trie (Prefix Tree)

---

## 10.1 Trie Structure

A Trie stores strings character by character. Each node has:
- 26 children (one per letter)
- `isEnd` flag — marks whether a complete word ends here

```
Insert "cat", "car", "card":

root
 └── c
      └── a
           ├── t (isEnd=true)
           └── r (isEnd=true)
                └── d (isEnd=true)
```

---

## 10.2 Implement Trie (LC 208)

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEnd = false;
}

class Trie {
    TrieNode root = new TrieNode();

    void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null)
                node.children[idx] = new TrieNode();
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    boolean search(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return false;
            node = node.children[idx];
        }
        return node.isEnd; // must be a complete word
    }

    boolean startsWith(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return false;
            node = node.children[idx];
        }
        return true; // just need to reach here
    }
}
```

---

## 10.3 Design Add and Search Words (LC 211)

Supports wildcard '.' which matches any letter. Use DFS for wildcards.

```java
class WordDictionary {
    TrieNode root = new TrieNode();

    void addWord(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null)
                node.children[idx] = new TrieNode();
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    boolean search(String word) {
        return dfs(word, 0, root);
    }

    boolean dfs(String word, int idx, TrieNode node) {
        if (idx == word.length()) return node.isEnd;

        char c = word.charAt(idx);

        if (c == '.') {
            // Try all possible children
            for (TrieNode child : node.children) {
                if (child != null && dfs(word, idx + 1, child)) return true;
            }
            return false;
        } else {
            TrieNode next = node.children[c - 'a'];
            return next != null && dfs(word, idx + 1, next);
        }
    }
}
```

---

## 10.4 Word Search II (LC 212)

Find all words from list in a grid.

```java
// Build Trie from word list, then DFS the grid
// At each cell, traverse Trie simultaneously with grid
// When node.isEnd = true → found a word

List<String> findWords(char[][] board, String[] words) {
    Trie trie = new Trie();
    for (String w : words) trie.insert(w);

    Set<String> result = new HashSet<>();
    int m = board.length, n = board[0].length;

    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            dfs(board, i, j, trie.root, new StringBuilder(), result);

    return new ArrayList<>(result);
}

void dfs(char[][] board, int i, int j,
         TrieNode node, StringBuilder path, Set<String> result) {
    if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return;

    char c = board[i][j];
    if (c == '#') return; // visited
    TrieNode next = node.children[c - 'a'];
    if (next == null) return; // not in trie

    path.append(c);
    if (next.isEnd) result.add(path.toString());

    board[i][j] = '#'; // mark visited
    dfs(board, i-1, j, next, path, result);
    dfs(board, i+1, j, next, path, result);
    dfs(board, i, j-1, next, path, result);
    dfs(board, i, j+1, next, path, result);
    board[i][j] = c;   // restore

    path.deleteCharAt(path.length() - 1);
}
```

### BYTS Problems — Phase 10

| # | Problem | Pattern |
|---|---------|---------|
| 208 | Implement Trie | Trie build |
| 211 | Design Add and Search Words | Trie + wildcard DFS |
| 212 | Word Search II | Trie + grid DFS |

---

# 📋 Master Java Syntax Sheet — Strings

```java
// ── STRING BASICS ─────────────────────────────────────────────
s.length()
s.charAt(i)
s.substring(i, j)              // [i, j)
s.indexOf(c) / s.lastIndexOf(c)
s.contains("sub")
s.equals(t) / s.equalsIgnoreCase(t)
s.compareTo(t)
s.startsWith("x") / s.endsWith("x")
s.trim() / s.strip()
s.toLowerCase() / s.toUpperCase()
s.replace(old, new)
s.split("\\s+")
String.join(", ", parts)

// ── CONVERT ───────────────────────────────────────────────────
char[] arr = s.toCharArray();
String s = new String(arr);
String s = String.valueOf(num);
int n = Integer.parseInt(s);

// ── STRINGBUILDER ─────────────────────────────────────────────
StringBuilder sb = new StringBuilder();
sb.append(c);
sb.append(str);
sb.deleteCharAt(sb.length() - 1); // undo last char
sb.reverse();
sb.toString();
sb.length();
sb.charAt(i);
sb.setLength(n);
sb.insert(i, c);
sb.delete(i, j);

// ── CHARACTER ─────────────────────────────────────────────────
Character.isLetter(c)
Character.isDigit(c)
Character.isLetterOrDigit(c)
Character.isWhitespace(c)
Character.toLowerCase(c)
c - 'a'                        // 0-25 index for lowercase
(char)('a' + i)                // index to lowercase char

// ── FREQUENCY ARRAY ───────────────────────────────────────────
int[] freq = new int[26];
freq[c - 'a']++;
Arrays.equals(freq1, freq2);

// ── PALINDROME EXPAND ─────────────────────────────────────────
int expand(String s, int l, int r) {
    while (l >= 0 && r < s.length()
               && s.charAt(l) == s.charAt(r)) l--; r++;
    return r - l - 1;
}

// ── SLIDING WINDOW ────────────────────────────────────────────
// add right:
window.merge(s.charAt(j), 1, Integer::sum);
// remove left:
window.merge(s.charAt(i), -1, Integer::sum);
if (window.get(c) == 0) window.remove(c);

// ── STACK FOR BRACKETS ────────────────────────────────────────
Deque<Character> stack = new ArrayDeque<>();
stack.push(c);
stack.pop();
stack.peek();
stack.isEmpty();

// ── TRIE ──────────────────────────────────────────────────────
TrieNode[] children = new TrieNode[26];
boolean isEnd = false;
int idx = c - 'a';
if (node.children[idx] == null) node.children[idx] = new TrieNode();
node = node.children[idx];
```

---

# 🧭 Pattern Decision Table

```
SIGNAL IN PROBLEM                    → PATTERN
──────────────────────────────────────────────────────
"Is anagram?"                        → int[26] freq array
"Group by same characters"           → Sort key HashMap
"Find anagram in string"             → Fixed window + freq compare
"Is palindrome?"                     → Two pointer from ends
"Longest palindromic substring"      → Expand from center
"Count palindromic substrings"       → Expand from center
"Longest substring with condition"   → Dynamic sliding window
"Minimum window containing all"      → Sliding window + need/have
"Permutation exists in string?"      → Fixed window + freq
"Reverse words / parts"              → Split + reverse or triple reverse
"Compress string run-length"         → Linear scan + count
"Decode k[string] format"            → Two stacks
"Valid brackets"                     → Stack matching
"Calculator with operators"          → Stack + operator precedence
"Simplify path"                      → Stack with split
"Prefix search / autocomplete"       → Trie
"Search with wildcard ."             → Trie + DFS
"String is rotation of other"        → s+s contains goal
"Repeated pattern in string"         → (s+s)[1..-1] contains s
"Backspace / cancel characters"      → Two pointer from end
"Number from string"                 → Digit by digit + sign + overflow
"Word → number chains"               → Roman numeral map
"Multiply large numbers"             → Column-by-column multiply
```

---

# 📚 All BYTS String Problems by Phase

**Phase 2 (Frequency):** 49, 242, 383, 389, 438, 451, 567

**Phase 3 (Two Pointer):** 125, 151, 344, 345, 392, 541, 680

**Phase 4 (Sliding Window):** 3, 76, 424, 438, 567, 904, 2981

**Phase 5 (Manipulation):** 71, 394, 443, 459, 541, 796

**Phase 6 (Palindrome):** 5, 9, 125, 131, 647, 680

**Phase 7 (Stack):** 20, 32, 71, 150, 155, 224, 227, 394, 735, 921, 1249

**Phase 8 (Search):** 28, 459, 686, 796, 844

**Phase 9 (Number↔String):** 7, 8, 9, 13, 43, 66, 271

**Phase 10 (Trie):** 208, 211, 212

---

*Updated: 2026-06-10 | Java | BYTS Sheet*
*All string patterns: Frequency · Two Pointer · Sliding Window · Palindrome · Stack · Trie · Number Conversion*
