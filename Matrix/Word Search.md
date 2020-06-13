## Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

### Example 1:
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```


> **Constraints:** 
> board and word consists only of lowercase and uppercase English letters.  
> 1 <= board.length <= 200  
> 1 <= board[i].length <= 200  
> 1 <= word.length <= 10^3  


 ### Solutioning:

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        if(board.length == 0){
            return false;
        }
        
        int m = board.length;
        int n = board[0].length;
        
        for(int i=0;i<m;i++){
           for(int j=0;j<n;j++){
               
               if(board[i][j] == word.charAt(0)){
                   //We store the cell no. in row major form in visited set
                   Set<Integer> visited = new HashSet<>(Arrays.asList(i*n + j));
                   if(dfs(board, word.substring(1), i, j, m, n, visited)){
                       return true;
                   }
               }
               
            } 
        }
        
        return false;
    }
    
    private boolean dfs(char[][] board, String word, int i, int j, int m , int n, 
                        Set<Integer> visited){
        
        //Base case
        if(word.length() == 0){
            return true;
        }
        
        int[] deltaX = new int[]{-1, 0, +1, 0};
        int[] deltaY = new int[]{0, 1, 0, -1};
        
        for(int k=0;k<4;k++){
            
            int xCoord = i - deltaX[k];
            int yCoord = j - deltaY[k];

            if(isSafe(xCoord, yCoord, m, n, visited) && board[xCoord][yCoord] == word.charAt(0)){
                
                visited.add(xCoord*n + yCoord);
                if(dfs(board, word.substring(1), xCoord, yCoord, m, n, visited)){
                    return true;
                }
                visited.remove(xCoord*n + yCoord);
            }
        }
        
        return false;
    }
    
    //To check if we can move onto this cell i.e. it is within the board premises as well as unvisited 
    private boolean isSafe(int i, int j, int m , int n, Set<Integer> visited){
        if(i >= 0 && i < m && j >= 0 && j < n && !visited.contains(i*n + j)){
            return true;
        }
        return false;
    }
}
```  
**Time Complexity:** O(N⋅4 <sup>L</sup>) where N is the number of cells in the board and L is the length of the word to be matched.  

We iterate through the board for backtracking, i.e. there could be N times invocation for the backtracking/dfs function in the worst case.  

For the backtracking/dfs function, its execution trace would be visualized as a 4-ary tree, each of the branches represent a potential exploration in the corresponding direction. Therefore, in the worst case, the total number of invocation would be the number of nodes in a full 4-nary tree, which is about 4<sup>L</sup>.  

**Space Complexity:** O(L) where L is the length of the word to be matched.  

The main consumption of the memory lies in the recursion call of the backtracking function. The maximum length of the call stack would be the length of the word. 
