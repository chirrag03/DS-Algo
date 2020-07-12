## Meeting Rooms II

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.


### Example 1:
```
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```


### Example 2:
```
Input: [[7,10],[2,4]]
Output: 1
```

 ### Solutioning:

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        List<Integer> startList = new ArrayList<>();
        List<Integer> endList = new ArrayList<>();
        
        for(int i=0;i<intervals.length;i++){
            startList.add(intervals[i][0]);
            endList.add(intervals[i][1]);
        }
        
        Collections.sort(startList);
        Collections.sort(endList);
        
        int max = 0;
        int rooms = 0;
        int i = 0, j = 0;
        while(i < startList.size() && j < endList.size()){
            if(startList.get(i) < endList.get(j)){
                rooms++;
                i++;
            }else{
                rooms--;
                j++;
            }
            max = Math.max(rooms, max);
        }
        
        return max;
    }
}
```  
**Time Complexity:** O(N)  
**Space Complexity:**  O(N) 
