# Accumulated Times

## Problem Statement

A dasher is dropping off orders in a large building and we want to
 compute how much time they spend in each room. We are given a list of elements containing the name of a room, a time period, and a value
indicating whether a user is entering/leaving the room. The dasher cannot be in more than one room
at the same time. Once they exit a room, they return to the previous room. Determine the time spent in each room.
i.e.: 
```
Lobby 0 Enter
Hallway1 10 Enter
Room1 30 Enter
Room1 40 Exit // back in hallway1 now
Room1 50 Enter
Room1 60 Exit
Hallway1 90 Exit // back in Lobby
Hallway2 100 Enter
Hallway2 150 Exit
Lobby 160 Exit
```

Output:
```
map(Lobby = 30, Hallway1 = 60, Room1 = 20, Hallway2 = 50)
```

Allow the interviewee to construct their own data structures for function input/output.
Gain some understanding of whether they are comfortable in their language of choice and if they have
some reasoning for their choice.
Some examples:
```
Kotlin - List<Data class(enum, int, boolean/enum)> for cleanliness, readability, and unit testability
Python - List<Tuple> for ease of looping through
```

### Constraints

Candidates should ask for these themselves in order to fully grasp the problem.
If these are given by the interviewer or not considered in any way, reflect in scorecard below.

- Elements are sorted with respect to time.
- No negative times.
- Can assume valid flow. Each entered room will have an exit.
- Can assume non-cyclical/non-recursive flows. A dasher can't re-enter the room 
they're already in and they won't re-enter a room they've already entered without exiting. (i.e. Once in Hallway1, can't enter Lobby)






```java
import java.util.HashMap;
import java.util.Map;
import java.util.Stack;

public class DasherTimeTracker {

    public static Map<String, Integer> calculateTimeSpent(String[] events) {
        Map<String, Integer> timeSpent = new HashMap<>();
        Stack<String> roomStack = new Stack<>();
        int currentTime = 0;

        for (String event : events) {
            String[] parts = event.split(" ");
            String room = parts[0];
            int eventTime = Integer.parseInt(parts[1]);
            boolean entered = parts[2].equals("Enter");

            if (!roomStack.isEmpty()) {
                String lastRoom = roomStack.peek();
                int timeInRoom = eventTime - currentTime;
                timeSpent.put(lastRoom, timeSpent.getOrDefault(lastRoom, 0) + timeInRoom);
            }

            if (!entered) {
                roomStack.pop();
            } else {
                roomStack.push(room);
            }

            currentTime = eventTime;
        }

        return timeSpent;
    }

    public static void main(String[] args) {
        String[] events = {
                "Lobby 0 Enter",
                "Hallway1 10 Enter",
                "Room1 30 Enter",
                "Room1 40 Exit",
                "Room1 50 Enter",
                "Room1 60 Exit",
                "Hallway1 90 Exit",
                "Hallway2 100 Enter",
                "Hallway2 150 Exit",
                "Lobby 160 Exit"
        };

        Map<String, Integer> result = calculateTimeSpent(events);

        // Output the time spent in each room
        for (Map.Entry<String, Integer> entry : result.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
    }
}
