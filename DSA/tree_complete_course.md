# 🌳 Complete Trees Course — SDE Interview Mastery
**Muthu Harish T | BYTS Sheet | Java Edition**

> Covers every tree concept asked in top tech interviews — Amazon, Google, Meta, Microsoft, Flipkart, etc.
> Each phase maps directly to your BYTS LC sheet problems.

---

## 📚 TABLE OF CONTENTS

```
Phase 1 — Tree Foundations & Node Structure
Phase 2 — Tree Traversals (DFS: Pre/In/Post)
Phase 3 — Level Order Traversal (BFS)
Phase 4 — Tree Properties & Measurements
Phase 5 — Binary Search Tree (BST) Operations
Phase 6 — Lowest Common Ancestor (LCA)
Phase 7 — Tree Construction Problems
Phase 8 — Tree Paths & Path Sum Problems
Phase 9 — Tree Views (Right, Left, Top, Bottom)
Phase 10 — Tree DP & Advanced Problems
Phase 11 — Tries (Prefix Trees)
Phase 12 — Segment Trees (Bonus SDE Round)
```

---

## 🔷 PHASE 1 — Tree Foundations & Node Structure

### 1.1 What is a Binary Tree?

A tree where each node has **at most 2 children** (left and right).

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

### 1.2 TreeNode Class — Memorize This

```java
// Standard TreeNode — used in every LC Tree problem
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

### 1.3 Types of Binary Trees

| Type | Property |
|------|----------|
| Full Binary Tree | Every node has 0 or 2 children |
| Complete Binary Tree | All levels full except possibly last (filled left to right) |
| Perfect Binary Tree | All internal nodes have 2 children, all leaves same level |
| Balanced Binary Tree | Height of left and right subtrees differ by at most 1 |
| BST | left < node < right |
| Degenerate (Skewed) | Each node has only one child (linked list shape) |

### 1.4 Key Formulas

```
Height of tree       = number of edges from root to deepest leaf
Depth of node        = number of edges from root to that node
Number of nodes      = up to 2^(h+1) - 1  for perfect binary tree
Leaves in perfect BT = 2^h
```

---

## 🔷 PHASE 2 — Tree Traversals (DFS)

> 🧠 **Pattern: Recursion — think "what to do at this node, then recurse"**

### 2.1 Preorder — Root → Left → Right (NLR)

```java
// Recursive
void preorder(TreeNode root) {
    if (root == null) return;
    System.out.print(root.val + " ");  // PROCESS FIRST
    preorder(root.left);
    preorder(root.right);
}

// Iterative — uses Stack
List<Integer> preorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        result.add(node.val);         // process
        if (node.right != null) stack.push(node.right); // push right FIRST
        if (node.left != null) stack.push(node.left);   // push left second (LIFO)
    }
    return result;
}
```

**Use case:** Copy a tree, serialize a tree, prefix expressions.

---

### 2.2 Inorder — Left → Root → Right (LNR)

```java
// Recursive
void inorder(TreeNode root) {
    if (root == null) return;
    inorder(root.left);
    System.out.print(root.val + " ");  // PROCESS IN MIDDLE
    inorder(root.right);
}

// Iterative — uses Stack
List<Integer> inorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {       // go as left as possible
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        result.add(curr.val);        // process
        curr = curr.right;           // move right
    }
    return result;
}
```

**🔑 KEY INSIGHT:** Inorder of BST = sorted ascending order. This is the #1 BST trick.

---

### 2.3 Postorder — Left → Right → Root (LRN)

```java
// Recursive
void postorder(TreeNode root) {
    if (root == null) return;
    postorder(root.left);
    postorder(root.right);
    System.out.print(root.val + " ");  // PROCESS LAST
}

// Iterative — two stack approach
List<Integer> postorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        result.add(0, node.val);     // add to FRONT (reverse preorder trick)
        if (node.left != null) stack.push(node.left);
        if (node.right != null) stack.push(node.right);
    }
    return result;
}
```

**Use case:** Delete tree, evaluate expression tree, directory size problems.

---

### 2.4 Traversal Summary Quick Reference

```
Tree:       1
           / \
          2   3
         / \
        4   5

