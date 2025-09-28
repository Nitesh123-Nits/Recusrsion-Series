
# Post #9 — Recursion on Graphs (Java Modular Classes)

## Problems we’ll cover

1. **Number of Islands (DFS on grid)** — `NumIslands.java` + `NumIslandsTest.java` (LC 200)
2. **Clone Graph** — `CloneGraph.java` + `CloneGraphTest.java` (LC 133)
3. **Course Schedule (Topological Sort)** — `CourseSchedule.java` + `CourseScheduleTest.java` (LC 207)
4. **Graph Cycle Detection (Directed / Undirected)** — `GraphCycle.java` + `GraphCycleTest.java` (LC 261)
5. **Word Search (Grid Backtracking)** — `WordSearch.java` + `WordSearchTest.java` (LC 79)

---

## 1️⃣ Number of Islands (DFS on Grid) (LC 200)

```java
// NumIslands.java
public class NumIslands {
    public int numIslands(char[][] grid) {
        int count = 0;
        for (int i = 0; i < grid.length; i++)
            for (int j = 0; j < grid[0].length; j++)
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count++;
                }
        return count;
    }

    private void dfs(char[][] grid, int r, int c) {
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length || grid[r][c] == '0')
            return;
        grid[r][c] = '0';
        dfs(grid, r + 1, c);
        dfs(grid, r - 1, c);
        dfs(grid, r, c + 1);
        dfs(grid, r, c - 1);
    }
}
```

```java
// NumIslandsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class NumIslandsTest {
    @Test
    public void testNumIslands() {
        NumIslands solver = new NumIslands();
        char[][] grid = {
            {'1','1','0','0','0'},
            {'1','1','0','0','0'},
            {'0','0','1','0','0'},
            {'0','0','0','1','1'}
        };
        assertEquals(3, solver.numIslands(grid));
    }
}
```

---

## 2️⃣ Clone Graph (LC 133)

```java
// CloneGraph.java
import java.util.*;

class GraphNode {
    int val;
    List<GraphNode> neighbors;
    GraphNode(int val) { this.val = val; neighbors = new ArrayList<>(); }
}

public class CloneGraph {
    private Map<GraphNode, GraphNode> map = new HashMap<>();

    public GraphNode cloneGraph(GraphNode node) {
        if (node == null) return null;
        if (map.containsKey(node)) return map.get(node);
        GraphNode clone = new GraphNode(node.val);
        map.put(node, clone);
        for (GraphNode neighbor : node.neighbors) {
            clone.neighbors.add(cloneGraph(neighbor));
        }
        return clone;
    }
}
```

```java
// CloneGraphTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CloneGraphTest {
    @Test
    public void testCloneGraph() {
        GraphNode node1 = new GraphNode(1);
        GraphNode node2 = new GraphNode(2);
        node1.neighbors.add(node2);
        node2.neighbors.add(node1);

        CloneGraph solver = new CloneGraph();
        GraphNode cloned = solver.cloneGraph(node1);
        assertEquals(1, cloned.val);
        assertEquals(2, cloned.neighbors.get(0).val);
        assertEquals(cloned, cloned.neighbors.get(0).neighbors.get(0));
    }
}
```

---

## 3️⃣ Course Schedule (Topological Sort) (LC 207)

```java
// CourseSchedule.java
import java.util.*;

public class CourseSchedule {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) graph.add(new ArrayList<>());
        for (int[] p : prerequisites) graph.get(p[1]).add(p[0]);
        boolean[] visited = new boolean[numCourses];
        boolean[] onStack = new boolean[numCourses];
        for (int i = 0; i < numCourses; i++)
            if (!visited[i] && dfs(i, graph, visited, onStack)) return false;
        return true;
    }

    private boolean dfs(int node, List<List<Integer>> graph, boolean[] visited, boolean[] onStack) {
        visited[node] = true;
        onStack[node] = true;
        for (int nei : graph.get(node)) {
            if (!visited[nei] && dfs(nei, graph, visited, onStack)) return true;
            if (onStack[nei]) return true;
        }
        onStack[node] = false;
        return false;
    }
}
```

```java
// CourseScheduleTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CourseScheduleTest {
    @Test
    public void testCanFinish() {
        CourseSchedule solver = new CourseSchedule();
        int[][] pre = {{1,0},{2,1}};
        assertTrue(solver.canFinish(3, pre));
        int[][] pre2 = {{1,0},{0,1}};
        assertFalse(solver.canFinish(2, pre2));
    }
}
```

---

## 4️⃣ Graph Cycle Detection (Directed / Undirected) (LC 261)

```java
// GraphCycle.java
import java.util.*;

public class GraphCycle {
    public boolean hasCycle(int n, int[][] edges) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        for (int[] e : edges) { graph.get(e[0]).add(e[1]); graph.get(e[1]).add(e[0]); }
        boolean[] visited = new boolean[n];
        for (int i = 0; i < n; i++)
            if (!visited[i] && dfs(i, -1, visited, graph)) return true;
        return false;
    }

    private boolean dfs(int node, int parent, boolean[] visited, List<List<Integer>> graph) {
        visited[node] = true;
        for (int nei : graph.get(node)) {
            if (!visited[nei]) {
                if (dfs(nei, node, visited, graph)) return true;
            } else if (nei != parent) return true;
        }
        return false;
    }
}
```

```java
// GraphCycleTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class GraphCycleTest {
    @Test
    public void testHasCycle() {
        GraphCycle solver = new GraphCycle();
        int[][] edges = {{0,1},{1,2},{2,0}};
        assertTrue(solver.hasCycle(3, edges));
        int[][] edges2 = {{0,1},{1,2}};
        assertFalse(solver.hasCycle(3, edges2));
    }
}
```

---

## 5️⃣ Word Search (Grid Backtracking) (LC 79)

```java
// WordSearch.java
public class WordSearch {
    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; i++)
            for (int j = 0; j < board[0].length; j++)
                if (dfs(board, i, j, word, 0)) return true;
        return false;
    }

    private boolean dfs(char[][] board, int r, int c, String word, int idx) {
        if (idx == word.length()) return true;
        if (r < 0 || r >= board.length || c < 0 || c >= board[0].length || board[r][c] != word.charAt(idx))
            return false;
        char temp = board[r][c];
        board[r][c] = '#'; // mark visited
        boolean found = dfs(board, r+1, c, word, idx+1) ||
                        dfs(board, r-1, c, word, idx+1) ||
                        dfs(board, r, c+1, word, idx+1) ||
                        dfs(board, r, c-1, word, idx+1);
        board[r][c] = temp;
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
        WordSearch solver = new WordSearch();
        char[][] board = {
            {'A','B','C','E'},
            {'S','F','C','S'},
            {'A','D','E','E'}
        };
        assertTrue(solver.exist(board, "ABCCED"));
        assertFalse(solver.exist(board, "ABCB"));
    }
}
```

---

## ✅ Engineering / Practice Notes

* **Graph DFS pattern** is versatile: grids, adjacency list, adjacency matrix.
* **Backtracking on grids**: mark visited cells and revert after recursion.
* **Cycle detection**: differ for directed vs undirected graphs.
* Use memoization in **word search / path counting** to optimize overlapping subproblems.
* Most of these algorithms are **O(V+E)** for graphs or **O(m*n*word length)** for grids.

---

Next post (Post #10) will cover **Recursion on Advanced DP Graph / Grid Problems**, including:

* Minimum Cost Path with obstacles
* Unique Paths with obstacles
* Dungeon Game / DP on grid
* LeetCode links for further practice

