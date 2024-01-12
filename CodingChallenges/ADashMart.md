# ADashMart

ADashMart is a warehouse run by DoorDash that houses items found in convenience stores, grocery stores, and restaurants. We have a city with open roads, blocked-off roads, and DashMarts.

City planners want you to identify how far a location is from its closest DashMart.

You can only travel over open roads (up, down, left, right). Locations are given in `[row, col]` format.

- Example 1:

Given a grid where:
- 'O' represents an open road that you can travel over in any direction (up, down, left, or right).
- 'X' represents a blocked road that you cannot travel through.
- 'D' represents a DashMart.

The grid is provided as a 2D array, and a list of locations is provided where each location is a pair `[row, col]`.
```
[
  ['X', 'O', 'O', 'D', 'O', 'O', 'X', 'O', 'X'], #0
  ['X', 'O', 'X', 'X', 'X', 'O', 'X', 'O', 'X'], #1
  ['O', 'O', 'O', 'D', 'X', 'X', 'O', 'X', 'O'], #2
  ['O', 'O', 'D', 'O', 'D', 'O', 'O', 'O', 'X'], #3
  ['O', 'O', 'O', 'O', 'O', 'X', 'O', 'O', 'X'], #4
  ['X', 'O', 'X', 'O', 'O', 'O', 'O', 'X', 'X'], #5
]
```
List of pairs `[row, col]` for locations:
[
  [200, 200],
  [1, 4],
  [0, 3],
  [5, 8],
  [1, 8],
  [5, 5],
]

- Your task is to return the distance for each location from the closest DashMart.

Provided:

- `city: char[][]`
- `locations: int[][]`

**Return:**

- `answer: int[]`

## BFS 

```java
import java.util.*;

public class DistanceToDashMart {
    private int rows;
    private int cols;
    private int[] answer;
    private static final int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};

    public int[] distanceToClosestDashMart(char[][] city, int[][] locations) {
        rows = city.length;
        cols = city[0].length;
        answer = new int[locations.length];

        // Calculate distances for each location
        for (int i = 0; i < locations.length; i++) {
            int row = locations[i][0];
            int col = locations[i][1];
            if (isValid(row, col)) {
                answer[i] = bfs(city, row, col);
            }
        }

        return answer;
    }

    // Helper method to check if a position is valid
    private boolean isValid(int x, int y) {
        return x >= 0 && x < rows && y >= 0 && y < cols;
    }

    // Helper method for BFS
    private int bfs(char[][] city, int startR, int startC) {
        Queue<int[]> queue = new LinkedList<>();
        Set<List<Integer>> visited = new HashSet<>();

        queue.offer(new int[]{startR, startC, 0});
        visited.add(Arrays.asList(startR, startC));

        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int r = current[0];
            int c = current[1];
            int distance = current[2];

            if (city[r][c] == 'D') {
                return distance;
            }

            for (int[] dir : directions) {
                int nr = r + dir[0];
                int nc = c + dir[1];

                if (isValid(nr, nc) && !visited.contains(Arrays.asList(nr, nc)) && city[nr][nc] != 'X') {
                    queue.offer(new int[]{nr, nc, distance + 1});
                    visited.add(Arrays.asList(nr, nc));
                }
            }
        }

        return -1; // If no DashMart is reachable from the current location
    }

    public static void main(String[] args) {
        DistanceToDashMart solution = new DistanceToDashMart();

        char[][] city = {
                {'X', 'O', 'O', 'D', 'O', 'O', 'X', 'O', 'X'},
                {'X', 'O', 'X', 'X', 'X', 'O', 'X', 'O', 'X'},
                {'O', 'O', 'O', 'D', 'X', 'X', 'O', 'X', 'O'},
                {'O', 'O', 'D', 'O', 'D', 'O', 'O', 'O', 'X'},
                {'O', 'O', 'O', 'O', 'O', 'X', 'O', 'O', 'X'},
                {'X', 'O', 'X', 'O', 'O', 'O', 'O', 'X', 'X'},
        };

        int[][] locations = {
                {200, 200},
                {1, 4},
                {0, 3},
                {5, 8},
                {1, 8},
                {5, 5},
        };

        int[] result = solution.distanceToClosestDashMart(city, locations);
        System.out.println(Arrays.toString(result));
    }
}


```
