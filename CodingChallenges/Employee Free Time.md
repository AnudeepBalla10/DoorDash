# Employee Free Time

```java
/*
// Definition for an Interval.
class Interval {
    public int start;
    public int end;

    public Interval() {}

    public Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/

class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> result = new ArrayList<>();
        List<Interval> all = new ArrayList<>();
        
        //1. Add all the intervals to a list
        for (List<Interval> list: schedule) {
            for (Interval i: list) {
                all.add(i);
            }
        }
        //2. Sort based on start time
        Collections.sort(all, (a,b) -> Integer.compare(a.start, b.start));
        
        //3. Track latest end time and compare with next start time, if lesser, then it is free
        int latest = all.get(0).end;
        for (int i = 1; i < all.size(); i++) {
            int currStart = all.get(i).start;
            int currEnd = all.get(i).end;
            if (latest < currStart) {
                result.add(new Interval(latest, currStart));
            }
            latest = latest > currEnd? latest: currEnd;
        }
        return result;
    }
}
```

Time and Space Complexity -
n -> total number of intervals
Time complexity = O(n log n)
Space complexity = O(n)
