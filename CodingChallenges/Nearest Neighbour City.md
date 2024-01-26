# Nearest Neighbour City
A number of cities are arranged on a graph that has been divided up like an ordinary Cartesian plane. Each city is located at an integral (x, y) coordinate intersection. City names and locations are given in the form of three arrays: c, x, and y, which are aligned by the index to provide the city name (c[i]), and its coordinates, (x[i], y[i]). Determine the name of the nearest city that shares either an x or a y coordinate with the queried city. If no other cities share an x or y coordinate, return 'NONE'. If two cities have the same distance to the queried city, q[i], consider the one with an alphabetically shorter name (i.e. 'ab' < 'aba' < 'abb') as the closest choice. The distance is the Manhattan distance, the absolute difference in x plus the absolute difference in y.

The time complexity for my solution is O(NlogK) for processing input + O(QlogK) for returning the result for all the given queries,
where N is the number of cities, K is the max number of cities with same x or y coordinate and Q is the number of queries.


```java
import java.util.*;

public class NearestNeighbourCity {

    public static String findNearestCity(String[] c, int[] x, int[] y, String[] queries) {
        // Maps to store cities by their x and y coordinates
        Map<Integer, List<String>> citiesByX = new HashMap<>();
        Map<Integer, List<String>> citiesByY = new HashMap<>();

        // Populate the maps and sort the cities by name for each coordinate
        for (int i = 0; i < c.length; i++) {
            citiesByX.computeIfAbsent(x[i], k -> new ArrayList<>()).add(c[i]);
            citiesByY.computeIfAbsent(y[i], k -> new ArrayList<>()).add(c[i]);
        }

        citiesByX.values().forEach(Collections::sort);
        citiesByY.values().forEach(Collections::sort);

        // Map to store the nearest city for each query
        Map<String, String> nearestCity = new HashMap<>();

        for (String query : queries) {
            nearestCity.put(query, "NONE");
            int minDistance = Integer.MAX_VALUE;
            String nearest = "NONE";

            for (int i = 0; i < c.length; i++) {
                if (c[i].equals(query)) {
                    int qx = x[i];
                    int qy = y[i];

                    // Check for nearest city with same x coordinate
                    for (String otherCity : citiesByX.getOrDefault(qx, new ArrayList<>())) {
                        if (!otherCity.equals(query)) {
                            int distance = Math.abs(y[i] - y[Arrays.asList(c).indexOf(otherCity)]);
                            if (distance < minDistance || (distance == minDistance && otherCity.compareTo(nearest) < 0)) {
                                minDistance = distance;
                                nearest = otherCity;
                            }
                        }
                    }

                    // Check for nearest city with same y coordinate
                    for (String otherCity : citiesByY.getOrDefault(qy, new ArrayList<>())) {
                        if (!otherCity.equals(query)) {
                            int distance = Math.abs(x[i] - x[Arrays.asList(c).indexOf(otherCity)]);
                            if (distance < minDistance || (distance == minDistance && otherCity.compareTo(nearest) < 0)) {
                                minDistance = distance;
                                nearest = otherCity;
                            }
                        }
                    }

                    // Update the nearest city for the current query
                    nearestCity.put(query, nearest);
                    break;
                }
            }
        }

        // Build the result string
        StringBuilder result = new StringBuilder();
        for (String query : queries) {
            result.append(nearestCity.get(query)).append("\n");
        }

        return result.toString();
    }

    public static void main(String[] args) {
        String[] c = {"c1", "c2", "c3"};
        int[] x = {2, 2, 1};
        int[] y = {3, 2, 3};
        String[] queries = {"c1", "c2", "c3"};

        System.out.println(findNearestCity(c, x, y, queries));
    }
}

```
