## Spiral Matrix

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.


### Example 1:
```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```


### Example 2:
```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```


 ### Solutioning:
The answer will be all the elements in clockwise order from the first-outer layer, followed by the elements from the second-outer layer, and so on.


```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if(matrix.length == 0){
            return new ArrayList<>();
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        int rowStart = 0;
        int colStart = 0;
        int rowEnd = m - 1;
        int colEnd = n - 1;
        
        List<Integer> result = new ArrayList<>();
        
        while(rowStart <= rowEnd && colStart <= colEnd){
            for(int j=colStart;j<=colEnd;j++){
                result.add(matrix[rowStart][j]);
            }
            
            for(int i=rowStart+1;i<=rowEnd;i++){
                result.add(matrix[i][colEnd]);
            }
            
            if(rowStart != rowEnd){
                for(int j=colEnd-1;j>colStart;j--){
                    result.add(matrix[rowEnd][j]);
                }
            }
            
            if(colStart != colEnd){
                for(int i=rowEnd;i>rowStart;i--){
                    result.add(matrix[i][colStart]);
                }
            }
            
            rowStart++;
            colStart++;
            rowEnd--;
            colEnd--;
        }
        
        return result;
    }
}
```  
**Time Complexity:** O(N) where N is the total number of elements in the input matrix. We add every element in the matrix to our final answer.
   
**Space Complexity:** O(1) without considering the output array
