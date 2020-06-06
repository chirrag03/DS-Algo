## Valid Sudoku

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.


### Example 1:
```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

### Example 2:
```
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being 
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

> **Note:**  
> A Sudoku board (partially filled) could be valid but is not necessarily solvable.  
> Only the filled cells need to be validated according to the mentioned rules.  
> The given board contain only digits 1-9 and the character '.'.  
> The given board size is always 9x9.  


 ### Solutioning:  

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int m = board.length;
        int n= board[0].length;
        
        Map<Integer, Set<Character>> rowWiseMap = new HashMap<>();
        Map<Integer, Set<Character>> colWiseMap = new HashMap<>();
        Map<Integer, Set<Character>> gridWiseMap = new HashMap<>();
        
        for(int i=0;i<9;i++){
            rowWiseMap.put(i, new HashSet());
            colWiseMap.put(i, new HashSet());
            gridWiseMap.put(i, new HashSet());
        }
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j] == '.'){
                    continue;
                }
                
                Set<Character> rowSet = rowWiseMap.get(i);
                Set<Character> colSet = colWiseMap.get(j);
                Set<Character> gridSet = gridWiseMap.get(getSubGrid(i, j, 3));
                
                if(rowSet.contains(board[i][j]) || colSet.contains(board[i][j]) || 
                   gridSet.contains(board[i][j])){
                    return false;
                }
                
                rowSet.add(board[i][j]);
                colSet.add(board[i][j]);
                gridSet.add(board[i][j]);
            }
        }
        
        return true;
    }
    
    private int getSubGrid(int i, int j, int n){
        int mapped_i = i/3;
        int mapped_j = j/3;
        
        return mapped_i*n + mapped_j;   
    }
}
```  
**Time Complexity:** O(mn)   
**Space Complexity:** O(mn) 
