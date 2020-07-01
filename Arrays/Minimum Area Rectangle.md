## Minimum Area Rectangle

Given a set of points in the xy-plane, determine the minimum area of a rectangle formed from these points, with sides parallel to the x and y axes.

If there isn't any rectangle, return 0.  


### Example 1:
```
Input: [[1,1],[1,3],[3,1],[3,3],[2,2]]
Output: 4
```


### Example 2:
```
Input: [[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
Output: 2
```

> **Note:**  
> 1 <= points.length <= 500.  
> 0 <= points[i][0] <= 40000.  
> 0 <= points[i][1] <= 40000.  
> All points are distinct.  

 ### Solutioning:


```java
import java.util.Map.*;
class Solution {
    public int minAreaRect(int[][] points) {
        
        //Vertical lines map: key:xcoord, value:ycoord
        Map<Integer, Set<Integer>> map = new TreeMap<>();
        
        for(int i=0;i<points.length;i++){
            Set<Integer> possibleYcoord = map.getOrDefault(points[i][0], new HashSet<>());
            possibleYcoord.add(points[i][1]);
            map.put(points[i][0], possibleYcoord);
        }
        
        int min = Integer.MAX_VALUE;
        for(int i=0;i<points.length;i++){
            int x1 = points[i][0];
            int y1 = points[i][1];
            
            //possibleYcoord for same x1
            Set<Integer> possibleYcoord = map.get(x1);
            
            for(Integer y2 : possibleYcoord){
                if(y2 == y1){
                    continue;
                }
                
                //Till now we have 2 points with same x1 coord i.e. (x1, y1) and (x1, y2)
                
                //We need to find 2 more points for same x2 such that (x2, y1) and (x2, y2)
                for(Entry<Integer, Set<Integer>> e : map.entrySet()){
                    int x2 = e.getKey();
                    
                    if(x1 == x2){
                        continue;
                    }
                    
                    Set<Integer> currSet = e.getValue();
                    if(currSet.contains(y1) && currSet.contains(y2)){
                        int area = Math.abs(x1 - x2)*Math.abs(y1 - y2);
                        min = Math.min(min, area);
                    }
                }
            }
        }
        
        if(min == Integer.MAX_VALUE){
            return 0;
        }
        return min;
    }
}
```  
**Time Complexity:** O(N<sup>2</sup>) where N is the length of points.  

**Space Complexity:**  O(N) 
