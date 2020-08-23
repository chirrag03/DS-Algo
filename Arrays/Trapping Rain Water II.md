## Trapping Rain Water II

Given an m x n matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.  

### Example 1:
```
Given the following 3x6 height map:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]

Return 4.
```

>**Constraints:**  
> 1 <= m, n <= 110  
> 0 <= heightMap[i][j] <= 20000  
 
 ### Optimized Solutioning:


```java
class Item{
    int height;
    int x;
    int y;
    
    public Item(int val, int x, int y){
        this.height = val;
        this.x = x;
        this.y = y;
    }
}
class Solution {
    public int trapRainWater(int[][] heightMap) {
     
        int m = heightMap.length;
        int n = heightMap[0].length;
                
        PriorityQueue<Item> q = new PriorityQueue<>((i1, i2) -> i1.height - i2.height);
        Set<Integer> visited = new HashSet<>();
        
        for(int i=0;i<m;i++){
            q.add(new Item(heightMap[i][0], i, 0));
            q.add(new Item(heightMap[i][n-1], i, n-1));
            markVisited(visited, n, i, 0);
            markVisited(visited, n, i, n-1);
        }
        
        for(int j=1;j<n-1;j++){
            q.add(new Item(heightMap[0][j], 0, j));
            q.add(new Item(heightMap[m-1][j], m-1, j));
            markVisited(visited, n, 0, j);
            markVisited(visited, n, m-1, j);
        }
        
        int[] dirX = {-1, 0, 1, 0};
        int[] dirY = {0, 1, 0, -1};
        
        int total = 0;
        while(!q.isEmpty()){
            Item curr = q.poll();
            
            for(int i=0;i<4;i++){
                int xCoord = curr.x + dirX[i];
                int yCoord = curr.y + dirY[i];
                
                if(xCoord >= 0 && xCoord < m && yCoord >= 0 && yCoord < n 
                   && !visited.contains(xCoord*n + yCoord)){
                    
                    if(heightMap[xCoord][yCoord] < curr.height){
                        total += curr.height - heightMap[xCoord][yCoord];
                        q.add(new Item(curr.height, xCoord, yCoord));
                        markVisited(visited, n, xCoord, yCoord);
                    }else{
                        q.add(new Item(heightMap[xCoord][yCoord], xCoord, yCoord));
                        markVisited(visited, n, xCoord, yCoord);
                    }
                    
                }
            }
            
        }
       
        return total;
    }
    
    private void markVisited(Set<Integer> visited, int n, int i, int j){
        visited.add(i*n + j);
    }
}
```  
**Time Complexity:** O(mn*log(mn))  
**Space Complexity:**  O(mn) 



