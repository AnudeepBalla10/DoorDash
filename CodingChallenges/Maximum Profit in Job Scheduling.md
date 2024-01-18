# Maximum Profit in Job Scheduling

We have n jobs, where every job is scheduled to be done from startTime[i] to endTime[i], obtaining a profit of profit[i].

You're given the startTime, endTime and profit arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

If you choose a job that ends at time X you will be able to start another job that starts at time X.


```java
class Solution {
    private class Job {
        private int startTime;
        private int endTime;
        private int profit;

        public Job(int s, int e, int p) {
            this.startTime = s;
            this.endTime = e;
            this.profit = p;
        }
    }

    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        List<Job> jobList = new ArrayList<>();

        for (int i = 0; i < profit.length; i++) {
            jobList.add(new Job(startTime[i], endTime[i], profit[i]));
        }

        Collections.sort(jobList, (a, b) -> a.endTime - b.endTime);

        TreeMap<Integer, Integer> map = new TreeMap<>();
        map.put(0, 0);

        int ans = 0;

        for (Job currJob : jobList) {
            int lastEndTime = map.floorKey(currJob.startTime);
            int lastEndTimeMaxProfit = map.get(lastEndTime);
            ans = Math.max(ans, lastEndTimeMaxProfit + currJob.profit);
            map.put(currJob.endTime, ans);
        }

        return ans;
    }
}

// TC: O(n * logn), SC: O(n)
```
