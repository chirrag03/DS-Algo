## Search a 2D Matrix

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:  

Integers in each row are sorted in ascending from left to right.  
The first integer of each row is greater than the last integer of the previous row.

### Example 1:
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```


### Example 2:
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

 ### Solutioning:
The algorithm is a standard binary search :
> Initialise left and right indexes left = 0 and right = m x n - 1.  
> While left < right :
- Pick up the index in the middle of the virtual array as a pivot index : pivot_idx = (left + right) / 2.  
- The index corresponds to row = pivot_idx / n and col = pivot_idx % n in the initial matrix, and hence one could get the pivot_element. This element splits the virtual array in two parts.  
- Compare pivot_element and target to identify in which part one has to look for target.

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix.length == 0){
            return false;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        int start = 0; 
        int end = m * n - 1;
        
        while(start <= end){
            int midIndex = (start + end) / 2;
            int midElement = matrix[midIndex / n][midIndex % n];
            
            if(target == midElement){
                return true;
            }else if(target < midElement){
                end = midIndex - 1;
            }else if(target > midElement){
                start = midIndex + 1;
            }
        }
        
        return false;
    }
}
```  
**Time Complexity:** O(log(mn))   
**Space Complexity:** O(1) 
