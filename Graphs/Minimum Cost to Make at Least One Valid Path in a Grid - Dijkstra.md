## Minimum Cost to Make at Least One Valid Path in a Grid

Given a m x n grid. Each cell of the grid has a sign pointing to the next cell you should visit if you are currently in this cell. The sign of grid[i][j] can be:  
1 which means go to the cell to the right. (i.e go from grid[i][j] to grid[i][j + 1])  
2 which means go to the cell to the left. (i.e go from grid[i][j] to grid[i][j - 1])  
3 which means go to the lower cell. (i.e go from grid[i][j] to grid[i + 1][j])  
4 which means go to the upper cell. (i.e go from grid[i][j] to grid[i - 1][j])  
Notice that there could be some **invalid signs** on the cells of the grid which points outside the grid.  

You will initially start at the upper left cell (0,0). A valid path in the grid is a path which starts from the upper left cell (0,0) and ends at the bottom-right cell (m - 1, n - 1) following the signs on the grid. **The valid path doesn't have to be the shortest.**  

You can modify the sign on a cell with cost = 1. You can modify the sign on a cell **one time only.**

Return the minimum cost to make the grid have at least one valid path.  


### Example 1:
```
Input: grid = [[1,1,1,1],[2,2,2,2],[1,1,1,1],[2,2,2,2]]
Output: 3
Explanation: You will start at point (0, 0).
The path to (3, 3) is as follows. (0, 0) --> (0, 1) --> (0, 2) --> (0, 3) change the arrow to down with cost = 1 --> (1, 3) --> (1, 2) --> (1, 1) --> (1, 0) change the arrow to down with cost = 1 --> (2, 0) --> (2, 1) --> (2, 2) --> (2, 3) change the arrow to down with cost = 1 --> (3, 3)
The total cost = 3.
```

### Example 2:
```
Input: grid = [[1,1,3],[3,2,2],[1,1,4]]
Output: 0
Explanation: You can follow the path from (0, 0) to (2, 2).
```

### Example 3:
```
Input: grid = [[1,2],[4,3]]
Output: 1
```

### Example 4:
```
Input: grid = [[2,2,2],[2,2,2]]
Output: 3
```

### Example 5:
```
Input: grid = [[4]]
Output: 0
```

> **Constraints:**    
> m == grid.length  
> n == grid[i].length  
> 1 <= m, n <= 100    


 ### Solutioning:
We'll use Dijkstra's algorithm. 

```java
class Solution {
    public int minCost(int[][] grid) {
        
        int m = grid.length;
        int n = grid[0].length;
        
        Integer[][] shortestDistanceStorage = new Integer[m][n];
        
        PriorityQueue<Integer[]> heap = new PriorityQueue<>((i1, i2) -> i1[2] - i2[2]);
        heap.add(new Integer[]{0, 0, 0});
        
        while(!heap.isEmpty()){
            
            //As we poll an element from the heap, 
            //it means now its a visited vertex and we got its shortest distance
            Integer[] src = heap.poll();
            int srcX =  src[0];
            int srcY = src[1];
            int dist = src[2];
            
            if(srcX == m-1 && srcY == n-1){
                return dist;
            }
            
            //If this vertex has been taken out of heap already, 
            //then its shortest distance must have been stored, hence we skip this 
            //This is required because we never update a node when we get less distance, 
            //instead we added the node again with lesser distance
            if(shortestDistanceStorage[srcX][srcY] != null){
                continue;
            }
            
            shortestDistanceStorage[srcX][srcY] = dist;
            
            int[] dirX = new int[]{0, 0, 1, -1};
            int[] dirY = new int[]{1, -1, 0, 0};

            for(int k=0;k<4;k++){

                int adjX = srcX + dirX[k];
                int adjY = srcY + dirY[k];
                
                if(!isValid(adjX, adjY, m, n)){
                    continue;
                }
                
                int cost = grid[srcX][srcY] - 1 != k ? 1 : 0;
                int newDist = dist + cost;

                //Only if we haven't computed dist for adjNode push it in heap
                if(shortestDistanceStorage[adjX][adjY] == null){
                    heap.add(new Integer[]{adjX, adjY, newDist});
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
**NOTE**: Here there will never be a path that will be longer and has less cost because movement allowed only in 4 directions. However, if a diagonal movement was allowed then what would have happened? Dijkstra would have given me the shortest path to reach the node but not with min cost. There could have been a possibility that a longer path existed that gave me mincost. Then this would have been similar to the question Shortest Path in a Grid with Obstacles Elimination.

**Time Complexity:** O(mnlog(mn)) as complexity of Dijkstra is Elog(E)....to be checked      

**Space Complexity:** O(mn) ....to be checked   