Preorder  (Root-Left-Right): 1 2 4 5 3
Inorder   (Left-Root-Right): 4 2 5 1 3
Postorder (Left-Right-Root): 4 5 2 3 1
Level     (BFS):             1 2 3 4 5
```

### 🎯 BYTS Practice Problems — Phase 2

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 144 | Binary Tree Preorder Traversal | Easy | recursive + iterative |
| 94 | Binary Tree Inorder Traversal | Easy | iterative with stack |
| 145 | Binary Tree Postorder Traversal | Easy | reverse preorder trick |
| 589 | N-ary Tree Preorder Traversal | Easy | extend to N children |

---

## 🔷 PHASE 3 — Level Order Traversal (BFS)

> 🧠 **Pattern: Queue — process level by level using queue size trick**

### 3.1 Basic Level Order

```java
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int levelSize = queue.size();      // 🔑 capture size BEFORE processing
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(level);
    }
    return result;
}
```

**🔑 THE TRICK:** `int levelSize = queue.size()` before the inner loop captures how many nodes are on the current level.

---

### 3.2 Zigzag Level Order (LC 103)

```java
List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    boolean leftToRight = true;

    while (!queue.isEmpty()) {
        int size = queue.size();
        LinkedList<Integer> level = new LinkedList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            if (leftToRight) level.addLast(node.val);   // normal
            else level.addFirst(node.val);               // reverse — add to front
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(level);
        leftToRight = !leftToRight;   // flip direction each level
    }
    return result;
}
```

---

### 3.3 Right Side View (LC 199)

```java
// Only take the LAST node of each level
List<Integer> rightSideView(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            if (i == size - 1) result.add(node.val);   // 🔑 last of level
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
    return result;
}
```

---

### 3.4 Find Largest Value in Each Level (LC 515)

```java
List<Integer> largestValues(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            max = Math.max(max, node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(max);
    }
    return result;
}
```

---

### 3.5 Maximum Level Sum (LC 1161)

```java
int maxLevelSum(TreeNode root) {
    int maxSum = Integer.MIN_VALUE;
    int result = 1;
    int level = 1;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        int sum = 0;
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            sum += node.val;
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        if (sum > maxSum) { maxSum = sum; result = level; }
        level++;
    }
    return result;
}
```

### 🎯 BYTS Practice Problems — Phase 3

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 102 | Binary Tree Level Order Traversal | Med | BFS queue + size trick |
| 103 | Binary Tree Zigzag Level Order | Med | LinkedList addFirst/addLast |
| 199 | Binary Tree Right Side View | Med | last node of each level |
| 515 | Find Largest Value in Each Tree Row | Med | max per level |
| 1161 | Maximum Level Sum of Binary Tree | Med | sum per level |

---

## 🔷 PHASE 4 — Tree Properties & Measurements

> 🧠 **Pattern: Return value from recursive calls to compute property**

### 4.1 Maximum Depth (LC 104)

```java
// Height = max depth
int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

### 4.2 Minimum Depth (LC 111)

```java
// 🚨 CAREFUL: min depth is NOT symmetric to max depth!
// Minimum depth = path to NEAREST LEAF (not null node)
int minDepth(TreeNode root) {
    if (root == null) return 0;
    if (root.left == null) return 1 + minDepth(root.right); // must go right
    if (root.right == null) return 1 + minDepth(root.left); // must go left
    return 1 + Math.min(minDepth(root.left), minDepth(root.right));
}
```

---

### 4.3 Balanced Binary Tree (LC 110)

```java
// A tree is balanced if every subtree is balanced AND heights differ by at most 1
// Return -1 to signal "unbalanced"

boolean isBalanced(TreeNode root) {
    return checkHeight(root) != -1;
}

int checkHeight(TreeNode node) {
    if (node == null) return 0;
    int left = checkHeight(node.left);
    if (left == -1) return -1;         // early exit
    int right = checkHeight(node.right);
    if (right == -1) return -1;        // early exit
    if (Math.abs(left - right) > 1) return -1;
    return 1 + Math.max(left, right);
}
```

---

### 4.4 Diameter of Binary Tree (LC 543)

```java
// Diameter = longest path between ANY two nodes (doesn't have to go through root)
// At each node: diameter through that node = leftHeight + rightHeight

int maxDiameter = 0;

int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return maxDiameter;
}

int height(TreeNode node) {
    if (node == null) return 0;
    int left = height(node.left);
    int right = height(node.right);
    maxDiameter = Math.max(maxDiameter, left + right); // 🔑 update global max
    return 1 + Math.max(left, right);
}
```

---

### 4.5 Same Tree (LC 100)

```java
boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;
    if (p == null || q == null) return false;
    if (p.val != q.val) return false;
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

---

### 4.6 Symmetric Tree (LC 101)

```java
// Mirror check: left subtree mirrors right subtree
boolean isSymmetric(TreeNode root) {
    return isMirror(root.left, root.right);
}

boolean isMirror(TreeNode left, TreeNode right) {
    if (left == null && right == null) return true;
    if (left == null || right == null) return false;
    return (left.val == right.val)
        && isMirror(left.left, right.right)   // 🔑 cross-check
        && isMirror(left.right, right.left);
}
```

---

### 4.7 Subtree of Another Tree (LC 572)

```java
boolean isSubtree(TreeNode root, TreeNode subRoot) {
    if (root == null) return false;
    if (isSameTree(root, subRoot)) return true;
    return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
}

boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;
    if (p == null || q == null) return false;
    return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

---

### 4.8 Count Nodes in Complete Binary Tree (LC 222)

```java
// O(log^2 n) — smarter than O(n) full traversal
int countNodes(TreeNode root) {
    if (root == null) return 0;
    int leftH = 0, rightH = 0;
    TreeNode left = root, right = root;
    while (left != null) { leftH++; left = left.left; }
    while (right != null) { rightH++; right = right.right; }
    if (leftH == rightH) return (1 << leftH) - 1; // 🔑 perfect binary tree shortcut
    return 1 + countNodes(root.left) + countNodes(root.right);
}
```

### 🎯 BYTS Practice Problems — Phase 4

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 104 | Maximum Depth of Binary Tree | Easy | 1 + max(left, right) |
| 110 | Balanced Binary Tree | Easy | return -1 as sentinel |
| 543 | Diameter of Binary Tree | Easy | global max at each node |
| 100 | Same Tree | Easy | structural + value check |
| 101 | Symmetric Tree | Easy | mirror check |
| 572 | Subtree of Another Tree | Easy | isSameTree at each node |
| 257 | Binary Tree Paths | Easy | pass path string in recursion |

---

## 🔷 PHASE 5 — Binary Search Tree (BST) Operations

> 🧠 **Pattern: Use BST property (left < node < right) to guide decisions**

### 5.1 BST Search

