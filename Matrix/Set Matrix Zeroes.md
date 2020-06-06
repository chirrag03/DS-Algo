## Set Matrix Zeroes

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.


### Example 1:
```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

### Example 2:
```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

> **Follow up:**  
> A straight forward solution using O(mn) space is probably a bad idea.  
> A simple improvement uses O(m + n) space, but still not the best solution.  
> Could you devise a constant space solution?  


 ### Solutioning:

Iterate over the original array and if we find an entry, say cell[i][j] to be 0, then we iterate over row i and column j separately and set all the non zero elements to some high negative dummy value (say -1000000). Note, choosing the right dummy value for your solution is dependent on the constraints of the problem. Any value outside the range of permissible values in the matrix will work as a dummy value.  

Finally, we iterate over the original matrix and if we find an entry to be equal to the high negative value (constant defined initially in the code as MODIFIED), then we set the value in the cell to 0.

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if(matrix.length == 0){
            return;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
    
        int zeroReplacement = -10000;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j] == 0){
                     for(int k=0;k<m;k++){
                         if(matrix[k][j] != 0){
                             matrix[k][j] = zeroReplacement;
                         }
                     }
                    for(int k=0;k<n;k++){
                         if(matrix[i][k] != 0){
                             matrix[i][k] = zeroReplacement;
                         }
                     }
                }
            }
        }
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j] == zeroReplacement){
                     matrix[i][j] = 0;
                }
            }
        }
    }
      
}
```  
**Time Complexity:** O((M×N)×(M+N))  
**Space Complexity:** O(1)  


 ### Alternate Solutioning:

The inefficiency in the second approach is that we might be repeatedly setting a row or column even if it was set to zero already. We can avoid this by postponing the step of setting a row or a column to zeroes.

We can rather use the first cell of every row and column as a flag. This flag would determine whether a row or column has been set to zero. This means for every cell instead of going to M+NM+N cells and setting it to zero we just set the flag in two cells.

These flags are used later to update the matrix. If the first cell of a row is set to zero this means the row should be marked zero. If the first cell of a column is set to zero this means the column should be marked zero.

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if(matrix.length == 0){
            return;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
            
        boolean isRow = false;
        boolean isCol = false;
        
        for(int i=0;i<m;i++){
             if(matrix[i][0] == 0){
                 isCol = true;
             }
        }

        for(int j=0;j<n;j++){
             if(matrix[0][j] == 0){
                 isRow = true;
             }
        }
        
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(matrix[i][j] == 0){
                     matrix[i][0] = 0;
                     matrix[0][j] = 0;
                }
            }
        }
        
        for(int i=1;i<m;i++){
             if(matrix[i][0] == 0){
                 for(int j=0;j<n;j++){
                     matrix[i][j] = 0;
                }
             }
        }
        
        for(int j=1;j<n;j++){
             if(matrix[0][j] == 0){
                 for(int i=0;i<m;i++){
                     matrix[i][j] = 0;
                }
             }
        }
        
        if(isRow){
            for(int j=0;j<n;j++){
                 matrix[0][j] = 0;
            }
        }
        
        
        if(isCol){
            for(int i=0;i<m;i++){
                 matrix[i][0] = 0;
            }
        }
        
    }
      
}
```
**Time Complexity:** O(M×N)
**Space Complexity:** O(1)  
