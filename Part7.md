
# Post #7 — Recursion + Memoization (Dynamic Programming) (Java Modular Classes)

## Problems we’ll cover

1. **Fibonacci Number** — `Fibonacci.java` + `FibonacciTest.java` (LC 509)
2. **Climbing Stairs** — `ClimbingStairs.java` + `ClimbingStairsTest.java` (LC 70)
3. **Unique Paths** — `UniquePaths.java` + `UniquePathsTest.java` (LC 62)
4. **Decode Ways** — `DecodeWays.java` + `DecodeWaysTest.java` (LC 91)
5. **House Robber** — `HouseRobber.java` + `HouseRobberTest.java` (LC 198)

---

## 1️⃣ Fibonacci Number (LC 509)

```java
// Fibonacci.java
import java.util.*;

public class Fibonacci {
    private Map<Integer, Integer> memo = new HashMap<>();

    public int fib(int n) {
        if (n <= 1) return n;
        if (memo.containsKey(n)) return memo.get(n);
        int result = fib(n - 1) + fib(n - 2);
        memo.put(n, result);
        return result;
    }
}
```

```java
// FibonacciTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class FibonacciTest {
    @Test
    public void testFib() {
        Fibonacci solver = new Fibonacci();
        assertEquals(0, solver.fib(0));
        assertEquals(1, solver.fib(1));
        assertEquals(55, solver.fib(10));
    }
}
```

---

## 2️⃣ Climbing Stairs (LC 70)

```java
// ClimbingStairs.java
import java.util.*;

public class ClimbingStairs {
    private Map<Integer, Integer> memo = new HashMap<>();

    public int climbStairs(int n) {
        if (n <= 2) return n;
        if (memo.containsKey(n)) return memo.get(n);
        int result = climbStairs(n - 1) + climbStairs(n - 2);
        memo.put(n, result);
        return result;
    }
}
```

```java
// ClimbingStairsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class ClimbingStairsTest {
    @Test
    public void testClimbStairs() {
        ClimbingStairs solver = new ClimbingStairs();
        assertEquals(2, solver.climbStairs(2));
        assertEquals(8, solver.climbStairs(5));
    }
}
```

---

## 3️⃣ Unique Paths (LC 62)

```java
// UniquePaths.java
import java.util.*;

public class UniquePaths {
    private Map<String, Integer> memo = new HashMap<>();

    public int uniquePaths(int m, int n) {
        return dfs(0, 0, m, n);
    }

    private int dfs(int r, int c, int m, int n) {
        if (r == m - 1 && c == n - 1) return 1;
        if (r >= m || c >= n) return 0;
        String key = r + "," + c;
        if (memo.containsKey(key)) return memo.get(key);
        int result = dfs(r + 1, c, m, n) + dfs(r, c + 1, m, n);
        memo.put(key, result);
        return result;
    }
}
```

```java
// UniquePathsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class UniquePathsTest {
    @Test
    public void testUniquePaths() {
        UniquePaths solver = new UniquePaths();
        assertEquals(28, solver.uniquePaths(3, 7));
        assertEquals(6, solver.uniquePaths(3, 3));
    }
}
```

---

## 4️⃣ Decode Ways (LC 91)

```java
// DecodeWays.java
import java.util.*;

public class DecodeWays {
    private Map<Integer, Integer> memo = new HashMap<>();

    public int numDecodings(String s) {
        return dfs(s, 0);
    }

    private int dfs(String s, int i) {
        if (i == s.length()) return 1;
        if (s.charAt(i) == '0') return 0;
        if (memo.containsKey(i)) return memo.get(i);
        int ans = dfs(s, i + 1);
        if (i + 1 < s.length() && Integer.parseInt(s.substring(i, i + 2)) <= 26)
            ans += dfs(s, i + 2);
        memo.put(i, ans);
        return ans;
    }
}
```

```java
// DecodeWaysTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class DecodeWaysTest {
    @Test
    public void testNumDecodings() {
        DecodeWays solver = new DecodeWays();
        assertEquals(2, solver.numDecodings("12"));
        assertEquals(3, solver.numDecodings("226"));
    }
}
```

---

## 5️⃣ House Robber (LC 198)

```java
// HouseRobber.java
import java.util.*;

public class HouseRobber {
    private Map<Integer, Integer> memo = new HashMap<>();

    public int rob(int[] nums) {
        return dfs(nums, 0);
    }

    private int dfs(int[] nums, int i) {
        if (i >= nums.length) return 0;
        if (memo.containsKey(i)) return memo.get(i);
        int result = Math.max(nums[i] + dfs(nums, i + 2), dfs(nums, i + 1));
        memo.put(i, result);
        return result;
    }
}
```

```java
// HouseRobberTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class HouseRobberTest {
    @Test
    public void testRob() {
        HouseRobber solver = new HouseRobber();
        assertEquals(4, solver.rob(new int[]{1,2,3,1}));
        assertEquals(12, solver.rob(new int[]{2,7,9,3,1}));
    }
}
```

---

## ✅ Engineering / Practice Notes

* Memoization drastically reduces **time complexity** from exponential to linear.
* Use **Map or array** to cache intermediate results.
* Many combinatorial or path-counting problems can be solved recursively first, then memoized.
* Once memoization works, **tabulation (bottom-up DP)** is usually the next step.

---

Next post (Post #8) will cover **Recursion with Backtracking + DP (Hard combinatorial problems)**: Combination Sum III, Palindrome Partitioning II, Word Break II, Minimum Path Sum, and LeetCode references.

Do you want me to draft **Post #8 — Advanced Recursion + DP / Backtracking** now?
