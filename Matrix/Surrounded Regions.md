## Surrounded Regions

Given a 2D board containing <b>'X'</b> and <b>'O'</b> (the letter O), capture all regions surrounded by <b>'X'</b>.

A region is captured by flipping all <b>'O'</b> into <b>'X'</b> in that surrounded region.

### Example 1:
```
X X X X
X O O X
X X O X
X O X X
After running your function, the board should be:

X X X X
X X X X
X X X X
X O X X
```
**Explanation:**  

Surrounded regions shouldn’t be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.


 ### Solutioning:

```java
class Solution {
    public void solve(char[][] board) {
        if(board.length == 0){
            return;
        }
        
        int m = board.length;
        int n = board[0].length;
        
        Set<Integer> uncapturedRegions = new HashSet<>();
        for(int i=0;i<m;i++){
           if(board[i][0] == 'O'){
                dfs(board, i, 0, m, n, uncapturedRegions);
            }
            
            if(board[i][n-1] == 'O'){
                dfs(board, i, n-1, m, n, uncapturedRegions);
            }
        }
        
        for(int j=0;j<n;j++){
            if(board[0][j] == 'O'){
                dfs(board, 0, j, m, n, uncapturedRegions);
            }
            
            if(board[m-1][j] == 'O'){
                dfs(board, m-1, j, m, n, uncapturedRegions);
            }
        } 
        
        for(int i=0;i<m;i++){
           for(int j=0;j<n;j++){
                if(!uncapturedRegions.contains(i*n + j)){
                    board[i][j] = 'X';
                }
            } 
        }
        
    }
    
    public void dfs(char[][] board, int i, int j, int m, int n, Set<Integer> visited) {
        
        visited.add(i*n + j);
        
        int[] deltaX = {-1, 0, 1, 0};
        int[] deltaY = {0, 1, 0, -1};
        
        for(int k=0;k<4;k++){
            int xCoord = i + deltaX[k];
            int yCoord = j + deltaY[k];
            
            if(isSafe(board, xCoord, yCoord, m, n, visited) && board[xCoord][yCoord] == 'O'){
                dfs(board, xCoord, yCoord, m, n, visited);    
            }
        }
    }
    
    public boolean isSafe(char[][] board, int i, int j, int m, int n, Set<Integer> visited) {
        
        if(i >= 0 && i < m && j >= 0 && j < n && !visited.contains(i*n + j)){
            return true;
        }
        
        return false;
    }
}
```  
<b>Time Complexity:</b> O(M*N) In the worst case where it contains only the O cells on the board, we would traverse each cell.  

<b>Space Complexity:</b> O(M*N) For the visited set. And the maximum depth of recursive calls would be M*N as in the worst scenario mentioned in the time complexity.


