# Dasher max profit

You're a dasher, and you want to try planning out your schedule. You can view a list of deliveries along with their associated start time, end time, and dollar amount for completing the order. Assuming dashers can only deliver one order at a time, determine the maximum amount of money you can make from the given deliveries.

The inputs are as follows:

- int start_time: when you plan to start your schedule
- int end_time: when you plan to end your schedule
- int d_starts[n]: the start times of each delivery[i]
- int d_ends[n]: the end times of each delivery[i]
- int d_pays[n]: the pay for each delivery[i]
  
The output should be an integer representing the maximum amount of money you can make by forming a schedule with the given deliveries.

Example #1
- start_time = 0
- end_time = 10
- d_starts = [2, 3, 5, 7]
- d_ends = [6, 5, 10, 11]
- d_pays = [5, 2, 4, 1]
Expected output: 6

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.ArrayList;

public class DasherSchedule {

    static class Delivery {
        int start, end, pay;
        
        Delivery(int start, int end, int pay) {
            this.start = start;
            this.end = end;
            this.pay = pay;
        }
    }

    public static void main(String[] args) {
        int start_time = 0;
        int end_time = 10;
        int[] d_starts = {2, 3, 5, 7};
        int[] d_ends = {6, 5, 10, 11};
        int[] d_pays = {5, 2, 4, 100};

        System.out.println(maximumEarnings(start_time, end_time, d_starts, d_ends, d_pays));
    }

    private static int maximumEarnings(int start_time, int end_time, int[] d_starts, int[] d_ends, int[] d_pays) {
        ArrayList<Delivery> deliveriesList = new ArrayList<>();

        // Filter deliveries that fit within the start_time and end_time
        for (int i = 0; i < d_starts.length; i++) {
            if (d_starts[i] >= start_time && d_ends[i] <= end_time) {
                deliveriesList.add(new Delivery(d_starts[i], d_ends[i], d_pays[i]));
            }
        }

        // Convert the list to an array for easier processing
        Delivery[] deliveries = new Delivery[deliveriesList.size()];
        deliveries = deliveriesList.toArray(deliveries);

        // Sort the deliveries based on their end times
        Arrays.sort(deliveries, Comparator.comparingInt(d -> d.end));

        // dp[i] will store the maximum earnings till the ith delivery
        int[] dp = new int[deliveries.length];
        dp[0] = deliveries[0].pay; // Base case for the first delivery

        // Iterate over each delivery
        for (int i = 1; i < deliveries.length; i++) {
            int pay = deliveries[i].pay;

            // Find the last non-conflicting delivery
            int l = binarySearch(deliveries, i);
            if (l != -1) {
                pay += dp[l];
            }

            // dp[i] is the max of including or excluding the current delivery
            dp[i] = Math.max(pay, dp[i - 1]);
        }

        // The last element of dp will have the maximum earnings
        return deliveries.length > 0 ? dp[deliveries.length - 1] : 0;
    }

    private static int binarySearch(Delivery[] deliveries, int index) {
        int lo = 0, hi = index - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (deliveries[mid].end <= deliveries[index].start) {
                if (mid == index - 1 || deliveries[mid + 1].end > deliveries[index].start) {
                    return mid;
                }
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
        return -1; // No non-conflicting delivery found
    }
}
