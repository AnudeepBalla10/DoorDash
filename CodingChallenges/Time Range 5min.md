# Time Range 5min



you are given 2 strings.  startTime : "mon 10:45 am" and  endTime: "mon 11:00 am". 
you need to output all the times between starttime and endtime in the interval of 5 minutes. 
output: ["11045", "11050","11055", "111000"]. 
in output each string represents the day+time+minute. eg: 11045: 1+10+45 => monday represents 1. tuesday 2 etc. 
Also, the output should be in 24hr format and input will be in 12hr format.
you are required to do input validations as they can have invalid time formats.

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class TimeIntervalGenerator {

    public static List<String> generateTimeInterval(String startTime, String endTime, int intervalMinutes) {
        List<String> result = new ArrayList<>();

        try {
            SimpleDateFormat inputFormat = new SimpleDateFormat("E hh:mm a");
            SimpleDateFormat outputFormat = new SimpleDateFormat("uHHmm");

            Date startDate = inputFormat.parse(startTime);
            Date endDate = inputFormat.parse(endTime);

            Date currentTime = startDate;

            while (currentTime.compareTo(endDate) <= 0) {
                result.add(outputFormat.format(currentTime));
                currentTime = new Date(currentTime.getTime() + intervalMinutes * 60 * 1000);
            }

        } catch (ParseException e) {
            e.printStackTrace();
            // Handle invalid date format
        }

        return result;
    }

    public static void main(String[] args) {
        String startTime = "Mon 10:45 AM";
        String endTime = "Mon 11:00 AM";
        int intervalMinutes = 5;

        List<String> output = generateTimeInterval(startTime, endTime, intervalMinutes);

        System.out.println(output);
    }
}

```


# Without Lib
```java
import java.util.ArrayList;
import java.util.List;

public class TimeIntervalGenerator {

    public static List<String> generateTimeInterval(String startTime, String endTime, int intervalMinutes) {
        List<String> result = new ArrayList<>();

        // Parse start time
        int startDay = getDayOfWeek(startTime.substring(0, 3));
        int startHour = Integer.parseInt(startTime.substring(4, 6));
        int startMinute = Integer.parseInt(startTime.substring(7, 9));

        // Parse end time
        int endDay = getDayOfWeek(endTime.substring(0, 3));
        int endHour = Integer.parseInt(endTime.substring(4, 6));
        int endMinute = Integer.parseInt(endTime.substring(7, 9));

        // Generate time intervals
        while (startDay < endDay || (startDay == endDay && (startHour < endHour || (startHour == endHour && startMinute <= endMinute)))) {
            result.add(formatTime(startDay, startHour, startMinute));
            startMinute += intervalMinutes;
            if (startMinute >= 60) {
                startHour++;
                startMinute %= 60;
            }
            if (startHour >= 24) {
                startDay++;
                startHour %= 24;
            }
        }

        return result;
    }

    private static int getDayOfWeek(String day) {
        // Map day abbreviation to numeric value (e.g., Mon -> 1, Tue -> 2, ...)
        switch (day) {
            case "Mon": return 1;
            case "Tue": return 2;
            case "Wed": return 3;
            case "Thu": return 4;
            case "Fri": return 5;
            case "Sat": return 6;
            case "Sun": return 7;
            default: return -1; // Invalid day
        }
    }

    private static String formatTime(int day, int hour, int minute) {
        // Format time as "uHHmm"
        return String.format("%d%02d%02d", day, hour, minute);
    }

    public static void main(String[] args) {
        String startTime = "Mon 10:45 AM";
        String endTime = "Mon 11:00 AM";
        int intervalMinutes = 5;

        List<String> output = generateTimeInterval(startTime, endTime, intervalMinutes);

        System.out.println(output);
    }
}

