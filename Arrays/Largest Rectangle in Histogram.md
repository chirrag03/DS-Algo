## Largest Rectangle in Histogram

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.
  

### Example 1:
```
Input: [2,1,5,6,2,3]
Output: 10
```
 

 ### Solutioning:
https://www.youtube.com/watch?time_continue=1&v=RVIh0snn4Qc&feature=emb_logo

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int max = 0;
        Stack<Integer> s = new Stack<>();
        int i = 0;
        
        while(i < heights.length){
            if(s.isEmpty() || heights[i] >= heights[s.peek()]){
                s.push(i);
                i++;
            }else{
                int h = heights[s.pop()];
                int w = s.isEmpty() ? i : i-1-s.peek();
                max = Math.max(max, h*w);
            }
        }
        
        while(!s.isEmpty()){
            int h = heights[s.pop()];
            int w = s.  isEmpty() ? i : i-1-s.peek();
            max = Math.max(max, h*w);
        }
        
        return max;
    }
}
```  
**Time Complexity:** O(N)  
**Space Complexity:**  O(N) 

