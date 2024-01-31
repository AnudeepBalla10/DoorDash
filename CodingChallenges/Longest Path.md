Given a grid of numbers, find the longest path you can take by walking only on adjacent numbers of the same value. You cannot walk on the same number twice in the same path.

use DFS
```
[
    [1, 1, 2, 1],
    [5, 5, 5, 5],
    [5, 5, 5, 1]
]
```
```java
public class LongestPathInGrid {
    private static int maxPath = 0;

    public static void main(String[] args) {
        int[][] grid = {
            {1, 1, 2, 1},
            {5, 5, 5, 5},
            {5, 5, 5, 1}
        };
        System.out.println("Longest path length: " + longestPath(grid));
    }

    private static int longestPath(int[][] grid) {
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                boolean[][] visited = new boolean[grid.length][grid[0].length];
                dfs(grid, i, j, visited, grid[i][j], 0);
            }
        }
        return maxPath;
    }

    private static void dfs(int[][] grid, int i, int j, boolean[][] visited, int value, int length) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != value || visited[i][j]) {
            maxPath = Math.max(maxPath, length);
            return;
        }

        visited[i][j] = true;

        // Explore all 4 directions
        dfs(grid, i + 1, j, visited, value, length + 1); // Down
        dfs(grid, i - 1, j, visited, value, length + 1); // Up
        dfs(grid, i, j + 1, visited, value, length + 1); // Right
        dfs(grid, i, j - 1, visited, value, length + 1); // Left

        visited[i][j] = false; // Backtrack
    }
}
```
