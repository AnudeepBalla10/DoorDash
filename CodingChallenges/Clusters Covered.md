# Cluster Covered
At DoorDash, we need to make sure that drivers are available at popular locations to deliver food.
 There are certain location clusters in a city that need drivers, and a driver can only cover certain location clusters.
 A location cluster is considered “covered” if the driver can deliver for the entire location cluster.
 How many location clusters are covered?

 Provided:
 ```
 driver_available_locations = {
 {1,1,1,0,0},
 {0,1,1,1,1},
 {0,0,0,0,0],
 {1,0,0,1,0],
 {1,1,0,1,1}
 }
 drivers_needed_at = {
 {1,1,1,0,0},
 {0,0,1,1,1},
 {0,1,0,0,0},
 {1,0,1,1,0},
 {0,1,0,1,0}
 }
```

Return: Int indicating number of location cluster covered ## 3

```Java
import java.util.HashSet;
import java.util.Set;

public class LocationClusterCoverage {

    public static void main(String[] args) {
        int[][] driver_available_locations = {
            {1, 1, 1, 0, 0},
            {0, 1, 1, 1, 1},
            {0, 0, 0, 0, 0},
            {1, 0, 0, 1, 0},
            {1, 1, 0, 1, 1}
        };

        int[][] drivers_needed_at = {
            {1, 1, 1, 0, 0},
            {0, 0, 1, 1, 1},
            {0, 1, 0, 0, 0},
            {1, 0, 1, 1, 0},
            {0, 1, 0, 1, 0}
        };

        System.out.println("Number of covered location clusters: " + check(driver_available_locations, drivers_needed_at));
    }

    private static int check(int[][] driver_available_locations, int[][] drivers_needed_at) {
        Set<String> visited = new HashSet<>();
        int rows = driver_available_locations.length;
        int cols = driver_available_locations[0].length;
        int ans = 0;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (drivers_needed_at[r][c] == 1 && !visited.contains(r + "," + c)) {
                    if (dfs(r, c, driver_available_locations, drivers_needed_at, visited)) {
                        ans++;
                    }
                }
            }
        }

        return ans;
    }

    private static boolean dfs(int r, int c, int[][] driver_available_locations, int[][] drivers_needed_at, Set<String> visited) {
        if (r < 0 || c < 0 || r >= driver_available_locations.length || c >= driver_available_locations[0].length || visited.contains(r + "," + c) || drivers_needed_at[r][c] == 0) {
            return true;
        }

        boolean cur = true;
        visited.add(r + "," + c);
        int[][] directions = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        
        for (int[] dir : directions) {
            int rr = r + dir[0];
            int cc = c + dir[1];
            cur = cur && dfs(rr, cc, driver_available_locations, drivers_needed_at, visited);
        }

        return driver_available_locations[r][c] == 1 && cur;
    }
}

```
