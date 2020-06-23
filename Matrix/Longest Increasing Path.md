## Longest Increasing Path in a Matrix

Given an integer matrix, find the length of the longest increasing path from any point to any other point in the matrix.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).  


### Example 1:
```
Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].
```

### Example 2:
```
Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```


 ### Solutioning:

Naive DFS  

```java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if(matrix.length == 0){
            return 0;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        int max = 0;
        for(int i=0;i<m;i++){
           for(int j=0;j<n;j++){
                max = Math.max(max, 1 + dfs(matrix, i, j, m, n));
            } 
        }
        
        return max;
    }
    
    private int dfs(int[][] board, int i, int j, int m , int n){
                
        int[] deltaX = new int[]{-1, 0, +1, 0};
        int[] deltaY = new int[]{0, 1, 0, -1};
        
        int max = 0;
        for(int k=0;k<4;k++){
            
            int xCoord = i - deltaX[k];
            int yCoord = j - deltaY[k];

            if(isSafe(xCoord, yCoord, m, n) && board[xCoord][yCoord] > board[i][j]){
                max = Math.max(max, 1+dfs(board, xCoord, yCoord, m, n));
            }
        }
        
        return max;
    }
    
    //To check if we can move onto this cell i.e. it is within the board premises 
    private boolean isSafe(int i, int j, int m , int n){
        if(i >= 0 && i < m && j >= 0 && j < n ){
            return true;
        }
        return false;
    }
}
```  
**Time Complexity:** O(2<sup>mn</sup>) or O(mn*2<sup>mn</sup>) is a confusion     
Consider a snake pattern path like below max length of a path is mn  
  ------>  
  <------  
  ------>  
    .  
    .  
    .  
  <------  
  ------->  
If you have direction from a ->b, then you don't have direction b->a. So in average we have only 2 directions for each cell.  


**Space Complexity:** O(mn) For each DFS we use stack space where the max depth of recursion is m*n   

**Optimized Solution**  
DFS + Memoization

```java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if(matrix.length == 0){
            return 0;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        Integer[][] dp = new Integer[m][n];
            
        int max = 0;
        for(int i=0;i<m;i++){
           for(int j=0;j<n;j++){
                max = Math.max(max, 1 + dfs(matrix, i, j, m, n, dp));
            } 
        }
        
        return max;
    }
    
    private int dfs(int[][] board, int i, int j, int m , int n, Integer[][] dp){
        
        if(dp[i][j] != null){
           return dp[i][j];
        }
        
        
        int[] deltaX = new int[]{-1, 0, +1, 0};
        int[] deltaY = new int[]{0, 1, 0, -1};
        
        int max = 0;
        for(int k=0;k<4;k++){
            int xCoord = i - deltaX[k];
            int yCoord = j - deltaY[k];

            if(isSafe(xCoord, yCoord, m, n) && board[xCoord][yCoord] > board[i][j]){
                max = Math.max(max, 1+dfs(board, xCoord, yCoord, m, n, dp));   
            }
        }
        dp[i][j] = max;
        return max;
    }
    
    //To check if we can move onto this cell i.e. it is within the board premises
    private boolean isSafe(int i, int j, int m , int n){
        if(i >= 0 && i < m && j >= 0 && j < n){
            return true;
        }
        return false;
    }
}
```

**Time Complexity:** O(mn)  Each vertex/cell will be calculated once and only once, and each edge will be visited once and only once. The total time complexity is then O(V+E). V is the total number of vertices and E is the total number of edges. In our problem, O(V) = O(mn), O(E) = O(4V) = O(mn).    

**Space Complexity:** O(mn)  
