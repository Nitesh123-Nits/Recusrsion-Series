
# Post #11 — Recursion with Bitmasking / Subset DP (Java Modular Classes)

## Problems we’ll cover

1. **Traveling Salesman Problem (TSP) Variant** — `TSP.java` + `TSPTest.java`
2. **Assignment Problem / Maximum Compatibility Sum** — `MaxCompatibility.java` + `MaxCompatibilityTest.java` (LC 1125 variant)
3. **Partition to K Equal Sum Subsets** — `KPartition.java` + `KPartitionTest.java` (LC 698)
4. **Minimum Subset Sum Difference** — `MinSubsetSumDiff.java` + `MinSubsetSumDiffTest.java`
5. **Word Squares / Advanced Backtracking with State** — `WordSquares.java` + `WordSquaresTest.java` (LC 425)

---

## 1️⃣ Traveling Salesman Problem (TSP) Variant

```java
// TSP.java
public class TSP {
    private int[][] dist;
    private int[][] memo;

    public int tsp(int[][] dist) {
        int n = dist.length;
        this.dist = dist;
        memo = new int[n][1 << n];
        for (int[] row : memo) java.util.Arrays.fill(row, -1);
        return dfs(0, 1);
    }

    private int dfs(int pos, int visited) {
        if (visited == (1 << dist.length) - 1) return dist[pos][0]; // back to start
        if (memo[pos][visited] != -1) return memo[pos][visited];
        int minDist = Integer.MAX_VALUE;
        for (int i = 0; i < dist.length; i++) {
            if ((visited & (1 << i)) == 0)
                minDist = Math.min(minDist, dist[pos][i] + dfs(i, visited | (1 << i)));
        }
        return memo[pos][visited] = minDist;
    }
}
```

```java
// TSPTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class TSPTest {
    @Test
    public void testTSP() {
        TSP solver = new TSP();
        int[][] dist = {{0,10,15,20},{10,0,35,25},{15,35,0,30},{20,25,30,0}};
        assertEquals(80, solver.tsp(dist));
    }
}
```

---

## 2️⃣ Assignment Problem / Maximum Compatibility Sum

```java
// MaxCompatibility.java
public class MaxCompatibility {
    private int[][] memo;

    public int maxCompatibility(int[][] students, int[][] mentors) {
        int n = students.length;
        memo = new int[1 << n][n];
        for (int[] row : memo) java.util.Arrays.fill(row, -1);
        return dfs(students, mentors, 0, 0);
    }

    private int dfs(int[][] s, int[][] m, int idx, int mask) {
        if (idx == s.length) return 0;
        if (memo[mask][idx] != -1) return memo[mask][idx];
        int maxScore = 0;
        for (int j = 0; j < m.length; j++) {
            if ((mask & (1 << j)) == 0) {
                int score = 0;
                for (int k = 0; k < s[0].length; k++) if (s[idx][k] == m[j][k]) score++;
                maxScore = Math.max(maxScore, score + dfs(s, m, idx + 1, mask | (1 << j)));
            }
        }
        return memo[mask][idx] = maxScore;
    }
}
```

```java
// MaxCompatibilityTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MaxCompatibilityTest {
    @Test
    public void testMaxCompatibility() {
        MaxCompatibility solver = new MaxCompatibility();
        int[][] students = {{1,1,0},{1,0,1},{0,0,1}};
        int[][] mentors = {{1,0,0},{0,0,1},{1,1,0}};
        assertEquals(8, solver.maxCompatibility(students, mentors));
    }
}
```

---

## 3️⃣ Partition to K Equal Sum Subsets (LC 698)

```java
// KPartition.java
public class KPartition {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % k != 0) return false;
        int target = sum / k;
        boolean[] visited = new boolean[nums.length];
        return backtrack(nums, visited, 0, 0, k, target);
    }

    private boolean backtrack(int[] nums, boolean[] visited, int start, int currSum, int k, int target) {
        if (k == 0) return true;
        if (currSum == target) return backtrack(nums, visited, 0, 0, k - 1, target);
        for (int i = start; i < nums.length; i++) {
            if (!visited[i] && currSum + nums[i] <= target) {
                visited[i] = true;
                if (backtrack(nums, visited, i + 1, currSum + nums[i], k, target)) return true;
                visited[i] = false;
            }
        }
        return false;
    }
}
```

```java
// KPartitionTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class KPartitionTest {
    @Test
    public void testCanPartitionKSubsets() {
        KPartition solver = new KPartition();
        int[] nums = {4,3,2,3,5,2,1};
        assertTrue(solver.canPartitionKSubsets(nums, 4));
    }
}
```

---

## 4️⃣ Minimum Subset Sum Difference

```java
// MinSubsetSumDiff.java
import java.util.*;

public class MinSubsetSumDiff {
    public int minDifference(int[] nums) {
        int sum = 0;
        for (int n : nums) sum += n;
        int n = nums.length;
        boolean[][] dp = new boolean[n+1][sum/2+1];
        for (int i = 0; i <= n; i++) dp[i][0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum/2; j++) {
                dp[i][j] = dp[i-1][j];
                if (j >= nums[i-1]) dp[i][j] |= dp[i-1][j-nums[i-1]];
            }
        }
        for (int j = sum/2; j >= 0; j--) if (dp[n][j]) return sum - 2*j;
        return 0;
    }
}
```

```java
// MinSubsetSumDiffTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MinSubsetSumDiffTest {
    @Test
    public void testMinDifference() {
        MinSubsetSumDiff solver = new MinSubsetSumDiff();
        int[] nums = {1,6,11,5};
        assertEquals(1, solver.minDifference(nums));
    }
}
```

---

## 5️⃣ Word Squares (LC 425)

```java
// WordSquares.java
import java.util.*;

public class WordSquares {
    private List<List<String>> ans = new ArrayList<>();
    private Map<String, List<String>> prefixMap = new HashMap<>();

    public List<List<String>> wordSquares(String[] words) {
        int n = words[0].length();
        for (String word : words) {
            for (int i = 1; i <= n; i++) {
                prefixMap.computeIfAbsent(word.substring(0,i), k -> new ArrayList<>()).add(word);
            }
        }
        List<String> path = new ArrayList<>();
        for (String word : words) {
            path.add(word);
            backtrack(path, n);
            path.remove(path.size()-1);
        }
        return ans;
    }

    private void backtrack(List<String> path, int n) {
        if (path.size() == n) {
            ans.add(new ArrayList<>(path));
            return;
        }
        StringBuilder prefix = new StringBuilder();
        int idx = path.size();
        for (String word : path) prefix.append(word.charAt(idx));
        List<String> candidates = prefixMap.getOrDefault(prefix.toString(), new ArrayList<>());
        for (String cand : candidates) {
            path.add(cand);
            backtrack(path, n);
            path.remove(path.size()-1);
        }
    }
}
```

```java
// WordSquaresTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class WordSquaresTest {
    @Test
    public void testWordSquares() {
        WordSquares solver = new WordSquares();
        String[] words = {"area","lead","wall","lady","ball"};
        List<List<String>> result = solver.wordSquares(words);
        assertTrue(result.size() > 0);
    }
}
```

---

## ✅ Engineering / Practice Notes

* Bitmasking + recursion is essential for **subset assignment problems**.
* Patterns:

  * Track **state using mask**
  * Combine **backtracking + DP**
  * Use for **TSP, assignment, subset sum, max/min optimization**
* Complexity is exponential in nature but reduced using memoization.
* Classic interview pattern for **hard combinatorial optimization problems**.

---


