## Shortest Path in a Grid with Obstacles Elimination

Given a m * n grid, where each cell is either 0 (empty) or 1 (obstacle). In one step, you can move up, down, left or right from and to an empty cell.

Return the minimum number of steps to walk from the upper left corner (0, 0) to the lower right corner (m-1, n-1) given that you can eliminate at most k obstacles. If it is not possible to find such walk return -1.


### Example 1:
```
Input: 
grid = 
[[0,0,0],
 [1,1,0],
 [0,0,0],
 [0,1,1],
 [0,0,0]], 
k = 1
Output: 6
Explanation: 
The shortest path without eliminating any obstacle is 10. 
The shortest path with one obstacle elimination at position (3,2) is 6. Such path is (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> (3,2) -> (4,2).
```

### Example 2:
```
Input: 
grid = 
[[0,1,1],
 [1,1,1],
 [1,0,0]], 
k = 1
Output: -1
Explanation: 
We need to eliminate at least two obstacles to find such a walk.
```


> **Constraints:**    
> grid.length == m  
> grid[0].length == n  
> 1 <= m, n <= 40  
> 1 <= k <= m*n  
> grid[i][j] == 0 or 1  
> grid[0][0] == grid[m-1][n-1] == 0  


 ### Solutioning:
We'll use Dijkstra's algorithm. 

```java
class Solution {
    public int shortestPath(int[][] grid, int k) {
        
        int m = grid.length;
        int n = grid[0].length;
        if(k > m + n - 1) 
            return m + n - 2;
        Integer[][] shortestDistStorage = new Integer[m][n];
        
        PriorityQueue<Integer[]> heap = new PriorityQueue<>((i1, i2) -> {
         if(i1[2] == i2[2]){
             return i1[3] - i2[3];
         }   
            return i1[2] - i2[2];
        });
        heap.add(new Integer[]{0, 0, 0, 0});    //(x, y, dist, obstacles)
        
        while(!heap.isEmpty()){
            Integer[] curr = heap.poll();
            int srcX = curr[0];
            int srcY = curr[1];
            int dist = curr[2];
            int removedObstacles = curr[3];
            
            if(srcX == m-1 && srcY == n-1){
                return dist;
            }
            
            if(shortestDistStorage[srcX][srcY] != null 
               && shortestDistStorage[srcX][srcY] <= removedObstacles){
                continue;
            }
            
            shortestDistStorage[srcX][srcY] = removedObstacles;
            
            int[] dirX = new int[]{-1, 0, 1, 0};
            int[] dirY = new int[]{0, 1, 0, -1};
            for(int i=0;i<4;i++){
                int adjX = srcX + dirX[i];
                int adjY = srcY + dirY[i];
                
                if(!isValid(adjX, adjY, m, n)){
                    continue;
                }
                
                if(shortestDistStorage[adjX][adjY] == null 
                   || shortestDistStorage[adjX][adjY] > removedObstacles + grid[srcX][srcY]){
                    
                    if(grid[adjX][adjY] == 0  && removedObstacles <= k){
                        heap.add(new Integer[]{adjX, adjY, dist+1, removedObstacles});
                    }else if(grid[adjX][adjY] == 1 && removedObstacles + 1 <= k){
                        heap.add(new Integer[]{adjX, adjY, dist+1, removedObstacles + 1});
                    }
                    
                }
            }
        }
        
        return -1;
    }
    
    private boolean isValid(int i, int j, int m, int n){
        if(i >= 0 && i < m && j >=0 && j < n){
            return true;
        }
        return false;
    }
}
```  
**NOTE**: Here we have modified Dijkstra a little. Why? Dijkstra gives me the shortest path to reach a node. But there can be a path that is longer and can be reached after removing less number of obstacles. At this stage I'll have to consider the path with less number of obstacles.

**Time Complexity:** O(mnklog(mnk)) as complexity of Dijkstra is Elog(E)....to be checked      

**Space Complexity:** O(mnk) ....to be checked   

