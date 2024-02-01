# Two Dashers completing deliveries

Two Dashers can complete up to N possible ordered pickups on a certain day. These pickups are represented by two arrays of length N, one for each dasher.

A pickup is represented by the name of the merchant, for example, "chilis" or "mcdonalds".

We could have the following two arrays that represent the pickup jobs for the two dashers:

dasher1_pickups:["chilis", "albertsons", "walmart", "albertsons", "chilis", "mcdonalds", "burger king"]
dasher2_pickups:["chilis", "walmart", "chilis", "albertsons", "burger king", "applebees", "mcdonalds"]

The two dashers ride in the same car and want to complete the same pickup jobs together while respecting the original ordering.
At the same time however, they want to complete as many deliveries as possible to maximize their payout.
Question: What is the longest sequence of pickups that these two dashers can complete together?

Note: The two dashers must complete the deliveries in the above order (from left to right) but are allowed to skip pickup jobs.
IE, It is okay for the dashers to go from "walmart" to "mcdonalds" together, but they now forfeit the right to complete any delivery between those two merchants, or complete any delivery before their respective "walmarts".

Example 1:
dasher1_pickups:["chilis", "albertsons", "walmart", "albertsons", "chilis", "mcdonalds", "burger king"]
dasher2_pickups:["chilis", "walmart", "chilis", "albertsons", "burger king", "applebees", "mcdonalds"]
returns ["chilis", "walmart", "chilis", "burger king"]

dasher1_pickups:["chilis", "albertsons", "mcdonalds"]
dasher2_pickups:["burger king", "jamba juice", "applebees"]
*Dasher1 and Dasher2 do not share any of the same pickup jobs, so they do not have any pickup jobs together.
returns [].

use Longest Common Sequence and dp[];

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class DashPickupsLCS {

    public static List<String> findLongestCommonSequence(String[] dasher1, String[] dasher2) {
        int m = dasher1.length, n = dasher2.length;
        int[][] dp = new int[m + 1][n + 1];

        // Fill dp table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (dasher1[i - 1].equals(dasher2[j - 1])) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // Reconstruct the longest common sequence
        List<String> sequence = new ArrayList<>();
        for (int i = m, j = n; i > 0 && j > 0; ) {
            if (dasher1[i - 1].equals(dasher2[j - 1])) {
                sequence.add(dasher1[i - 1]);
                i--;
                j--;
            } else if (dp[i - 1][j] > dp[i][j - 1]) {
                i--;
            } else {
                j--;
            }
        }

        Collections.reverse(sequence); // Reverse to get the correct order
        return sequence;
    }

    public static void main(String[] args) {
        String[] dasher1 = {"chilis", "albertsons", "walmart", "albertsons", "chilis", "mcdonalds", "burger king"};
        String[] dasher2 = {"chilis", "walmart", "chilis", "albertsons", "burger king", "applebees", "mcdonalds"};

        List<String> longestCommonSequence = findLongestCommonSequence(dasher1, dasher2);
        System.out.println("Longest common sequence of pickups: " + longestCommonSequence);
    }
}
```
