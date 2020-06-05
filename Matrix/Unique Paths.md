## Unique Paths

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?


### Example 1:
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

### Example 2:
```
Input: m = 7, n = 3
Output: 28
```

> **Constraints:**  
> 1 <= m, n <= 100  
> It's guaranteed that the answer will be less than or equal to 2 * 10 ^ 9.

 ### Solutioning:

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] storage = new int[m][n];
        for(int i=0;i<m;i++){
            storage[i][0] = 1;
        }
        for(int j=0;j<n;j++){
            storage[0][j] = 1;
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                storage[i][j] = storage[i-1][j] + storage[i][j-1];
            }
        }
        return storage[m-1][n-1];
    }
}
```  
**Time Complexity:** O(mn)   
**Space Complexity:** O(1) 
