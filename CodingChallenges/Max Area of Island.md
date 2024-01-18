# Max Area of Island

You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.


```java
class Solution {
    boolean seen[][];

    public int maxAreaOfIsland(int[][] grid) {
        int max_area = 0;
        int row = grid.length;
        int colums = grid[0].length;
       
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < colums; j++) {
                max_area = Math.max(max_area, area(i, j, grid));
            }
        }
        return max_area;
    }

    public int area(int row, int col, int[][] grid) {
        if (row < 0 || row >= grid.length || col < 0 || col >= grid[row].length || grid[row][col] == 0) {
            return 0;
        }
        grid[row][col] = 0;
        return (1 + area(row + 1, col, grid) + area(row - 1, col, grid) + area(row, col + 1, grid)
                + area(row, col - 1, grid));
    }
}
```
