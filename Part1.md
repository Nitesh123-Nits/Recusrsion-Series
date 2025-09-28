
# Series plan (12 posts)

1. **Recursion Fundamentals** — recursion anatomy, patterns, tail vs tree recursion, memoization. (this message)
2. **Backtracking I — Combinatorics** — subsets, combinations, permutations, generate-parentheses.
3. **Backtracking II — Constraint Search** — Combination Sum family, Letter Combinations, Word Search.
4. **Recursion on Trees** — preorder/inorder/postorder, depth, diameter, invert, path sums.
5. **Recursion on Linked Lists** — reverse, merge, remove-nth-from-end, recursion vs iterative tradeoffs.
6. **Divide & Conquer** — binary search recursion, merge sort recursion, quickselect.
7. **Grid & Graph DFS Recursion** — islands, connected components, DFS with visited sets, cycle detection.
8. **Recursion → DP Transition** — memoization, top-down vs bottom-up, catalan numbers examples.
9. **Advanced Backtracking** — permutations with duplicates, pruning, iterative deepening.
10. **Recursion Patterns for Interviews** — pattern templates, time-space reasoning, trick questions.
11. **Testing & Debugging Recursive Code** — unit tests, stack depth, iterative transforms, tail-call simulation.
12. **Capstone: Problem Walkthroughs** — 10 medium+ problems solved end-to-end (design, code, tests).

---

# Post #1 — Recursion Fundamentals (practical, Java-heavy)

## What recursion gives you

* Expressive, naturally maps to recursive data (trees, lists, combinations).
* Easier to implement some algorithms cleanly.
* Cost: stack depth, sometimes exponential time if repeated subproblems — fixable via memoization.

## Recap: the anatomy of a recursive function

1. **Base case(s)** — must cover termination.
2. **Recursive step** — operate closer to base case.
3. **State** — prefer immutable params or explicit helper wrappers to avoid bugs.
4. **Complexity mind-set** — write recurrence, solve or memoize.

---

## Templates & Patterns (with Java skeletons)

### 1) Simple linear recursion (e.g., factorial)

When your problem reduces size by 1 each step.

```java
// factorial — O(n) time, O(n) stack (can be O(1) with loop)
public class RecursionBasics {
    public static long factorial(int n) {
        if (n <= 1) return 1;
        return n * factorial(n - 1);
    }
}
```

### 2) Two-branch / tree recursion (e.g., naive Fibonacci)

Exponential blow-up unless memoized.

```java
// naive Fibonacci — O(2^n) time, O(n) stack
public static int fibNaive(int n) {
    if (n < 2) return n;
    return fibNaive(n - 1) + fibNaive(n - 2);
}

// memoized — O(n) time, O(n) space
public static int fibMemo(int n) {
    int[] memo = new int[Math.max(2, n + 1)];
    Arrays.fill(memo, -1);
    return fibMemoHelper(n, memo);
}
private static int fibMemoHelper(int n, int[] memo) {
    if (n < 2) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fibMemoHelper(n - 1, memo) + fibMemoHelper(n - 2, memo);
    return memo[n];
}
```

Practice: **Fibonacci (LeetCode 509)**. ([LeetCode][1])

### 3) Backtracking template (combinatorial generation)

Use a `List` for path, recurse, then undo (backtrack).

```java
// subsets example
public static List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    backtrackSubsets(nums, 0, new ArrayList<>(), ans);
    return ans;
}

private static void backtrackSubsets(int[] nums, int idx, List<Integer> path, List<List<Integer>> ans) {
    ans.add(new ArrayList<>(path)); // record current subset
    for (int i = idx; i < nums.length; i++) {
        path.add(nums[i]);
        backtrackSubsets(nums, i + 1, path, ans); // move forward
        path.remove(path.size() - 1); // backtrack
    }
}
```

Practice: **Subsets (LeetCode 78)**. ([LeetCode][2])

### 4) Permutations (in-place swap approach)

```java
public static List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    permuteHelper(nums, 0, ans);
    return ans;
}

private static void permuteHelper(int[] nums, int i, List<List<Integer>> ans) {
    if (i == nums.length) {
        List<Integer> perm = new ArrayList<>();
        for (int v : nums) perm.add(v);
        ans.add(perm);
        return;
    }
    for (int j = i; j < nums.length; j++) {
        swap(nums, i, j);
        permuteHelper(nums, i + 1, ans);
        swap(nums, i, j); // backtrack
    }
}
private static void swap(int[] a, int i, int j) {
    int t = a[i]; a[i] = a[j]; a[j] = t;
}
```

Practice: **Permutations (LeetCode 46)**. ([LeetCode][3])

### 5) Generate well-formed parentheses (balanced constraints)

Use counts (open/close) to prune.

```java
public static List<String> generateParenthesis(int n) {
    List<String> ans = new ArrayList<>();
    gen(0, 0, n, new StringBuilder(), ans);
    return ans;
}

private static void gen(int open, int close, int n, StringBuilder sb, List<String> ans) {
    if (sb.length() == 2 * n) {
        ans.add(sb.toString());
        return;
    }
    if (open < n) {
        sb.append('(');
        gen(open + 1, close, n, sb, ans);
        sb.deleteCharAt(sb.length() - 1);
    }
    if (close < open) {
        sb.append(')');
        gen(open, close + 1, n, sb, ans);
        sb.deleteCharAt(sb.length() - 1);
    }
}
```

