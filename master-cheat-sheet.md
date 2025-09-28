
# Recursion + DP / Backtracking Master Cheat Sheet

| Post | Topic / Pattern           | Example Problem                                                       | Technique                   | LeetCode                                                                                                                                                                                     | Key Notes                                                          |
| ---- | ------------------------- | --------------------------------------------------------------------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| 1    | Basic Recursion           | Factorial, Fibonacci                                                  | Plain recursion             | [LC 509](https://leetcode.com/problems/fibonacci-number/)                                                                                                                                    | Simple recursion, exponential                                      |
| 2    | Recursion with Parameters | Sum of digits, Reverse string                                         | Parameter passing           | —                                                                                                                                                                                            | Track extra params to avoid global state                           |
| 3    | Recursion with Branching  | Subsets, Permutations                                                 | Backtracking                | [LC 78](https://leetcode.com/problems/subsets/), [LC 46](https://leetcode.com/problems/permutations/)                                                                                        | Use path list + recursion                                          |
| 4    | Recursion + String        | Reverse string, Palindrome                                            | Index-based recursion       | [LC 344](https://leetcode.com/problems/reverse-string/)                                                                                                                                      | Avoid extra copies, use indices                                    |
| 5    | Recursion + Array         | Maximum, Minimum, Sum                                                 | Index recursion             | —                                                                                                                                                                                            | Use index instead of slicing for efficiency                        |
| 6    | Recursion + Backtracking  | N-Queens, Rat in Maze                                                 | Path building               | [LC 51](https://leetcode.com/problems/n-queens/)                                                                                                                                             | Track path + backtrack, prune invalid states                       |
| 7    | Recursion + Memoization   | Climbing Stairs, Decode Ways                                          | Memoization                 | [LC 70](https://leetcode.com/problems/climbing-stairs/), [LC 91](https://leetcode.com/problems/decode-ways/)                                                                                 | Store computed results to avoid recomputation                      |
| 8    | Advanced Recursion + DP   | Combination Sum III, Word Break II, Target Sum                        | Backtracking + Memoization  | [LC 216](https://leetcode.com/problems/combination-sum-iii/), [LC 140](https://leetcode.com/problems/word-break-ii/), [LC 494](https://leetcode.com/problems/target-sum/)                    | Use HashMap / DP array to cache overlapping subproblems            |
| 9    | Recursion on Graphs       | Number of Islands, Clone Graph, Course Schedule                       | DFS, Topological Sort       | [LC 200](https://leetcode.com/problems/number-of-islands/), [LC 133](https://leetcode.com/problems/clone-graph/), [LC 207](https://leetcode.com/problems/course-schedule/)                   | Handle visited states, backtracking for grids                      |
| 10   | Grid DP + Recursion       | Minimum Path Sum, Unique Paths, Dungeon Game, Longest Increasing Path | Recursion + Memoization     | [LC 63](https://leetcode.com/problems/unique-paths-ii/), [LC 174](https://leetcode.com/problems/dungeon-game/), [LC 329](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/) | Use memo array, careful boundary & obstacle handling               |
| 11   | Bitmasking / Subset DP    | TSP, Max Compatibility, K-partition, Min Subset Sum Diff              | DP + Bitmask + Backtracking | [LC 698](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/), [LC 425](https://leetcode.com/problems/word-squares/)                                                             | Track state with bitmask, optimize exponential recursion with memo |

---

## ✅ Key Patterns Summary

1. **Recursion basics**: Always define base case clearly.
2. **Backtracking**: Build path → recurse → remove last (undo).
3. **Memoization**: Store results keyed by index, state, or mask.
4. **Graph recursion**: Track visited, DFS or BFS depending on problem.
5. **Grid DP**: Combine recursion + memo for min/max path, longest path, obstacles.
6. **Bitmasking / Subset DP**: Track assignment/selection states efficiently for combinatorial optimization.
7. **Interview prep tip**: Identify whether problem is **sequence / path / subset / partition / grid / graph** → choose correct pattern.

---