```java
TreeNode searchBST(TreeNode root, int val) {
    if (root == null || root.val == val) return root;
    if (val < root.val) return searchBST(root.left, val);  // go left
    return searchBST(root.right, val);                     // go right
}
```

---

### 5.2 BST Insert

```java
TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) return new TreeNode(val);   // base: create here
    if (val < root.val) root.left = insertIntoBST(root.left, val);
    else root.right = insertIntoBST(root.right, val);
    return root;   // 🔑 always return root to maintain parent's reference
}
```

---

### 5.3 BST Delete (LC 450)

```java
// 3 cases: no child, one child, two children
TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return null;
    if (key < root.val) root.left = deleteNode(root.left, key);
    else if (key > root.val) root.right = deleteNode(root.right, key);
    else {
        // Found node to delete
        if (root.left == null) return root.right;    // no left child
        if (root.right == null) return root.left;    // no right child
        // Two children: replace with inorder successor (min of right subtree)
        TreeNode minNode = findMin(root.right);
        root.val = minNode.val;                      // copy value
        root.right = deleteNode(root.right, minNode.val); // delete successor
    }
    return root;
}

TreeNode findMin(TreeNode node) {
    while (node.left != null) node = node.left;
    return node;
}
```

---

### 5.4 Validate BST (LC 98)

```java
// 🚨 WRONG approach: just check node.left.val < node.val
// That misses cases where a left subtree node is greater than ancestor

// ✅ CORRECT: pass valid range [min, max] down the tree
boolean isValidBST(TreeNode root) {
    return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

boolean validate(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;
    return validate(node.left, min, node.val)    // left bound becomes node.val
        && validate(node.right, node.val, max);  // right bound becomes node.val
}
```

---

### 5.5 Kth Smallest in BST (LC 230)

```java
// Inorder gives sorted order — just pick kth element
int k, result;

int kthSmallest(TreeNode root, int k) {
    this.k = k;
    inorder(root);
    return result;
}

void inorder(TreeNode node) {
    if (node == null) return;
    inorder(node.left);
    k--;
    if (k == 0) { result = node.val; return; }  // 🔑 kth element
    inorder(node.right);
}
```

---

### 5.6 BST Iterator (LC 173)

```java
class BSTIterator {
    Stack<TreeNode> stack = new Stack<>();

    public BSTIterator(TreeNode root) {
        pushLeft(root);   // push leftmost path
    }

    public int next() {
        TreeNode node = stack.pop();
        pushLeft(node.right);  // push right subtree's leftmost path
        return node.val;
    }

    public boolean hasNext() {
        return !stack.isEmpty();
    }

    private void pushLeft(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
}
```

---

### 5.7 Convert Sorted Array to BST (LC 108)

```java
// Always pick mid as root — guarantees balanced BST
TreeNode sortedArrayToBST(int[] nums) {
    return build(nums, 0, nums.length - 1);
}

TreeNode build(int[] nums, int left, int right) {
    if (left > right) return null;
    int mid = left + (right - left) / 2;
    TreeNode node = new TreeNode(nums[mid]);
    node.left = build(nums, left, mid - 1);
    node.right = build(nums, mid + 1, right);
    return node;
}
```

---

### 5.8 Lowest/Closest Value in BST (LC 270)

```java
int closestValue(TreeNode root, double target) {
    int closest = root.val;
    while (root != null) {
        if (Math.abs(root.val - target) < Math.abs(closest - target))
            closest = root.val;
        root = target < root.val ? root.left : root.right;
    }
    return closest;
}
```

### 🎯 BYTS Practice Problems — Phase 5

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 98 | Validate Binary Search Tree | Med | pass range min/max |
| 230 | Kth Smallest Element in BST | Med | inorder = sorted |
| 173 | Binary Search Tree Iterator | Med | lazy inorder stack |
| 450 | Delete Node in BST | Med | 3-case deletion |
| 108 | Convert Sorted Array to BST | Easy | mid as root |
| 538 | Convert BST to Greater Tree | Med | reverse inorder |

---

## 🔷 PHASE 6 — Lowest Common Ancestor (LCA)

> 🧠 **Pattern: Return the node itself when found, bubble up the common ancestor**

### 6.1 LCA of Binary Tree (LC 236)

```java
// Key insight: if one target is in left and other in right → current node is LCA
TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) return null;
    if (root == p || root == q) return root;   // found one target

    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);

    if (left != null && right != null) return root;  // 🔑 one in each subtree
    return left != null ? left : right;              // found in one side only
}
```

---

### 6.2 LCA of BST (LC 235)

```java
// Simpler: use BST property to navigate
TreeNode lowestCommonAncestorBST(TreeNode root, TreeNode p, TreeNode q) {
    while (root != null) {
        if (p.val < root.val && q.val < root.val)
            root = root.left;   // both in left subtree
        else if (p.val > root.val && q.val > root.val)
            root = root.right;  // both in right subtree
        else
            return root;        // 🔑 split point = LCA
    }
    return null;
}
```

---

### 6.3 LCA with Parent Pointers

```java
// When nodes have parent pointers — use "two pointer" trick like linked list intersection
TreeNode lowestCommonAncestorWithParent(Node p, Node q) {
    Node a = p, b = q;
    while (a != b) {
        a = (a.parent == null) ? q : a.parent;  // jump to other node when reaching root
        b = (b.parent == null) ? p : b.parent;
    }
    return a;
}
```

### 🎯 BYTS Practice Problems — Phase 6

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 236 | LCA of Binary Tree | Med | return when both sides found |
| 235 | LCA of a BST | Med | BST split point |
| 1123 | Lowest Common Ancestor of Deepest Leaves | Med | track depth |

