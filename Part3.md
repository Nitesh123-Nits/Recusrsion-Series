
# Post #3 — Backtracking with Constraints (Java Modular Classes)

## Problems we’ll cover

1. **Combination Sum I** — `CombinationSum.java` + `CombinationSumTest.java` (LC 39)
2. **Combination Sum II** — `CombinationSum2.java` + `CombinationSum2Test.java` (LC 40)
3. **Word Search** — `WordSearch.java` + `WordSearchTest.java` (LC 79)
4. **Permutations II (duplicates)** — `Permutations2.java` + `Permutations2Test.java` (LC 47)
5. **Palindrome Partitioning** — `PalindromePartitioning.java` + `PalindromePartitioningTest.java` (LC 131)

---

## 1️⃣ Combination Sum I (LC 39)

```java
// CombinationSum.java
import java.util.*;

public class CombinationSum {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(candidates); // optional but can help pruning
        backtrack(candidates, target, 0, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int[] candidates, int remain, int idx, List<Integer> path, List<List<Integer>> ans) {
        if (remain == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = idx; i < candidates.length; i++) {
            if (candidates[i] > remain) break; // prune
            path.add(candidates[i]);
            backtrack(candidates, remain - candidates[i], i, path, ans); // can reuse
            path.remove(path.size() - 1);
        }
    }
}
```

```java
// CombinationSumTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class CombinationSumTest {
    @Test
    public void testCombinationSum() {
        CombinationSum cs = new CombinationSum();
        int[] candidates = {2,3,6,7};
        List<List<Integer>> result = cs.combinationSum(candidates, 7);
        assertEquals(2, result.size());
        assertTrue(result.contains(Arrays.asList(7)));
        assertTrue(result.contains(Arrays.asList(2,2,3)));
    }
}
```

---

## 2️⃣ Combination Sum II (LC 40) — no duplicates in result

```java
// CombinationSum2.java
import java.util.*;

public class CombinationSum2 {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(candidates);
        backtrack(candidates, target, 0, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int[] candidates, int remain, int idx, List<Integer> path, List<List<Integer>> ans) {
        if (remain == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = idx; i < candidates.length; i++) {
            if (i > idx && candidates[i] == candidates[i-1]) continue; // skip duplicates
            if (candidates[i] > remain) break;
            path.add(candidates[i]);
            backtrack(candidates, remain - candidates[i], i + 1, path, ans); // cannot reuse
            path.remove(path.size() - 1);
        }
    }
}
```

```java
// CombinationSum2Test.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class CombinationSum2Test {
    @Test
    public void testCombinationSum2() {
        CombinationSum2 cs2 = new CombinationSum2();
        int[] candidates = {10,1,2,7,6,1,5};
        List<List<Integer>> result = cs2.combinationSum2(candidates, 8);
        assertTrue(result.contains(Arrays.asList(1,1,6)));
        assertTrue(result.contains(Arrays.asList(1,2,5)));
    }
}
```

---

## 3️⃣ Word Search (LC 79)

```java
// WordSearch.java
public class WordSearch {
    private int m, n;

    public boolean exist(char[][] board, String word) {
        m = board.length;
        n = board[0].length;
        boolean[][] seen = new boolean[m][n];
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (dfs(board, word, 0, i, j, seen)) return true;
        return false;
    }

    private boolean dfs(char[][] b, String w, int idx, int r, int c, boolean[][] seen) {
        if (idx == w.length()) return true;
        if (r < 0 || c < 0 || r >= m || c >= n || seen[r][c] || b[r][c] != w.charAt(idx))
            return false;
        seen[r][c] = true;
        boolean found = dfs(b, w, idx+1, r+1,c,seen) || dfs(b,w,idx+1,r-1,c,seen)
                     || dfs(b,w,idx+1,r,c+1,seen) || dfs(b,w,idx+1,r,c-1,seen);
        seen[r][c] = false;
        return found;
    }
}
```

```java
// WordSearchTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class WordSearchTest {
    @Test
    public void testExist() {
        WordSearch ws = new WordSearch();
        char[][] board = {{'A','B','C','E'},
                          {'S','F','C','S'},
                          {'A','D','E','E'}};
        assertTrue(ws.exist(board, "ABCCED"));
        assertFalse(ws.exist(board, "ABCB"));
    }
}
```

---

## 4️⃣ Permutations II (LC 47) — duplicates in input

```java
// Permutations2.java
import java.util.*;

public class Permutations2 {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        boolean[] used = new boolean[nums.length];
        backtrack(nums, new ArrayList<>(), used, ans);
        return ans;
    }

    private void backtrack(int[] nums, List<Integer> path, boolean[] used, List<List<Integer>> ans) {
        if (path.size() == nums.length) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i] || (i>0 && nums[i] == nums[i-1] && !used[i-1])) continue;
            used[i] = true;
            path.add(nums[i]);
            backtrack(nums, path, used, ans);
            path.remove(path.size()-1);
            used[i] = false;
        }
    }
}
```

```java
// Permutations2Test.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class Permutations2Test {
    @Test
    public void testPermuteUnique() {
        Permutations2 p2 = new Permutations2();
        int[] nums = {1,1,2};
        List<List<Integer>> result = p2.permuteUnique(nums);
        assertEquals(3, result.size());
        assertTrue(result.contains(Arrays.asList(1,1,2)));
        assertTrue(result.contains(Arrays.asList(1,2,1)));
    }
}
```

---

## 5️⃣ Palindrome Partitioning (LC 131)

```java
// PalindromePartitioning.java
import java.util.*;

public class PalindromePartitioning {
    public List<List<String>> partition(String s) {
        List<List<String>> ans = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(String s, int idx, List<String> path, List<List<String>> ans) {
        if (idx == s.length()) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = idx; i < s.length(); i++) {
            if (isPalindrome(s, idx, i)) {
                path.add(s.substring(idx, i+1));
                backtrack(s, i+1, path, ans);
                path.remove(path.size()-1);
            }
        }
    }

    private boolean isPalindrome(String s, int l, int r) {
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }
}
```

```java
// PalindromePartitioningTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class PalindromePartitioningTest {
    @Test
    public void testPartition() {
        PalindromePartitioning pp = new PalindromePartitioning();
        List<List<String>> result = pp.partition("aab");
        assertTrue(result.contains(Arrays.asList("a","a","b")));
        assertTrue(result.contains(Arrays.asList("aa","b")));
    }
}
```

---

✅ **Engineering / Practice Notes**

* **Pruning** is key: sort + skip unnecessary branches (Combination Sum, Permutations II).
* **Duplicate handling**: track usage or skip repeated candidates.
* **DFS/backtracking on grids**: always use visited matrix or mark/unmark cells.
* **Palindrome partitioning**: precompute palindrome table if string is long for performance.

---

Next post (Post #4) will move into **Recursion on Trees**: preorder/inorder/postorder, max depth, path sums, diameter, invert tree — again in **modular classes + JUnit tests** with complexity notes.

Do you want me to draft **Post #4 — Trees Recursion** now?
