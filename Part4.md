
# Post #4 — Recursion on Trees (Java Modular Classes)

## Problems we’ll cover

1. **Maximum Depth of Binary Tree** — `MaxDepthBinaryTree.java` + `MaxDepthBinaryTreeTest.java` (LC 104)
2. **Diameter of Binary Tree** — `DiameterBinaryTree.java` + `DiameterBinaryTreeTest.java` (LC 543)
3. **Invert Binary Tree** — `InvertBinaryTree.java` + `InvertBinaryTreeTest.java` (LC 226)
4. **Path Sum I** — `PathSum.java` + `PathSumTest.java` (LC 112)
5. **All Paths from Root to Leaf** — `BinaryTreePaths.java` + `BinaryTreePathsTest.java` (LC 257)

We’ll define a **common TreeNode class** for all:

```java
// TreeNode.java
public class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```

---

## 1️⃣ Maximum Depth of Binary Tree (LC 104)

```java
// MaxDepthBinaryTree.java
public class MaxDepthBinaryTree {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return Math.max(left, right) + 1;
    }
}
```

```java
// MaxDepthBinaryTreeTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MaxDepthBinaryTreeTest {
    @Test
    public void testMaxDepth() {
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(9);
        root.right = new TreeNode(20);
        root.right.left = new TreeNode(15);
        root.right.right = new TreeNode(7);

        MaxDepthBinaryTree solver = new MaxDepthBinaryTree();
        assertEquals(3, solver.maxDepth(root));
    }
}
```

---

## 2️⃣ Diameter of Binary Tree (LC 543)

```java
// DiameterBinaryTree.java
public class DiameterBinaryTree {
    private int diameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        depth(root);
        return diameter;
    }

    private int depth(TreeNode node) {
        if (node == null) return 0;
        int left = depth(node.left);
        int right = depth(node.right);
        diameter = Math.max(diameter, left + right);
        return Math.max(left, right) + 1;
    }
}
```

```java
// DiameterBinaryTreeTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class DiameterBinaryTreeTest {
    @Test
    public void testDiameter() {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);

        DiameterBinaryTree solver = new DiameterBinaryTree();
        assertEquals(3, solver.diameterOfBinaryTree(root));
    }
}
```

---

## 3️⃣ Invert Binary Tree (LC 226)

```java
// InvertBinaryTree.java
public class InvertBinaryTree {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode tmp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(tmp);
        return root;
    }
}
```

```java
// InvertBinaryTreeTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class InvertBinaryTreeTest {
    @Test
    public void testInvert() {
        TreeNode root = new TreeNode(4);
        root.left = new TreeNode(2);
        root.right = new TreeNode(7);
        root.left.left = new TreeNode(1);
        root.left.right = new TreeNode(3);
        root.right.left = new TreeNode(6);
        root.right.right = new TreeNode(9);

        InvertBinaryTree solver = new InvertBinaryTree();
        TreeNode inverted = solver.invertTree(root);
        assertEquals(7, inverted.left.val);
        assertEquals(2, inverted.right.val);
    }
}
```

---

## 4️⃣ Path Sum I (LC 112)

```java
// PathSum.java
public class PathSum {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        if (root.left == null && root.right == null) return root.val == targetSum;
        return hasPathSum(root.left, targetSum - root.val) ||
               hasPathSum(root.right, targetSum - root.val);
    }
}
```

```java
// PathSumTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class PathSumTest {
    @Test
    public void testHasPathSum() {
        TreeNode root = new TreeNode(5);
        root.left = new TreeNode(4);
        root.right = new TreeNode(8);
        root.left.left = new TreeNode(11);
        root.right.left = new TreeNode(13);
        root.right.right = new TreeNode(4);
        root.left.left.left = new TreeNode(7);
        root.left.left.right = new TreeNode(2);
        root.right.right.right = new TreeNode(1);

        PathSum solver = new PathSum();
        assertTrue(solver.hasPathSum(root, 22));
        assertFalse(solver.hasPathSum(root, 27));
    }
}
```

---

## 5️⃣ All Paths from Root to Leaf (LC 257)

```java
// BinaryTreePaths.java
import java.util.*;

public class BinaryTreePaths {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        if (root != null) dfs(root, "", ans);
        return ans;
    }

    private void dfs(TreeNode node, String path, List<String> ans) {
        if (node.left == null && node.right == null) {
            ans.add(path + node.val);
            return;
        }
        if (node.left != null) dfs(node.left, path + node.val + "->", ans);
        if (node.right != null) dfs(node.right, path + node.val + "->", ans);
    }
}
```

```java
// BinaryTreePathsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class BinaryTreePathsTest {
    @Test
    public void testBinaryTreePaths() {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.right = new TreeNode(5);

        BinaryTreePaths solver = new BinaryTreePaths();
        List<String> result = solver.binaryTreePaths(root);
        assertTrue(result.contains("1->2->5"));
        assertTrue(result.contains("1->3"));
    }
}
```

---

## ✅ Engineering / Practice Notes

* Most tree problems follow **DFS pattern**: return something + combine left/right results.
* **Base case**: `null` node (or leaf check).
* **Path tracking**: use StringBuilder or List, backtrack if mutable.
* Time complexity usually **O(n)**, space **O(h)** (h = height, recursion stack).
* Modular classes + tests make it easier to extend to **Binary Search Tree** variants.

---

Next post (Post #5) will cover **Recursion on Linked Lists**: reversing, merging, deleting nodes recursively, comparing iterative vs recursive approaches.