---

## 🔷 PHASE 7 — Tree Construction Problems

> 🧠 **Pattern: Use traversal properties to uniquely rebuild the tree**

### 7.1 Build Tree from Preorder + Inorder (LC 105)

```java
// CONCEPT: preorder[0] = root; find root in inorder → splits into left/right subtrees

Map<Integer, Integer> inorderMap;

TreeNode buildTree(int[] preorder, int[] inorder) {
    inorderMap = new HashMap<>();
    for (int i = 0; i < inorder.length; i++)
        inorderMap.put(inorder[i], i);
    return build(preorder, 0, preorder.length - 1, 0, inorder.length - 1);
}

TreeNode build(int[] pre, int preL, int preR, int inL, int inR) {
    if (preL > preR) return null;

    int rootVal = pre[preL];
    TreeNode root = new TreeNode(rootVal);
    int inRootIdx = inorderMap.get(rootVal);
    int leftSize = inRootIdx - inL;          // 🔑 size of left subtree

    root.left  = build(pre, preL + 1,           preL + leftSize, inL, inRootIdx - 1);
    root.right = build(pre, preL + leftSize + 1, preR,           inRootIdx + 1, inR);
    return root;
}
```

---

### 7.2 Build Tree from Inorder + Postorder (LC 106)

```java
// postorder[last] = root; same split logic as above

Map<Integer, Integer> inorderMap;

TreeNode buildTree(int[] inorder, int[] postorder) {
    inorderMap = new HashMap<>();
    for (int i = 0; i < inorder.length; i++)
        inorderMap.put(inorder[i], i);
    return build(inorder, postorder, 0, inorder.length - 1, 0, postorder.length - 1);
}

TreeNode build(int[] in, int[] post, int inL, int inR, int postL, int postR) {
    if (inL > inR) return null;

    int rootVal = post[postR];   // 🔑 last element is root
    TreeNode root = new TreeNode(rootVal);
    int inRootIdx = inorderMap.get(rootVal);
    int leftSize = inRootIdx - inL;

    root.left  = build(in, post, inL, inRootIdx - 1,  postL, postL + leftSize - 1);
    root.right = build(in, post, inRootIdx + 1, inR,  postL + leftSize, postR - 1);
    return root;
}
```

---

### 7.3 Serialize and Deserialize Binary Tree (LC 297) ⚠️ Hard

```java
// BFS-based serialization
public class Codec {
    // Encode tree to string
    public String serialize(TreeNode root) {
        if (root == null) return "";
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) { sb.append("#,"); continue; }
            sb.append(node.val).append(",");
            queue.offer(node.left);
            queue.offer(node.right);
        }
        return sb.toString();
    }

    // Decode string back to tree
    public TreeNode deserialize(String data) {
        if (data.isEmpty()) return null;
        String[] vals = data.split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int i = 1;
        while (!queue.isEmpty() && i < vals.length) {
            TreeNode node = queue.poll();
            if (!vals[i].equals("#")) {
                node.left = new TreeNode(Integer.parseInt(vals[i]));
                queue.offer(node.left);
            }
            i++;
            if (i < vals.length && !vals[i].equals("#")) {
                node.right = new TreeNode(Integer.parseInt(vals[i]));
                queue.offer(node.right);
            }
            i++;
        }
        return root;
    }
}
```

---

### 7.4 Flatten Binary Tree to Linked List (LC 114)

```java
// In-place: make it a right-linked list in preorder order
// Approach: find inorder predecessor of right child and attach

void flatten(TreeNode root) {
    TreeNode curr = root;
    while (curr != null) {
        if (curr.left != null) {
            TreeNode pred = curr.left;
            while (pred.right != null) pred = pred.right;  // rightmost of left subtree
            pred.right = curr.right;    // attach original right to rightmost of left
            curr.right = curr.left;     // move left to right
            curr.left = null;
        }
        curr = curr.right;
    }
}
```

### 🎯 BYTS Practice Problems — Phase 7

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 105 | Construct from Preorder + Inorder | Med | root = pre[0], split in inorder |
| 106 | Construct from Inorder + Postorder | Med | root = post[last] |
| 114 | Flatten Binary Tree to Linked List | Med | Morris traversal trick |
| 297 | Serialize and Deserialize Binary Tree | Hard ⚠️ | BFS with null markers |
| 95 | Unique Binary Search Trees II | Med | divide and conquer |

---

## 🔷 PHASE 8 — Tree Paths & Path Sum Problems

> 🧠 **Pattern: DFS with path tracking, return boolean or accumulate sum**

### 8.1 Path Sum I (LC 112) — Does path exist?

```java
boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) return false;
    targetSum -= root.val;
    if (root.left == null && root.right == null) return targetSum == 0; // leaf
    return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
}
```

---

### 8.2 Path Sum II (LC 113) — Find all paths

```java
List<List<Integer>> pathSum(TreeNode root, int target) {
    List<List<Integer>> result = new ArrayList<>();
    dfs(root, target, new ArrayList<>(), result);
    return result;
}

void dfs(TreeNode node, int remaining, List<Integer> path, List<List<Integer>> result) {
    if (node == null) return;
    path.add(node.val);
    remaining -= node.val;
    if (node.left == null && node.right == null && remaining == 0)
        result.add(new ArrayList<>(path));   // 🔑 copy the list!
    dfs(node.left, remaining, path, result);
    dfs(node.right, remaining, path, result);
    path.remove(path.size() - 1);           // 🔑 backtrack
}
```

---

