## Minimum Path Sum

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.


### Example:
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```


```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        int[][] minSumGrid = new int[m][n];
        minSumGrid[0][0] = grid[0][0];
                
        for(int i=1;i<m;i++){
            minSumGrid[i][0] = minSumGrid[i-1][0] + grid[i][0];
        }
        
        for(int j=1;j<n;j++){
            minSumGrid[0][j] = minSumGrid[0][j-1] + grid[0][j];
        }
        
                        
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(minSumGrid[i-1][j] < minSumGrid[i][j-1]){
                    minSumGrid[i][j] = minSumGrid[i-1][j] + grid[i][j];
                }else{
                    minSumGrid[i][j] = minSumGrid[i][j-1] + grid[i][j];
                }
            }
        }
        return minSumGrid[m-1][n-1];
    }
}
```  
