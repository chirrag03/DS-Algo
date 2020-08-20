## Container With Most Water

Given n non-negative integers a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub> , where each represents a point at coordinate (i, a<sub>i</sub>). n vertical lines are drawn such that the two endpoints of line i is at (i, a<sub>i</sub>) and (i, 0).  
Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and n is at least 2.  

### Example 1:
```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
In this case, the max area of water (blue section) the container can contain is 49 i.e between bar at index 1 and at index 8.
```
 

 ### Solutioning:

```java
class Solution {
    public int maxArea(int[] heights) {
        
        int max = 0;
        
        int left = 0;
        int right = heights.length - 1;
        
        while(left <= right){
            int h = Math.min(heights[left], heights[right]);
            int w = right - left;
            
            max = Math.max(max, h*w);
            
            if(heights[left] < heights[right]){
                left++;
            }else{
                right--;
            }
        }
    
        return max;
    }
}
```  
**Time Complexity:** O(N)  
**Space Complexity:**  O(1) 