### 8.3 Binary Tree Maximum Path Sum (LC 124) ⚠️ Hard

```java
// Path can start and end at ANY node — doesn't need to pass through root
// At each node: maxPath through this node = leftGain + node.val + rightGain

int maxSum = Integer.MIN_VALUE;

int maxPathSum(TreeNode root) {
    gainFrom(root);
    return maxSum;
}

int gainFrom(TreeNode node) {
    if (node == null) return 0;
    int leftGain = Math.max(0, gainFrom(node.left));   // 🔑 ignore negative paths
    int rightGain = Math.max(0, gainFrom(node.right));
    maxSum = Math.max(maxSum, node.val + leftGain + rightGain);  // update global
    return node.val + Math.max(leftGain, rightGain);  // return best single path
}
```

---

### 8.4 Sum Root to Leaf Numbers (LC 129)

```java
// Each root-to-leaf path forms a number: 1->2->3 = 123
int sumNumbers(TreeNode root) {
    return dfs(root, 0);
}

int dfs(TreeNode node, int currentNum) {
    if (node == null) return 0;
    currentNum = currentNum * 10 + node.val;
    if (node.left == null && node.right == null) return currentNum;  // leaf
    return dfs(node.left, currentNum) + dfs(node.right, currentNum);
}
```

---

### 8.5 Path Sum III (LC 437) — Paths summing to target (any node to any node)

```java
// Use prefix sum map — same technique as subarray sum equals k
int pathSum(TreeNode root, int targetSum) {
    Map<Long, Integer> prefixCount = new HashMap<>();
    prefixCount.put(0L, 1);   // base case: empty path
    return dfs(root, 0L, targetSum, prefixCount);
}

int dfs(TreeNode node, long currSum, int target, Map<Long, Integer> map) {
    if (node == null) return 0;
    currSum += node.val;
    int count = map.getOrDefault(currSum - target, 0);  // 🔑 prefix sum trick
    map.put(currSum, map.getOrDefault(currSum, 0) + 1);
    count += dfs(node.left, currSum, target, map) + dfs(node.right, currSum, target, map);
    map.put(currSum, map.get(currSum) - 1);  // 🔑 backtrack
    return count;
}
```

---

### 8.6 Binary Tree Paths (LC 257)

```java
List<String> binaryTreePaths(TreeNode root) {
    List<String> result = new ArrayList<>();
    dfs(root, "", result);
    return result;
}

void dfs(TreeNode node, String path, List<String> result) {
    if (node == null) return;
    path += node.val;
    if (node.left == null && node.right == null) { result.add(path); return; }
    path += "->";
    dfs(node.left, path, result);
    dfs(node.right, path, result);
}
```

### 🎯 BYTS Practice Problems — Phase 8

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 112 | Path Sum | Easy | subtract at each node |
| 113 | Path Sum II | Med | backtracking + copy list |
| 257 | Binary Tree Paths | Easy | pass path string down |
| 129 | Sum Root to Leaf Numbers | Med | currentNum * 10 + val |
| 437 | Path Sum III | Med | prefix sum map |
| 124 | Binary Tree Maximum Path Sum | Hard ⚠️ | global max, ignore negatives |

---

## 🔷 PHASE 9 — Tree Views

### 9.1 Right Side View (LC 199) — Already covered in Phase 3

```java
// BFS: last element of each level
// OR DFS: go right-first, add to result only when level == result.size()

List<Integer> rightSideView(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    dfs(root, 0, result);
    return result;
}

void dfs(TreeNode node, int depth, List<Integer> result) {
    if (node == null) return;
    if (depth == result.size()) result.add(node.val);  // 🔑 first visit at this depth
    dfs(node.right, depth + 1, result);  // go RIGHT first
    dfs(node.left, depth + 1, result);
}
```

---

### 9.2 Left Side View

```java
// Same as right view but go LEFT first
void dfs(TreeNode node, int depth, List<Integer> result) {
    if (node == null) return;
    if (depth == result.size()) result.add(node.val);
    dfs(node.left, depth + 1, result);   // go LEFT first
    dfs(node.right, depth + 1, result);
}
```

---

### 9.3 Top View of Binary Tree

```java
// Use horizontal distance (HD): root=0, left=-1, right=+1
// Top view = first node seen at each HD (in BFS order)

void topView(TreeNode root) {
    if (root == null) return;
    Map<Integer, Integer> hdMap = new TreeMap<>(); // sorted by HD
    Queue<int[]> queue = new LinkedList<>();  // {node_val, hd}
    // Use wrapper or pair; simplified:
    Queue<TreeNode> nodeQ = new LinkedList<>();
    Queue<Integer> hdQ = new LinkedList<>();
    nodeQ.offer(root); hdQ.offer(0);
    while (!nodeQ.isEmpty()) {
        TreeNode node = nodeQ.poll();
        int hd = hdQ.poll();
        if (!hdMap.containsKey(hd)) hdMap.put(hd, node.val);  // first = top
        if (node.left != null) { nodeQ.offer(node.left); hdQ.offer(hd - 1); }
        if (node.right != null) { nodeQ.offer(node.right); hdQ.offer(hd + 1); }
    }
    System.out.println(hdMap.values());
}
```

---

### 9.4 Bottom View of Binary Tree

