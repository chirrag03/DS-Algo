## Maximal Rectangle

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.


### Example 1:
```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```



 ### Solutioning:

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix.length == 0){
            return 0;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        int max = 0;
        
        int[] prevHist = new int[n];
        
        for(int i=0;i<m;i++){
            int[] currHist = new int[n];
            for(int j=0;j<n;j++){
                currHist[j] = matrix[i][j] == '1' ? prevHist[j] + 1 : 0;
            }
            prevHist = currHist;
            max = Math.max(max, largestRectangleArea(currHist));
        }
        
        return max;
    }
    
    //Histogram problem no. 84
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
            int w = s.isEmpty() ? i : i-1-s.peek();
            max = Math.max(max, h*w);
        }
        
        return max;
    }
}
```  
**Time Complexity:** O(M*N) where M is the number of rows and N is the number of columns.   

**Space Complexity:**  O(N) 

