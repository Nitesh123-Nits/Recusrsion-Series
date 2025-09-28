
# Post #2 — Backtracking Combinatorics (Java)

## Problems we’ll cover (modular classes)

1. **Subsets** — `Subsets.java` + `SubsetsTest.java` (LC 78)
2. **Combinations (n choose k)** — `Combinations.java` + `CombinationsTest.java` (LC 77)
3. **Permutations** — `Permutations.java` + `PermutationsTest.java` (LC 46)
4. **Generate Parentheses** — `GenerateParentheses.java` + `GenerateParenthesesTest.java` (LC 22)
5. **Letter Combinations of Phone Number** — `LetterCombinations.java` + `LetterCombinationsTest.java` (LC 17)

---

## 1️⃣ Subsets (LC 78)

```java
// Subsets.java
import java.util.*;

public class Subsets {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int[] nums, int idx, List<Integer> path, List<List<Integer>> ans) {
        ans.add(new ArrayList<>(path));
        for (int i = idx; i < nums.length; i++) {
            path.add(nums[i]);
            backtrack(nums, i + 1, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```

```java
// SubsetsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class SubsetsTest {
    @Test
    public void testSubsets() {
        Subsets s = new Subsets();
        int[] nums = {1, 2, 3};
        List<List<Integer>> result = s.subsets(nums);
        assertEquals(8, result.size());
        assertTrue(result.contains(Arrays.asList(1,2)));
        assertTrue(result.contains(Arrays.asList()));
    }
}
```

---

## 2️⃣ Combinations (n choose k) (LC 77)

```java
// Combinations.java
import java.util.*;

public class Combinations {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(1, n, k, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int start, int n, int k, List<Integer> path, List<List<Integer>> ans) {
        if (path.size() == k) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i <= n; i++) {
            path.add(i);
            backtrack(i + 1, n, k, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```

```java
// CombinationsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class CombinationsTest {
    @Test
    public void testCombine() {
        Combinations c = new Combinations();
        List<List<Integer>> result = c.combine(4, 2);
        assertEquals(6, result.size());
        assertTrue(result.contains(Arrays.asList(1,2)));
    }
}
```

---

## 3️⃣ Permutations (LC 46)

```java
// Permutations.java
import java.util.*;

public class Permutations {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        permuteHelper(nums, 0, ans);
        return ans;
    }

    private void permuteHelper(int[] nums, int idx, List<List<Integer>> ans) {
        if (idx == nums.length) {
            List<Integer> perm = new ArrayList<>();
            for (int n : nums) perm.add(n);
            ans.add(perm);
            return;
        }
        for (int i = idx; i < nums.length; i++) {
            swap(nums, i, idx);
            permuteHelper(nums, idx + 1, ans);
            swap(nums, i, idx);
        }
    }

    private void swap(int[] nums, int i, int j) {
        int t = nums[i]; nums[i] = nums[j]; nums[j] = t;
    }
}
```

```java
// PermutationsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class PermutationsTest {
    @Test
    public void testPermute() {
        Permutations p = new Permutations();
        int[] nums = {1,2,3};
        List<List<Integer>> result = p.permute(nums);
        assertEquals(6, result.size());
        assertTrue(result.contains(Arrays.asList(1,2,3)));
    }
}
```

---

## 4️⃣ Generate Parentheses (LC 22)

```java
// GenerateParentheses.java
import java.util.*;

public class GenerateParentheses {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        backtrack(0, 0, n, new StringBuilder(), ans);
        return ans;
    }

    private void backtrack(int open, int close, int n, StringBuilder sb, List<String> ans) {
        if (sb.length() == 2 * n) {
            ans.add(sb.toString());
            return;
        }
        if (open < n) {
            sb.append('(');
            backtrack(open + 1, close, n, sb, ans);
            sb.deleteCharAt(sb.length() - 1);
        }
        if (close < open) {
            sb.append(')');
            backtrack(open, close + 1, n, sb, ans);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```

```java
// GenerateParenthesesTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class GenerateParenthesesTest {
    @Test
    public void testGenerateParenthesis() {
        GenerateParentheses g = new GenerateParentheses();
        List<String> result = g.generateParenthesis(3);
        assertEquals(5, result.size());
        assertTrue(result.contains("((()))"));
    }
}
```

---

## 5️⃣ Letter Combinations of Phone Number (LC 17)

```java
// LetterCombinations.java
import java.util.*;

public class LetterCombinations {
    private static final String[] KEYS = {
        "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
    };

    public List<String> letterCombinations(String digits) {
        List<String> ans = new ArrayList<>();
        if (digits == null || digits.isEmpty()) return ans;
        backtrack(digits, 0, new StringBuilder(), ans);
        return ans;
    }

    private void backtrack(String digits, int idx, StringBuilder sb, List<String> ans) {
        if (idx == digits.length()) {
            ans.add(sb.toString());
            return;
        }
        String letters = KEYS[digits.charAt(idx) - '0'];
        for (char c : letters.toCharArray()) {
            sb.append(c);
            backtrack(digits, idx + 1, sb, ans);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```

```java
// LetterCombinationsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class LetterCombinationsTest {
    @Test
    public void testLetterCombinations() {
        LetterCombinations l = new LetterCombinations();
        List<String> result = l.letterCombinations("23");
        assertEquals(9, result.size());
        assertTrue(result.contains("ad"));
        assertTrue(result.contains("cf"));
    }
}
```

---

✅ **Next Steps for Practice / Engineering**

* Add **edge cases**: empty input, single-element input, duplicates.
* Measure **stack depth** for large input.
* Compare **iterative vs recursive** where possible.
* Commit each class + test as a separate file for modular repo.

---

I can prepare **Post #3 — Backtracking with Constraints** next (Combination Sum, Word Search, Subsets/Permutations with duplicates, pruning strategies) in **the same modular structure** with unit tests and complexity notes.

Do you want me to go ahead and draft Post #3 now?
