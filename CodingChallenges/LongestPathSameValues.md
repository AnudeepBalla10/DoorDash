# LongestPathSameValues


Given an integer matrix, find the length of the longest path that have same values. The matrix has no boundary limits. (like, Google Maps - see edit for context)

From each cell, you can either move to two directions: horizontal or vertical. You may NOT move diagonally or move outside of the boundary.

nums = [
[1,1],
[5,5],
[5,5]
]

Return 4 ( Four 5's).

```java
public class LongestPathSameValues {
    public int longestPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return 0;
        }

        int rows = matrix.length;
        int cols = matrix[0].length;
        int maxLength = 0;

        boolean[][] visited = new boolean[rows][cols];

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                maxLength = Math.max(maxLength, dfs(matrix, i, j, matrix[i][j], visited));
            }
        }

        return maxLength;
    }

    private int dfs(int[][] matrix, int row, int col, int value, boolean[][] visited) {
        int rows = matrix.length;
        int cols = matrix[0].length;

        if (row < 0 || row >= rows || col < 0 || col >= cols || visited[row][col] || matrix[row][col] != value) {
            return 0;
        }

        visited[row][col] = true;
        int length = 1;

        // Explore horizontal and vertical directions
        length += dfs(matrix, row + 1, col, value, visited);
        length += dfs(matrix, row - 1, col, value, visited);
        length += dfs(matrix, row, col + 1, value, visited);
        length += dfs(matrix, row, col - 1, value, visited);

        return length;
    }

    public static void main(String[] args) {
        int[][] nums = {
                {1, 1},
                {5, 5},
                {5, 5}
        };

        LongestPathSameValues solver = new LongestPathSameValues();
        int result = solver.longestPath(nums);
        System.out.println(result); // Output: 4
    }
}