Practice: **Generate Parentheses (LeetCode 22)**. ([LeetCode][4])

### 6) DFS on grid (visited matrix) — word search example

```java
// word search boolean check using DFS/backtracking
public static boolean exist(char[][] board, String word) {
    int m = board.length, n = board[0].length;
    boolean[][] seen = new boolean[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (dfsWord(board, word, 0, i, j, seen)) return true;
        }
    }
    return false;
}

private static boolean dfsWord(char[][] b, String w, int idx, int r, int c, boolean[][] seen) {
    if (idx == w.length()) return true;
    if (r < 0 || c < 0 || r >= b.length || c >= b[0].length) return false;
    if (seen[r][c] || b[r][c] != w.charAt(idx)) return false;
    seen[r][c] = true;
    boolean found = dfsWord(b, w, idx + 1, r + 1, c, seen)
                 || dfsWord(b, w, idx + 1, r - 1, c, seen)
                 || dfsWord(b, w, idx + 1, r, c + 1, seen)
                 || dfsWord(b, w, idx + 1, r, c - 1, seen);
    seen[r][c] = false;
    return found;
}
```

Practice: **Word Search (LeetCode 79)**. ([LeetCode][5])

---

## Complexity checklist & tips

* Always write the recurrence: e.g., `T(n) = T(n-1) + O(1)` or `T(n) = 2T(n-1) + O(1)` etc.
* For backtracking that enumerates results (permutations/subsets), the time is *output-sensitive* (e.g., `O(n * 2^n)` for subsets).
* Use memoization for overlapping subproblems; use pruning (bounds, sorting) for backtracking duplicates.
* Prefer iterative when recursion depth might blow the stack (e.g., deep linked list) — or use `Deque` and simulate recursion.

---

## Quick practice roadmap (first 2 weeks)

* Day 1: Factorial, Fibonacci (memo), Recurrence practice — small write-ups + unit tests. **LC 509**. ([LeetCode][1])
* Day 2–3: Subsets, Combinations, Permutations. **LC 78, 77, 46**. ([LeetCode][2])
* Day 4–5: Generate Parentheses, Letter Combinations. **LC 22, 17**. ([LeetCode][4])
* Day 6–8: Backtracking with constraints — Combination Sum, Word Search. **LC 39, 79**. ([LeetCode][6])
* Day 9–12: Trees recursion (max depth, invert tree, path sums) — then move to DP/memoization.

---

## Starter checklist for your codebase (engineering-grade)

* Put recursive helpers `private static` inside the same class; prefer wrapper methods to keep public API clean.
* Add JUnit tests with representative base cases and boundary cases (empty inputs, single element).
* Add iterative alternatives where stack depth can exceed limits; profile for hot spots.
* Document time/space per method as comments.

---

## Links to the core LeetCode problems I referenced (quick access)

* Fibonacci (509). ([LeetCode][1])
* Permutations (46). ([LeetCode][3])
* Subsets (78). ([LeetCode][2])
* Combinations (77). ([LeetCode][7])
* Generate Parentheses (22). ([LeetCode][4])
* Letter Combinations (17). ([LeetCode][8])
* Combination Sum (39). ([LeetCode][6])
* Word Search (79). ([LeetCode][5])

---

## Next post (Post #2) — what you’ll get

* Deep-dive on **Backtracking (Combinatorics)**: clean, idiomatic Java solutions for Subsets / Combinations / Permutations / Generate Parentheses; advanced variants (duplicates, k-size), optimizations and iterative alternatives. Full working repo snippet and 10 practice problems with difficulty tags.

---

If this looks good I’ll start **Post #2** next: full backtracking chapter with polished Java classes, unit tests (JUnit 5), complexity proofs, and a short checklist of common pitfalls. Want the code in a GitHub-ready single-class repo layout or several small classes + tests? (I’ll just pick one if you don’t answer — I’ll prefer modular classes + JUnit tests.)

Happy to tailor the series tone — more interview-hack focused, or more engineering / production-quality?

[1]: https://leetcode.com/problems/fibonacci-number/?utm_source=chatgpt.com "Fibonacci Number"
[2]: https://leetcode.com/problems/subsets/?utm_source=chatgpt.com "Subsets"
[3]: https://leetcode.com/problems/permutations/?utm_source=chatgpt.com "Permutations"
[4]: https://leetcode.com/problems/generate-parentheses/?utm_source=chatgpt.com "Generate Parentheses"
[5]: https://leetcode.com/problems/word-search/?utm_source=chatgpt.com "Word Search"
[6]: https://leetcode.com/problems/combination-sum/?utm_source=chatgpt.com "Combination Sum"
[7]: https://leetcode.com/problems/combinations/?utm_source=chatgpt.com "Combinations"
[8]: https://leetcode.com/problems/letter-combinations-of-a-phone-number/?utm_source=chatgpt.com "Letter Combinations of a Phone Number"
