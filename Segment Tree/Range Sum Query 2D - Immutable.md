## Range Sum Query 2D - Immutable

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

### Example 1:
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

**Note:** 
> You may assume that the matrix does not change.  
> There are many calls to sumRegion function.  
> You may assume that row1 ≤ row2 and col1 ≤ col2.  


### Solutioning:

```java
class NumMatrix {

    int[][] storage;
    int[][] matrix;
    int m;
    int n;
    public NumMatrix(int[][] matrix) {
        this.matrix = matrix;
        this.m = matrix.length;
        this.n = m > 0 ? matrix[0].length : 0;
        storage = new int[m+1][n+1];
        preprocess();
    }
    
    private void preprocess(){
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                int sum = getSum(i-1, j) + getSum(i, j-1) - getSum(i-1, j-1) + matrix[i][j];
                setSum(i, j, sum);
            }    
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return getSum(row2, col2) - getSum(row1-1, col2) - getSum(row2, col1-1) + getSum(row1-1, col1-1);
    }
    
    private int getSum(int r, int c){
        return storage[r+1][c+1];
    }
    
    private void setSum(int r, int c, int s){
        storage[r+1][c+1] = s;
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```


**Time Complexity:** O(MN) where M-rows, N-columns  
**Space Complexity:** O(MN) 
