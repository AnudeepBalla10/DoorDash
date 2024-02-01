Here is an n*n map where grid[i][j] represents the number of blockers in the position (i, j).
    In every unit of time, the number of blockers in every position will be decreased by one. 
	Every position with one or more blockers is not accessible. 
	
	Assume you are a dasher at DoorDash. You can drive from one position to another 4-directionally adjacent position
	if and only if the position is accessible. You will start at the top left position (0, 0) and your goal is to reach the
	bottom right position (n-1, n-1). Assuming you can drive infinite distances in zero time, what would 
	be the least time that you can reach the destination?

    Constraints:
    * n == grid.length
    * n == grid[i].length
    * 1 <= n <= 50
    * 0 <= grid[i][j] < n^2
    * Each value grid[i][j] is unique.

    Example

    0  1  2  3  4
    24 16 22 21 5
    12 13 14 15 23
    11 17 18 19 20
    10 9  8  7  6

  input: int[][] grid
  output: 16 (int)
  explanation: 0->1->16->13->12->11->10->9->8->7->6, largest cell value in path is 16


```java
import java.util.PriorityQueue;

public class DasherPathFinder {

    private static final int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};

    public static int leastTimeToReachDestination(int[][] grid) {
        int n = grid.length;
        boolean[][] visited = new boolean[n][n];
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[2] - b[2]); // Min-heap based on blockers

        // Start from (0, 0) with the blocker value at this point
        pq.offer(new int[]{0, 0, grid[0][0]});
        visited[0][0] = true;

        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int r = current[0], c = current[1], blockers = current[2];

            // Reached destination
            if (r == n - 1 && c == n - 1) {
                return blockers;
            }

            for (int[] dir : directions) {
                int nr = r + dir[0], nc = c + dir[1];
                if (nr >= 0 && nr < n && nc >= 0 && nc < n && !visited[nr][nc]) {
                    visited[nr][nc] = true;
                    pq.offer(new int[]{nr, nc, Math.max(blockers, grid[nr][nc])});
                }
            }
        }

        return -1; // Should not reach here if input is valid
    }

    public static void main(String[] args) {
        int[][] grid = {
            {0, 1, 2, 3, 4},
            {24, 16, 22, 21, 5},
            {12, 13, 14, 15, 23},
            {11, 17, 18, 19, 20},
            {10, 9, 8, 7, 6}
        };

        System.out.println("Least time to reach destination: " + leastTimeToReachDestination(grid));
    }
}

```