```java
// Same as top view but OVERWRITE (last node at each HD = bottom view)
void bottomView(TreeNode root) {
    Map<Integer, Integer> hdMap = new TreeMap<>();
    Queue<TreeNode> nodeQ = new LinkedList<>();
    Queue<Integer> hdQ = new LinkedList<>();
    nodeQ.offer(root); hdQ.offer(0);
    while (!nodeQ.isEmpty()) {
        TreeNode node = nodeQ.poll();
        int hd = hdQ.poll();
        hdMap.put(hd, node.val);  // 🔑 always overwrite = bottom view
        if (node.left != null) { nodeQ.offer(node.left); hdQ.offer(hd - 1); }
        if (node.right != null) { nodeQ.offer(node.right); hdQ.offer(hd + 1); }
    }
    System.out.println(hdMap.values());
}
```

---

### 9.5 Vertical Order Traversal (LC 987)

```java
// Nodes with same (col, row) → sort by value
List<List<Integer>> verticalTraversal(TreeNode root) {
    // {col, row, val}
    List<int[]> nodes = new ArrayList<>();
    dfs(root, 0, 0, nodes);
    nodes.sort((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] != b[1] ? a[1] - b[1] : a[2] - b[2]);

    List<List<Integer>> result = new ArrayList<>();
    int prevCol = Integer.MIN_VALUE;
    for (int[] node : nodes) {
        if (node[0] != prevCol) { result.add(new ArrayList<>()); prevCol = node[0]; }
        result.get(result.size() - 1).add(node[2]);
    }
    return result;
}

void dfs(TreeNode node, int row, int col, List<int[]> nodes) {
    if (node == null) return;
    nodes.add(new int[]{col, row, node.val});
    dfs(node.left, row + 1, col - 1, nodes);
    dfs(node.right, row + 1, col + 1, nodes);
}
```

### 🎯 BYTS Practice Problems — Phase 9

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 199 | Binary Tree Right Side View | Med | BFS last or DFS right-first |
| 987 | Vertical Order Traversal | Hard ⚠️ | sort by col, row, val |
| 103 | Zigzag Level Order | Med | alternate left/right |

---

## 🔷 PHASE 10 — Tree DP & Advanced Problems

> 🧠 **Pattern: Compute answer at each node from its children's answers**

### 10.1 House Robber III (LC 337) — Tree DP

```java
// At each node: rob it (can't rob children) OR skip it (children can be robbed)
int rob(TreeNode root) {
    int[] res = dp(root);
    return Math.max(res[0], res[1]);
}

// Returns [skip_root, rob_root]
int[] dp(TreeNode node) {
    if (node == null) return new int[]{0, 0};
    int[] left = dp(node.left);
    int[] right = dp(node.right);
    int skip = Math.max(left[0], left[1]) + Math.max(right[0], right[1]); // skip node
    int rob  = node.val + left[0] + right[0];  // rob node, can't rob children
    return new int[]{skip, rob};
}
```

---

### 10.2 Find Duplicate Subtrees (LC 652)

```java
// Serialize each subtree and use map to detect duplicates
Map<String, Integer> count = new HashMap<>();
List<TreeNode> result = new ArrayList<>();

List<TreeNode> findDuplicateSubtrees(TreeNode root) {
    serialize(root);
    return result;
}

String serialize(TreeNode node) {
    if (node == null) return "#";
    String serial = node.val + "," + serialize(node.left) + "," + serialize(node.right);
    count.put(serial, count.getOrDefault(serial, 0) + 1);
    if (count.get(serial) == 2) result.add(node);  // only add once when count hits 2
    return serial;
}
```

---

### 10.3 All Nodes Distance K in Binary Tree (LC 863)

```java
// Convert tree to undirected graph, then BFS from target
Map<Integer, List<Integer>> graph = new HashMap<>();

List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
    buildGraph(root, null);
    List<Integer> result = new ArrayList<>();
    Set<Integer> visited = new HashSet<>();
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(target.val);
    visited.add(target.val);
    int dist = 0;
    while (!queue.isEmpty()) {
        if (dist == k) { result.addAll(queue); break; }
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int node = queue.poll();
            for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
        dist++;
    }
    return result;
}

void buildGraph(TreeNode node, TreeNode parent) {
    if (node == null) return;
    graph.computeIfAbsent(node.val, k -> new ArrayList<>());
    if (parent != null) {
        graph.get(node.val).add(parent.val);
        graph.get(parent.val).add(node.val);
    }
    buildGraph(node.left, node);
    buildGraph(node.right, node);
}
```

---

### 10.4 Delete Nodes and Return Forest (LC 1110)

```java
List<TreeNode> remainingForest = new ArrayList<>();
Set<Integer> toDelete;

List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
    toDelete = new HashSet<>();
    for (int d : to_delete) toDelete.add(d);
    dfs(root, true);  // root's parent is null, treat as "no parent"
    return remainingForest;
}

TreeNode dfs(TreeNode node, boolean isRoot) {
    if (node == null) return null;
    boolean deleted = toDelete.contains(node.val);
    if (isRoot && !deleted) remainingForest.add(node);  // add root of new tree
    node.left  = dfs(node.left,  deleted);  // if current deleted, left becomes root
    node.right = dfs(node.right, deleted);
    return deleted ? null : node;
}
```

---

### 10.5 Find Leaves of Binary Tree (LC 366)

```java
// Group nodes by their height (distance from deepest leaf is 0 for actual leaves)
List<List<Integer>> findLeaves(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    height(root, result);
    return result;
}

int height(TreeNode node, List<List<Integer>> result) {
    if (node == null) return -1;
    int h = 1 + Math.max(height(node.left, result), height(node.right, result));
    if (result.size() == h) result.add(new ArrayList<>());
    result.get(h).add(node.val);
    return h;
}
```

---

