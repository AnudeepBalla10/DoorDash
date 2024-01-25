# Get Active Time

Given a sequence of timestamps & actions of a dasher's activity within a day, 
we would like to know the active time of the dasher. Idle time is defined as the dasher has NO delivery at hand.
(That means all items have been dropped off at this moment and the dasher is just waiting for another pickup) 
Active time equals total time minus idle time. Below is an example. 

- Dropoff can only happen after pickup.
- 12:00am means midnight and 12:00pm means noon.
- All the time is within a day.

```  
Timestamp(12h) | Action
8:30am | pickup
9:10am | dropoff
10:20am| pickup
12:15pm| pickup
12:45pm| dropoff
2:25pm | dropoff
```
* total time = 2:25pm-8:30am = 355 mins;
* idle time = 10:20am-9:10am = 70 mins;
* active time = total time-idle time = 355-70 = 285 mins;

## Approach: PutTimes horizontally and calculate the interval diff;
-> [8:30,9:10][10:20,12:45][12:15,2:25] now find the gaps in intervals;

```java

import java.util.ArrayList;
import java.util.List;

public class ActiveTime {

    private static final String PICKUP = "pickup";
    private static final String DROPOFF = "dropoff";

    // Method to calculate active time based on pickup and dropoff intervals
    public static int getActiveTime(List<String> activity) {
        // Lists to store pickup and dropoff times
        List<Integer> pickups = new ArrayList<>();
        List<Integer> dropoffs = new ArrayList<>();

        // Variables to track the smallest pickup and highest dropoff times as 
        //smallestPickup - highestDropoff = total time.
        int smallestPickup = Integer.MAX_VALUE;
        int highestDropoff = Integer.MIN_VALUE;

        // Parse each action in the activity
        for (String action : activity) {
            // Split the action into parts
            String[] parts = action.split("\\|");
            // Extract action type (pickup/dropoff) and time in minutes
            String actionType = parts[1].trim();
            int timeInMinutes = getMins(parts[0].trim());
        
            // Process pickup action
            if (PICKUP.equals(actionType)) {
                pickups.add(timeInMinutes);
                smallestPickup = Math.min(smallestPickup, timeInMinutes);
            }
            // Process dropoff action
            else if (DROPOFF.equals(actionType)) {
                dropoffs.add(timeInMinutes);
                highestDropoff = Math.max(highestDropoff, timeInMinutes);
            }
        }

        // List to store pickup and dropoff intervals
        List<int[]> interval = new ArrayList<>();
        for (int i = 0; i < pickups.size(); i++) {
            System.out.println("store pickup : " + pickups.get(i)+" store dropoff: " + dropoffs.get(i));
            interval.add(new int[]{pickups.get(i), dropoffs.get(i)});
        }

        // Calculate total time and idle time
        int totalTime = highestDropoff - smallestPickup;
        System.out.println("total time "+ totalTime);
        int idleTime = 0;
        for (int index = 0; index < interval.size() - 1; index++) {
            int[] curr = interval.get(index);
            int[] next = interval.get(index + 1);

            // If there is an idle time between actions, add it to idleTime
            if (curr[1] < next[0]) {
                idleTime += next[0] - curr[1];
            } else {
                // If actions overlap, update the dropoff time of the current action
                next[1] = Math.max(next[1], curr[1]);
            }
        }

        // Return the active time (total time - idle time)
        return totalTime - idleTime;
    }

    // Method to convert time in string format to minutes
    public static int getMins(String time) {
        // Split time into hours and minutes
        String[] parts = time.split(":");
        int hours = Integer.parseInt(parts[0]);
        int mins = Integer.parseInt(parts[1].substring(0, parts[1].length() - 2));

        // Adjust hours based on am/pm and return total minutes
        if (parts[1].endsWith("pm") && hours < 12) {
            hours += 12;
        } else if (parts[1].endsWith("am") && hours == 12) {
            hours = 0;
        }

        return 60 * hours + mins;
    }

    // Main method for testing
    public static void main(String[] args) {
        List<String> activity = List.of(
                "8:30am | pickup",
                "9:10am | dropoff",
                "10:20am | pickup",
                "12:15pm | pickup",
                "12:45pm | dropoff",
                "2:25pm | dropoff"
        );

        int activeTime = getActiveTime(activity);

        System.out.println("Active Time: " + activeTime);
    }
}

```
