## Longest Line of Consecutive One in Matrix

Given a 01 matrix M, find the longest line of consecutive one in the matrix. The line could be horizontal, vertical, diagonal or anti-diagonal.    

### Example 1:
```
Input:
[[0,1,1,0],
 [0,1,1,0],
 [0,0,0,1]]
Output: 3
```

Hint: The number of elements in the given matrix will not exceed 10,000  


### Solutioning:

Using 3D Dynamic Programming

```java
class Solution {
    public int longestLine(int[][] M) {
        if(M.length == 0){
            return 0;
        }
        
        int m = M.length;
        int n = M[0].length;
        
        int max = 0;
        int[][][] dp = new int[m][n][4];
        // dp[x][y][0] is for horizontal
        // dp[x][y][1] is for vertical
        // dp[x][y][2] is for anti-diagonal
        // dp[x][y][3] is for diagonal
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(M[i][j] == 1){
                    dp[i][j][0] = dp[i][j][1] = dp[i][j][2] = dp[i][j][3] = 1;
                    
                    if(j-1 >= 0 && M[i][j-1] == 1){
                        dp[i][j][0] = dp[i][j-1][0] + 1;
                    }
                    if(i-1 >= 0 && M[i-1][j] == 1){
                        dp[i][j][1] = dp[i-1][j][1] + 1;
                    }
                    if(i-1 >= 0 && j+1 < n && M[i-1][j+1] == 1){
                        dp[i][j][2] = dp[i-1][j+1][2] + 1;
                    }
                    if(i-1 >= 0 && j-1 >= 0 && M[i-1][j-1] == 1){
                        dp[i][j][3] = dp[i-1][j-1][3] + 1;
                    }
                    
                    max = Math.max(max, dp[i][j][0]);
                    max = Math.max(max, dp[i][j][1]);
                    max = Math.max(max, dp[i][j][2]);
                    max = Math.max(max, dp[i][j][3]);
                    
                }
            }
        }
        
        return max;
    }
}
```  
**Time Complexity:** O(MN) where we traverse the entire matrix once only.   
**Space Complexity:**  O(MN) 



### Optimized Solutioning:

Using 2D Dynamic Programming

```java
class Solution {
    public int longestLine(int[][] M) {
        if(M.length == 0){
            return 0;
        }
        
        int m = M.length;
        int n = M[0].length;
        
        int max = 0;
        int[][] dp = new int[n][4];
        for(int i=0;i<m;i++){
            int[][] curr = new int[n][4];
            for(int j=0;j<n;j++){
                if(M[i][j] == 1){
                    curr[j][0] = curr[j][1] = curr[j][2] = curr[j][3] = 1;
                    
                    if(j-1 >= 0 && M[i][j-1] == 1){
                        curr[j][0] = curr[j-1][0] + 1;
                    }
                    if(i-1 >= 0 && M[i-1][j] == 1){
                        curr[j][1] = dp[j][1] + 1;
                    }
                    if(i-1 >= 0 && j+1 < n && M[i-1][j+1] == 1){
                        curr[j][2] = dp[j+1][2] + 1;
                    }
                    if(i-1 >= 0 && j-1 >= 0 && M[i-1][j-1] == 1){
                        curr[j][3] = dp[j-1][3] + 1;
                    }
                    
                    max = Math.max(max, curr[j][0]);
                    max = Math.max(max, curr[j][1]);
                    max = Math.max(max, curr[j][2]);
                    max = Math.max(max, curr[j][3]);
                    
                }
            }
            dp = curr;
        }
        
        return max;
    }
}
```   

**Time Complexity:** O(MN) where we traverse the entire matrix once only.   
**Space Complexity:**  O(N) dp array of size 4n is used, where n is the number of columns of the matrix. 