### 10.6 Minimum Height Trees (LC 310)

```java
// Topological sort — keep trimming leaves until 1 or 2 nodes remain
List<Integer> findMinHeightTrees(int n, int[][] edges) {
    if (n == 1) return List.of(0);
    List<Set<Integer>> adj = new ArrayList<>();
    for (int i = 0; i < n; i++) adj.add(new HashSet<>());
    for (int[] e : edges) { adj.get(e[0]).add(e[1]); adj.get(e[1]).add(e[0]); }

    Queue<Integer> leaves = new LinkedList<>();
    for (int i = 0; i < n; i++) if (adj.get(i).size() == 1) leaves.offer(i);

    int remaining = n;
    while (remaining > 2) {
        int size = leaves.size();
        remaining -= size;
        for (int i = 0; i < size; i++) {
            int leaf = leaves.poll();
            int neighbor = adj.get(leaf).iterator().next();
            adj.get(neighbor).remove(leaf);
            if (adj.get(neighbor).size() == 1) leaves.offer(neighbor);
        }
    }
    return new ArrayList<>(leaves);
}
```

### 🎯 BYTS Practice Problems — Phase 10

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 337 | House Robber III | Med | tree DP [skip, rob] array |
| 652 | Find Duplicate Subtrees | Med | serialize + hashmap |
| 863 | All Nodes Distance K | Med | convert to graph + BFS |
| 1110 | Delete Nodes And Return Forest | Med | DFS with isRoot flag |
| 366 | Find Leaves of Binary Tree | Med | height = leaf order |
| 310 | Minimum Height Trees | Med | topological trim leaves |
| 124 | Binary Tree Max Path Sum | Hard ⚠️ | gain function + global max |

---

## 🔷 PHASE 11 — TRIES (Prefix Trees)

> 🧠 **Pattern: 26-children array per node; mark end of word**

### 11.1 TrieNode & Trie Structure

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEnd = false;
}

class Trie {
    TrieNode root = new TrieNode();

    // Insert a word
    void insert(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null)
                curr.children[idx] = new TrieNode();
            curr = curr.children[idx];
        }
        curr.isEnd = true;
    }

    // Search exact word
    boolean search(String word) {
        TrieNode node = getNode(word);
        return node != null && node.isEnd;
    }

    // Check if any word starts with prefix
    boolean startsWith(String prefix) {
        return getNode(prefix) != null;
    }

    private TrieNode getNode(String str) {
        TrieNode curr = root;
        for (char c : str.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) return null;
            curr = curr.children[idx];
        }
        return curr;
    }
}
```

---

### 11.2 Design Add and Search Words (LC 211) — with wildcard '.'

```java
class WordDictionary {
    TrieNode root = new TrieNode();

    void addWord(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) curr.children[idx] = new TrieNode();
            curr = curr.children[idx];
        }
        curr.isEnd = true;
    }

    boolean search(String word) {
        return dfs(word, 0, root);
    }

    boolean dfs(String word, int i, TrieNode node) {
        if (i == word.length()) return node.isEnd;
        char c = word.charAt(i);
        if (c == '.') {
            // try all children for wildcard
            for (TrieNode child : node.children)
                if (child != null && dfs(word, i + 1, child)) return true;
            return false;
        }
        TrieNode next = node.children[c - 'a'];
        return next != null && dfs(word, i + 1, next);
    }
}
```

---

### 11.3 Word Search II (LC 212) ⚠️ Hard

```java
// Combine DFS on board + Trie to efficiently find all words
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

void dfs(char[][] board, int i, int j, TrieNode node, StringBuilder path, Set<String> result) {
    if (i < 0 || j < 0 || i >= board.length || j >= board[0].length) return;
    char c = board[i][j];
    if (c == '#' || node.children[c - 'a'] == null) return;

    path.append(c);
    TrieNode next = node.children[c - 'a'];
    if (next.isEnd) result.add(path.toString());

    board[i][j] = '#';  // mark visited
    dfs(board, i+1, j, next, path, result);
    dfs(board, i-1, j, next, path, result);
    dfs(board, i, j+1, next, path, result);
    dfs(board, i, j-1, next, path, result);
    board[i][j] = c;    // restore
    path.deleteCharAt(path.length() - 1);
}
```

### 🎯 BYTS Practice Problems — Phase 11

| LC # | Problem | Difficulty | Key Idea |
|------|---------|------------|----------|
| 208 | Implement Trie | Med | children[26] + isEnd |
| 211 | Design Add and Search Words | Med | wildcard DFS on trie |
| 212 | Word Search II | Hard ⚠️ | board DFS + Trie pruning |
| 648 | Replace Words | Med | find shortest prefix in trie |
| 677 | Map Sum Pairs | Med | trie with values |

---

## 🔷 PHASE 12 — Segment Trees (Bonus — SDE Advanced Rounds)

### 12.1 Segment Tree — Range Sum Query

```java
class SegmentTree {
    int[] tree;
    int n;

    SegmentTree(int[] nums) {
        n = nums.length;
        tree = new int[4 * n];
        build(nums, 0, 0, n - 1);
    }

    void build(int[] nums, int node, int start, int end) {
        if (start == end) { tree[node] = nums[start]; return; }
        int mid = (start + end) / 2;
        build(nums, 2*node+1, start, mid);
        build(nums, 2*node+2, mid+1, end);
        tree[node] = tree[2*node+1] + tree[2*node+2];
    }

