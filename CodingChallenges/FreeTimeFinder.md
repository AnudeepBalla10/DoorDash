```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class FreeTimeFinder {

    public static void main(String[] args) {
        List<int[]> meetingslist = Arrays.asList(
            new int[]{3, 20},
            new int[]{-2, 0},
            new int[]{0, 2},
            new int[]{16, 17},
            new int[]{19, 23},
            new int[]{30, 40},
            new int[]{27, 33}
        );

        int starttime = -5;
        int endtime = 27;
        int minDuration = 2;

        List<int[]> freetime = findFreeTime(starttime, endtime, minDuration, meetingslist);

        for (int[] time : freetime) {
            System.out.println("[" + time[0] + ", " + time[1] + "]");
        }
    }

    private static List<int[]> findFreeTime(int starttime, int endtime, int minDuration, List<int[]> meetingsList) {
        //sort the meetingsList 
        meetingsList.sort((a,b)-> a[0]-b[0]);
        
        //merge overlapping meetings;
        List<int[]> mergedMeetings = new ArrayList<>();
        for(int[] meeting : meetingsList){
            if(mergedMeetings.isEmpty() || mergedMeetings.get(mergedMeetings.size()-1)[1]<meeting[0]){
                mergedMeetings.add(meeting);
            } else{
                mergedMeetings.get(mergedMeetings.size()-1)[1]= Math.max(mergedMeetings.get(mergedMeetings.size()-1)[1], meeting[1]);
            }
        }
            
            //check the mergedMeetings and find the gaps with minDuration;
         List<int[]> freeTimeSlots = new ArrayList<>();
        int previousEnd = starttime; 
         for(int[] meetings : mergedMeetings){
             if(meetings[0] - previousEnd >= minDuration && previousEnd >= starttime){
                 freeTimeSlots.add(new int[]{previousEnd,meetings[0]});
             }
             previousEnd = Math.max(previousEnd,meetings[1]);
         }
             
             if(endtime-previousEnd >=minDuration ){
                  freeTimeSlots.add(new int[]{previousEnd,endtime});
             }
             return freeTimeSlots;
        }
    }



```
