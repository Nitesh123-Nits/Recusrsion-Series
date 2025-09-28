
# Post #10 — Advanced DP + Recursion on Graphs / Grids (Java Modular Classes)

## Problems we’ll cover

1. **Minimum Path Sum with Obstacles** — `MinPathSumWithObstacles.java` + `MinPathSumWithObstaclesTest.java` (LC 63 / 64 variant)
2. **Unique Paths with Obstacles** — `UniquePathsWithObstacles.java` + `UniquePathsWithObstaclesTest.java` (LC 63)
3. **Dungeon Game (Minimum HP)** — `DungeonGame.java` + `DungeonGameTest.java` (LC 174)
4. **Maximum Gold Collection (Grid DP)** — `MaxGoldCollection.java` + `MaxGoldCollectionTest.java` (classic problem)
5. **Longest Increasing Path in a Matrix** — `LongestIncreasingPath.java` + `LongestIncreasingPathTest.java` (LC 329)

---

## 1️⃣ Minimum Path Sum with Obstacles

```java
// MinPathSumWithObstacles.java
public class MinPathSumWithObstacles {
    private int[][] memo;

    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        memo = new int[m][n];
        for (int[] row : memo) java.util.Arrays.fill(row, -1);
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
// MinPathSumWithObstaclesTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MinPathSumWithObstaclesTest {
    @Test
    public void testMinPathSum() {
        MinPathSumWithObstacles solver = new MinPathSumWithObstacles();
        int[][] grid = {{1,3,1},{1,5,1},{4,2,1}};
        assertEquals(7, solver.minPathSum(grid));
    }
}
```

---

## 2️⃣ Unique Paths with Obstacles (LC 63)

```java
// UniquePathsWithObstacles.java
public class UniquePathsWithObstacles {
    private int[][] memo;

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        memo = new int[m][n];
        for (int[] row : memo) java.util.Arrays.fill(row, -1);
        return dfs(obstacleGrid, 0, 0);
    }

    private int dfs(int[][] grid, int r, int c) {
        if (r >= grid.length || c >= grid[0].length || grid[r][c] == 1) return 0;
        if (r == grid.length - 1 && c == grid[0].length - 1) return 1;
        if (memo[r][c] != -1) return memo[r][c];
        memo[r][c] = dfs(grid, r + 1, c) + dfs(grid, r, c + 1);
        return memo[r][c];
    }
}
```

```java
// UniquePathsWithObstaclesTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class UniquePathsWithObstaclesTest {
    @Test
    public void testUniquePaths() {
        UniquePathsWithObstacles solver = new UniquePathsWithObstacles();
        int[][] grid = {{0,0,0},{0,1,0},{0,0,0}};
        assertEquals(2, solver.uniquePathsWithObstacles(grid));
    }
}
```

---

## 3️⃣ Dungeon Game (Minimum HP) (LC 174)

```java
// DungeonGame.java
public class DungeonGame {
    private int[][] memo;

    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length, n = dungeon[0].length;
        memo = new int[m][n];
        for (int[] row : memo) java.util.Arrays.fill(row, -1);
        return dfs(dungeon, 0, 0);
    }

    private int dfs(int[][] grid, int r, int c) {
        if (r >= grid.length || c >= grid[0].length) return Integer.MAX_VALUE;
        if (r == grid.length - 1 && c == grid[0].length - 1) return Math.max(1, 1 - grid[r][c]);
        if (memo[r][c] != -1) return memo[r][c];
        int minNext = Math.min(dfs(grid, r + 1, c), dfs(grid, r, c + 1));
        memo[r][c] = Math.max(1, minNext - grid[r][c]);
        return memo[r][c];
    }
}
```

```java
// DungeonGameTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class DungeonGameTest {
    @Test
    public void testCalculateMinimumHP() {
        DungeonGame solver = new DungeonGame();
        int[][] dungeon = {{-2,-3,3},{-5,-10,1},{10,30,-5}};
        assertEquals(7, solver.calculateMinimumHP(dungeon));
    }
}
```

---

## 4️⃣ Maximum Gold Collection (Grid DP)

```java
// MaxGoldCollection.java
public class MaxGoldCollection {
    private int[][] memo;
    private int[][] grid;

    public int getMaxGold(int[][] grid) {
        this.grid = grid;
        int m = grid.length, n = grid[0].length;
        memo = new int[m][n];
        for (int[] row : memo) java.util.Arrays.fill(row, -1);
        int maxGold = 0;
        for (int i = 0; i < m; i++) maxGold = Math.max(maxGold, dfs(i, 0));
        return maxGold;
    }

    private int dfs(int r, int c) {
        if (r < 0 || r >= grid.length || c >= grid[0].length) return 0;
        if (memo[r][c] != -1) return memo[r][c];
        int rightUp = dfs(r - 1, c + 1);
        int right = dfs(r, c + 1);
        int rightDown = dfs(r + 1, c + 1);
        memo[r][c] = grid[r][c] + Math.max(right, Math.max(rightUp, rightDown));
        return memo[r][c];
    }
}
```

```java
// MaxGoldCollectionTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MaxGoldCollectionTest {
    @Test
    public void testGetMaxGold() {
        MaxGoldCollection solver = new MaxGoldCollection();
        int[][] grid = {{1,3,1},{2,0,4},{0,6,4}};
        assertEquals(12, solver.getMaxGold(grid));
    }
}
```

---

## 5️⃣ Longest Increasing Path in a Matrix (LC 329)

```java
// LongestIncreasingPath.java
public class LongestIncreasingPath {
    private int[][] memo;
    private int[][] dirs = {{0,1},{1,0},{0,-1},{-1,0}};
    private int[][] matrix;

    public int longestIncreasingPath(int[][] matrix) {
        this.matrix = matrix;
        int m = matrix.length, n = matrix[0].length;
        memo = new int[m][n];
        int maxLen = 0;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                maxLen = Math.max(maxLen, dfs(i, j));
        return maxLen;
    }

    private int dfs(int r, int c) {
        if (memo[r][c] != 0) return memo[r][c];
        int max = 1;
        for (int[] d : dirs) {
            int nr = r + d[0], nc = c + d[1];
            if (nr >= 0 && nr < matrix.length && nc >= 0 && nc < matrix[0].length
                && matrix[nr][nc] > matrix[r][c])
                max = Math.max(max, 1 + dfs(nr, nc));
        }
        memo[r][c] = max;
        return max;
    }
}
```

```java
// LongestIncreasingPathTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class LongestIncreasingPathTest {
    @Test
    public void testLongestIncreasingPath() {
        LongestIncreasingPath solver = new LongestIncreasingPath();
        int[][] matrix = {{9,9,4},{6,6,8},{2,1,1}};
        assertEquals(4, solver.longestIncreasingPath(matrix));
    }
}
```

---

## ✅ Engineering / Practice Notes

* Grid DP + recursion uses **memoization to avoid recomputation**.
* Patterns: right/down moves, 4-direction DFS, or 8-direction DFS depending on problem.
* Handle **obstacles, negative values, boundaries** carefully.
* This approach reduces **exponential recursion** to **O(m*n)** time in most cases.
* Classic interview patterns: Min/Max path, Collect maximum values, Longest increasing/decreasing paths.

---

Next post (Post #11) will cover **Recursion with Bitmasking / Subset DP / Advanced Combinatorial Optimization**
