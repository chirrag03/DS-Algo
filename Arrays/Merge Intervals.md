## Merge Intervals

Given a collection of intervals, merge all overlapping intervals.


### Example 1:
```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```


### Example 2:
```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```


 ### Solutioning:

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length <= 1){
            return intervals;
        }
                        
        Arrays.sort(intervals, (interval1, interval2) -> interval1[0] - interval2[0]);
        
        List<int[]> outputIntervals = new ArrayList<>();
        for(int i=0;i<intervals.length;i++){
            int[] currInterval = intervals[i];
            
            int[] lastOutputInterval = null;
            if(outputIntervals.size() > 0){
                lastOutputInterval = outputIntervals.get(outputIntervals.size()-1);
            }
            
            if(lastOutputInterval == null || lastOutputInterval[1] < currInterval[0]){
                outputIntervals.add(new int[]{currInterval[0], currInterval[1]});
            }else{
                lastOutputInterval[1] = Math.max(lastOutputInterval[1], currInterval[1]);
            }
        }
        
        return outputIntervals.toArray(new int[outputIntervals.size()][]);
    }
}
```  
**Time Complexity:** O(nlogn)   
**Space Complexity:** O(1) or (O(n)) If we can sort intervals in place like we did above, we do not need more than constant additional space. Otherwise, we must allocate linear space to store a copy of intervals and sort that.



