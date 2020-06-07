## Game of Life

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules:

* Any live cell with fewer than two live neighbors dies, as if caused by under-population.
* Any live cell with two or three live neighbors lives on to the next generation.
* Any live cell with more than three live neighbors dies, as if by over-population..
* Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.  
Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

### Example 1:
```
Input: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```


> **Follow up:** 
> 1) Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.  
> 2) In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?  


 ### Solutioning:

```java
class Solution {
    public void gameOfLife(int[][] board) {
        
        int m = board.length;
        int n = board[0].length;
        
        int[] deltaX = new int[]{-1, -1, -1, 0, 1, 1, 1, 0};
        int[] deltaY = new int[]{-1, 0, 1, 1, 1, 0, -1, -1};
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                
                int liveNeighbours = 0;
                for(int k=0;k<8;k++){
                    int xCoord = i + deltaX[k];
                    int yCoord = j + deltaY[k];
                    
                    if(xCoord >= m || yCoord >= n || xCoord < 0 || yCoord < 0){
                        continue;
                    }
                    
                    int neigbourValue = board[xCoord][yCoord];

                    if(board[xCoord][yCoord] >= 2){
                        neigbourValue = getOriginalState(board[xCoord][yCoord]);
                    }
                    
                    if(neigbourValue == 1){
                        liveNeighbours++;
                    }
                
                }
                
                if(board[i][j] == 1 && liveNeighbours < 2){
                    board[i][j] = 4;
                }else if(board[i][j] == 1 && liveNeighbours >= 2 && liveNeighbours <= 3){
                    board[i][j] = 5;
                }else if(board[i][j] == 1 && liveNeighbours > 3){
                    board[i][j] = 4;
                }else if(board[i][j] == 0 && liveNeighbours == 3){
                    board[i][j] = 3;
                }
            }
        }
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j] >= 2){
                    board[i][j] = geUpdatedState(board[i][j]);
                }
            }
        }
    }
    
    private int getOriginalState(int n){
        switch(n){
            case 2: return 0;
            case 3: return 0;
            case 4: return 1;
            case 5: return 1;
        }
        return -1;
    }
    
    private int geUpdatedState(int n){
        switch(n){
            case 2: return 0;
            case 3: return 1;
            case 4: return 0;
            case 5: return 1;
        }
        return -1;
    }
}
```  
**Time Complexity:** O(M*N)   
**Space Complexity:** O(1) 

**Follow Up Solutions Hint**  
If we have an extremely sparse matrix, it would make much more sense to actually save the location of only the live cells and then apply the 4 rules accordingly using only these live cells.

The only problem with this solution would be when the entire board cannot fit into memory. If that is indeed the case, then we would have to approach this problem in a different way. For that scenario, we assume that the contents of the matrix are stored in a file, one row at a time.  

In order for us to update a particular cell, we only have to look at its 8 neighbors which essentially lie in the row above and below it. So, for updating the cells of a row, we just need the row above and the row below. Thus, we read one row at a time from the file and at max we will have 3 rows in memory. We will keep discarding rows that are processed and then we will keep reading new rows from the file, one at a time.


