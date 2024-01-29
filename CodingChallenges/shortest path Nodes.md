# Road Part of the shortest path

A dasher sometimes travels between cities. To avoid delays, the dasher first checks for the shortest routes. Given a map of the cities and their bidirectional roads represented by a graph of nodes and edges, determine which given roads are along any shortest path. Return an array of strings, one for each road in order, where the value is YES if the road is along a shortest path or NO if it is not.The roads or edges are named using their 1-based index within the input arrays.

Example
given a map of g_nodes = 5 nodes, the starting nodes, ending nodes and road lengths are:

Road from/to/weight
1 (1, 2, 1)
2 (2, 3, 1)
3 (3, 4, 1)
4 (4, 5, 1)
5 (5, 1, 3)
6 (1, 3, 2)
7 (5, 3, 1)

Example Input: (5, [1, 2, 3, 4, 5, 1, 5], [
2, 3, 4, 5, 1, 3, 3], [1, 1, 1, 1, 3, 2, 1])
The traveller must travel from city 1 to city g_nodes, so from node 1 to node 5 in this case.
The shortest path is 3 units long and there are three paths of that length: 1 → 5, 1 → 2 → 3 → 5, and 1 → 3 → 5.
Return an array of strings, one for each road in order, where the value is YES if a road is along a shortest path or NO if it is not. In this case, the resulting array is ['YES', 'YES', 'NO', 'NO', 'YES', 'YES', 'YES']. The third and fourth roads connect nodes (3, 4) and (4, 5) respectively. They are not on a shortest path, i.e. one with a length of 3 in this case.

```java
import java.util.*;

public class ShortestPathChecker {
    private static class Edge {
        int to, weight;
        Edge(int to, int weight) {
            this.to = to;
            this.weight = weight;
        }
    }

    private static class Node implements Comparable<Node> {
        int id, dist;
        Node(int id, int dist) {
            this.id = id;
            this.dist = dist;
        }

        @Override
        public int compareTo(Node other) {
            return Integer.compare(this.dist, other.dist);
        }
    }

    public static List<String> findShortestPaths(int g_nodes, int[] sources, int[] destinations, int[] weights) {
        // Build the graph
        Map<Integer, List<Edge>> graph = new HashMap<>();
        Map<String, Integer> weightBetween = new HashMap<>();
        for (int i = 0; i < sources.length; i++) {
            graph.computeIfAbsent(sources[i], k -> new ArrayList<>()).add(new Edge(destinations[i], weights[i]));
            graph.computeIfAbsent(destinations[i], k -> new ArrayList<>()).add(new Edge(sources[i], weights[i]));
            String key = sources[i] + "," + destinations[i];
            weightBetween.put(key, weights[i]);
            weightBetween.put(destinations[i] + "," + sources[i], weights[i]);
        }

        // Dijkstra's algorithm to find shortest path
        Map<Integer, Integer> dist = new HashMap<>();
        Map<Integer, Set<Integer>> parents = new HashMap<>();
        PriorityQueue<Node> queue = new PriorityQueue<>();
        queue.offer(new Node(1, 0));
        dist.put(1, 0);

        while (!queue.isEmpty()) {
            Node current = queue.poll();
            int node = current.id;

            if (graph.containsKey(node)) {
                for (Edge edge : graph.get(node)) {
                    int nei = edge.to;
                    int weight = edge.weight;
                    String key = node + "," + nei;
                    int newDist = dist.get(node) + weightBetween.get(key);

                    if (!dist.containsKey(nei) || newDist < dist.get(nei)) {
                        dist.put(nei, newDist);
                        parents.put(nei, new HashSet<>(Arrays.asList(node)));
                        queue.offer(new Node(nei, newDist));
                    } else if (newDist == dist.get(nei)) {
                        parents.get(nei).add(node);
                    }
                }
            }
        }

        // DFS to find shortest edges
        Set<String> shortestEdges = new HashSet<>();
        dfs(g_nodes, parents, new HashSet<>(), shortestEdges);

        // Determine which edges are part of the shortest path
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < sources.length; i++) {
            String edge1 = sources[i] + "," + destinations[i];
            String edge2 = destinations[i] + "," + sources[i];
            if (shortestEdges.contains(edge1) || shortestEdges.contains(edge2)) {
                ans.add("YES");
            } else {
                ans.add("NO");
            }
        }

        return ans;
    }

    private static void dfs(int node, Map<Integer, Set<Integer>> parents, Set<Integer> visited, Set<String> shortestEdges) {
        if (visited.contains(node)) return;
        visited.add(node);
        if (parents.containsKey(node)) {
            for (int p : parents.get(node)) {
                shortestEdges.add(node + "," + p);
                dfs(p, parents, visited, shortestEdges);
            }
        }
    }

    public static void main(String[] args) {
        List<String> result = findShortestPaths(
            5,
            new int[]{1, 2, 3, 4, 5, 1, 5},
            new int[]{2, 3, 4, 5, 1, 3, 3},
            new int[]{1, 1, 1, 1, 3, 2, 1}
        );
        System.out.println(result);
    }
}

```
