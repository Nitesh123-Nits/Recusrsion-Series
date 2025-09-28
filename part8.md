
# Post #8 — Advanced Recursion + DP / Backtracking (Java Modular Classes)

## Problems we’ll cover

1. **Combination Sum III** — `CombinationSum3.java` + `CombinationSum3Test.java` (LC 216)
2. **Palindrome Partitioning II** — `PalindromePartitioningII.java` + `PalindromePartitioningIITest.java` (LC 132)
3. **Word Break II** — `WordBreakII.java` + `WordBreakIITest.java` (LC 140)
4. **Minimum Path Sum** — `MinPathSum.java` + `MinPathSumTest.java` (LC 64)
5. **Target Sum** — `TargetSum.java` + `TargetSumTest.java` (LC 494)

---

## 1️⃣ Combination Sum III (LC 216)

```java
// CombinationSum3.java
import java.util.*;

public class CombinationSum3 {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(k, n, 1, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int k, int remain, int start, List<Integer> path, List<List<Integer>> ans) {
        if (path.size() == k && remain == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i <= 9; i++) {
            if (remain < i) break;
            path.add(i);
            backtrack(k, remain - i, i + 1, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```

```java
// CombinationSum3Test.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class CombinationSum3Test {
    @Test
    public void testCombinationSum3() {
        CombinationSum3 solver = new CombinationSum3();
        List<List<Integer>> result = solver.combinationSum3(3, 7);
        assertTrue(result.contains(Arrays.asList(1,2,4)));
    }
}
```

---

## 2️⃣ Palindrome Partitioning II (LC 132)

```java
// PalindromePartitioningII.java
import java.util.*;

public class PalindromePartitioningII {
    private int[] memo;

    public int minCut(String s) {
        int n = s.length();
        memo = new int[n];
        Arrays.fill(memo, -1);
        return dfs(s, 0);
    }

    private int dfs(String s, int start) {
        if (start == s.length()) return -1; // no cut needed
        if (memo[start] != -1) return memo[start];
        int minCuts = Integer.MAX_VALUE;
        for (int end = start; end < s.length(); end++) {
            if (isPalindrome(s, start, end)) {
                minCuts = Math.min(minCuts, 1 + dfs(s, end + 1));
            }
        }
        memo[start] = minCuts;
        return minCuts;
    }

    private boolean isPalindrome(String s, int l, int r) {
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }
}
```

```java
// PalindromePartitioningIITest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class PalindromePartitioningIITest {
    @Test
    public void testMinCut() {
        PalindromePartitioningII solver = new PalindromePartitioningII();
        assertEquals(1, solver.minCut("aab"));
    }
}
```

---

## 3️⃣ Word Break II (LC 140)

```java
// WordBreakII.java
import java.util.*;

public class WordBreakII {
    private Map<String, List<String>> memo = new HashMap<>();

    public List<String> wordBreak(String s, List<String> wordDict) {
        return dfs(s, new HashSet<>(wordDict));
    }

    private List<String> dfs(String s, Set<String> wordDict) {
        if (memo.containsKey(s)) return memo.get(s);
        List<String> res = new ArrayList<>();
        if (s.isEmpty()) { res.add(""); return res; }
        for (int i = 1; i <= s.length(); i++) {
            String prefix = s.substring(0, i);
            if (wordDict.contains(prefix)) {
                List<String> subList = dfs(s.substring(i), wordDict);
                for (String sub : subList) {
                    res.add(prefix + (sub.isEmpty() ? "" : " ") + sub);
                }
            }
        }
        memo.put(s, res);
        return res;
    }
}
```

```java
// WordBreakIITest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class WordBreakIITest {
    @Test
    public void testWordBreak() {
        WordBreakII solver = new WordBreakII();
        List<String> dict = Arrays.asList("cat","cats","and","sand","dog");
        List<String> result = solver.wordBreak("catsanddog", dict);
        assertTrue(result.contains("cats and dog"));
        assertTrue(result.contains("cat sand dog"));
    }
}
```

---

## 4️⃣ Minimum Path Sum (LC 64)

```java
// MinPathSum.java
import java.util.*;

public class MinPathSum {
    private int[][] memo;

    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        memo = new int[m][n];
        for (int[] row : memo) Arrays.fill(row, -1);
        return dfs(grid, 0, 0);
    }

    private int dfs(int[][] grid, int r, int c) {
        if (r >= grid.length || c >= grid[0].length) return Integer.MAX_VALUE;
        if (r == grid.length - 1 && c == grid[0].length - 1) return grid[r][c];
        if (memo[r][c] != -1) return memo[r][c];
        memo[r][c] = grid[r][c] + Math.min(dfs(grid, r + 1, c), dfs(grid, r, c + 1));
        return memo[r][c];
    }
}
```

```java
// MinPathSumTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MinPathSumTest {
    @Test
    public void testMinPathSum() {
        MinPathSum solver = new MinPathSum();
        int[][] grid = {{1,3,1},{1,5,1},{4,2,1}};
        assertEquals(7, solver.minPathSum(grid));
    }
}
```

---

## 5️⃣ Target Sum (LC 494)

```java
// TargetSum.java
import java.util.*;

public class TargetSum {
    private Map<String, Integer> memo = new HashMap<>();

    public int findTargetSumWays(int[] nums, int target) {
        return dfs(nums, 0, target);
    }

    private int dfs(int[] nums, int index, int remaining) {
        if (index == nums.length) return remaining == 0 ? 1 : 0;
        String key = index + "," + remaining;
        if (memo.containsKey(key)) return memo.get(key);
        int ways = dfs(nums, index + 1, remaining - nums[index]) +
                   dfs(nums, index + 1, remaining + nums[index]);
        memo.put(key, ways);
        return ways;
    }
}
```

```java
// TargetSumTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class TargetSumTest {
    @Test
    public void testFindTargetSumWays() {
        TargetSum solver = new TargetSum();
        int[] nums = {1,1,1,1,1};
        assertEquals(5, solver.findTargetSumWays(nums, 3));
    }
}
```

---

## ✅ Engineering / Practice Notes

* Memoization is essential for **combinatorial explosion** in backtracking problems.
* Key patterns:

  * Track **state with index + remaining / path**.
  * Use **HashMap / array** for caching.
  * Backtracking + memoization often converts exponential recursion into **polynomial time**.
* Common interview patterns: subset, partition, sequence reconstruction, min/max path.

---

Next post (Post #9) will cover **Recursion on Graphs**: DFS, cycle detection, connected components, topological sort, and backtracking on grids — again with **modular classes + JUnit tests + LeetCode references**.