    void update(int node, int start, int end, int idx, int val) {
        if (start == end) { tree[node] = val; return; }
        int mid = (start + end) / 2;
        if (idx <= mid) update(2*node+1, start, mid, idx, val);
        else update(2*node+2, mid+1, end, idx, val);
        tree[node] = tree[2*node+1] + tree[2*node+2];
    }

    int query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return 0;         // out of range
        if (l <= start && end <= r) return tree[node]; // fully in range
        int mid = (start + end) / 2;
        return query(2*node+1, start, mid, l, r)
             + query(2*node+2, mid+1, end, l, r);
    }
}
```

**Related LC:** 307 (Range Sum Query Mutable)

---

## 🔷 MASTER CHEAT SHEET — Patterns Quick Reference

### When to use what:

```
Problem asks...                    → Use this pattern
─────────────────────────────────────────────────────
Level-by-level processing         → BFS with queue + size trick
Path from root to leaf             → DFS with backtracking
Max/Min across all paths           → DFS returning value + global var
Height / depth of subtree          → Recursive return int
LCA of two nodes                   → DFS: return when found, merge
Build tree from traversals         → Find root, split inorder
Serialize/Deserialize              → BFS with null markers
Check if balanced                  → Return -1 as sentinel
BST property (sorted, kth, etc.)   → Inorder traversal
Search/insert in BST               → Navigate left/right by comparison
Paths summing to k                 → Prefix sum + HashMap
Find duplicate patterns            → Serialize subtree + HashMap
Views (right, left, top, bottom)   → BFS last/first per level, or HD
Word prefix problems               → Trie
```

---

## 🔷 INTERVIEW TIPS — Mistakes to Avoid

```java
// ❌ MISTAKE 1: Forgetting null check at start
int maxDepth(TreeNode root) {
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right)); // NPE!
}
// ✅ FIX: Always check null first
int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}

// ❌ MISTAKE 2: Mutating list without copying in path problems
result.add(path);      // adds reference, list changes later!
// ✅ FIX:
result.add(new ArrayList<>(path));  // copy!

// ❌ MISTAKE 3: Forgetting to backtrack in DFS
path.add(node.val);
dfs(node.left, path);
dfs(node.right, path);
// ✅ FIX: Remove after recursion
path.add(node.val);
dfs(node.left, path);
dfs(node.right, path);
path.remove(path.size() - 1);  // backtrack!

// ❌ MISTAKE 4: BST validation — only checking immediate parent
if (node.left.val < node.val) ...  // wrong! doesn't check ancestors
// ✅ FIX: Pass min/max range
validate(node, Long.MIN_VALUE, Long.MAX_VALUE)

// ❌ MISTAKE 5: Diameter — returning leftH + rightH up the call stack
// The diameter is computed AT the node, not returned
// ✅ FIX: Use global variable, return only height upward
```

---

## 🔷 TIME & SPACE COMPLEXITY SUMMARY

| Operation | Time | Space |
|-----------|------|-------|
| DFS traversal | O(n) | O(h) stack |
| BFS traversal | O(n) | O(w) queue (w = max width) |
| BST search | O(h) | O(1) |
| BST insert/delete | O(h) | O(h) |
| Balanced BST h | O(log n) | — |
| Skewed BST h | O(n) | — |
| Build from traversals | O(n) | O(n) |
| Trie insert | O(L) | O(L) |
| Trie search | O(L) | O(1) |
| Path Sum III | O(n) | O(n) |
| LCA | O(n) | O(h) |

---

## 🔷 YOUR BYTS SHEET — ALL TREE PROBLEMS MAPPED

### Problems Not Done Yet (From Your Sheet)

| LC # | Problem | Phase |
|------|---------|-------|
| 114 | Flatten Binary Tree to Linked List | Phase 7 |
| 236 | LCA of Binary Tree | Phase 6 |
| 241 | Different Ways to Add Parentheses | Phase 7 |
| 310 | Minimum Height Trees | Phase 10 |
| 366 | Find Leaves of Binary Tree | Phase 10 |
| 652 | Find Duplicate Subtrees | Phase 10 |
| 863 | All Nodes Distance K | Phase 10 |
| 988 | Smallest String Starting from Leaf | Phase 8 |
| 95 | Unique Binary Search Trees II | Phase 7 |
| 96 | Unique Binary Search Trees | Phase 5 |
| 103 | Binary Tree Zigzag Level Order | Phase 3 |

### Problems Already Done (Revise These)

| LC # | Problem | Phase |
|------|---------|-------|
| 226 | Invert Binary Tree | Phase 4 |
| 104 | Maximum Depth | Phase 4 |
| 100 | Same Tree | Phase 4 |
| 572 | Subtree of Another Tree | Phase 4 |
| 110 | Balanced Binary Tree | Phase 4 |
| 102 | Level Order Traversal | Phase 3 |
| 199 | Right Side View | Phase 9 |
| 94 | Inorder Traversal | Phase 2 |
| 98 | Validate BST | Phase 5 |
| 230 | Kth Smallest in BST | Phase 5 |
| 235 | LCA of BST | Phase 6 |
| 105 | Construct from Pre+Inorder | Phase 7 |
| 101 | Symmetric Tree | Phase 4 |
| 257 | Binary Tree Paths | Phase 8 |
| 543 | Diameter | Phase 4 |
| 145 | Postorder Traversal | Phase 2 |
| 297 | Serialize and Deserialize | Phase 7 |
| 124 | Max Path Sum | Phase 8 |

---

*Last updated: 2026-06-12 | Muthu Harish T | BYTS Sheet Tree Module*
*Complete course — 12 phases, all SDE tree patterns covered*
