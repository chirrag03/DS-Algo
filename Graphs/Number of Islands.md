## Number of Islands 

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.


### Example 1:
```
Input:
11110
11010
11000
00000

Output: 1
```

### Example 2:
```
Input:
11000
11000
00100
00011

Output: 3
```

 ### Solutioning:

Basically we need to find all connected components.  

**Optimized Solution : O(mn)**  
```java
class Solution {
    
    public int numIslands(char[][] grid) {
        
        if(grid.length == 0){
            return 0;
        }

        int m = grid.length;
        int n = grid[0].length;

        boolean[][] visited = new boolean[m][n];
        
        int componentCount = 0;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                
                if(grid[i][j] == '1' && !visited[i][j]){
                    componentCount++;
                    dfs(i, j, m, n, grid, visited);
                }
                
            }
        }
        return componentCount;
    }
    
    public void dfs(int i, int j, int m, int n, char[][] grid, boolean[][] visited) {

      visited[i][j] = true;

      int[] deltaX = new int[]{0, -1, 0, +1};
      int[] deltaY = new int[]{-1, 0, +1, 0};

      for(int index=0;index<4;index++){
          int xCoord = i + deltaX[index];
          int yCoord = j + deltaY[index];

          if(xCoord >= 0 && yCoord >= 0 && xCoord < m && yCoord < n 
             && grid[xCoord][yCoord] == '1' && !visited[xCoord][yCoord]){
              dfs(xCoord, yCoord, m, n, grid, visited);
          }                        
      }
    }
}
```

**Alternate Solution:** This is lengthy because first we actually create the graph and then do a DFS. So we need to create classes Vertex and Cell.  

```java
import java.util.Map.Entry;

class Cell{
    int i;
    int j;
    
    public Cell(int i, int j){
        this.i = i;
        this.j = j;
    }
    
    public int hashCode() {
        return i+j;
    }
    public boolean equals(Object obj) {
        Cell v = (Cell) obj;
        return this.i == v.i && this.j == v.j;
    }
}
class Vertex{
    boolean visited;
    List<Vertex> adjVertices;
    
    public Vertex(){
        adjVertices = new ArrayList<>();
    }
}

class Solution {
    public int numIslands(char[][] grid) {
        
        if(grid.length == 0){
            return 0;
        }
        //key:cell; value:island no.
        Map<Cell, Vertex> map = new HashMap<>();

        int m = grid.length;
        int n = grid[0].length;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j] == '1'){
                    Cell currCellLocation = new Cell(i, j);
                    Vertex currVertex;
                    if(!map.containsKey(currCellLocation)){
                        currVertex = new Vertex();
                        map.put(currCellLocation, currVertex);
                    }else{
                        currVertex = map.get(currCellLocation);
                    }
                    
                    int[] deltaX = new int[]{0, -1, 0, +1};
                    int[] deltaY = new int[]{-1, 0, +1, 0};
                    
                    for(int index=0;index<4;index++){
                        int xCoord = i + deltaX[index];
                        int yCoord = j + deltaY[index];
                        
                        if(xCoord>=0 && yCoord>=0 && xCoord<m && yCoord<n 
                           && grid[xCoord][yCoord]=='1'){
                            
                            Cell adjCellLocation = new Cell(xCoord, yCoord);

                            Vertex adjCellVertex;
                            if(!map.containsKey(adjCellLocation)){
                                adjCellVertex = new Vertex();
                                map.put(adjCellLocation, adjCellVertex);
                            }else{
                                adjCellVertex = map.get(adjCellLocation);
                            }
                            currVertex.adjVertices.add(adjCellVertex);
                        }                        
                    }
                }
            }
        }
        return maxComponent(map);
    }
    
    public int maxComponent(Map<Cell, Vertex> map){
        int count = 0;
        
        for(Entry<Cell, Vertex> e : map.entrySet()){
            Vertex v = e.getValue();
            if(!v.visited){
                count++;
                dfs(v);
            }
        }
    
        return count;
    }
    
    public void dfs(Vertex v){
        v.visited = true;
        Iterator value = v.adjVertices.iterator(); 
        while (value.hasNext()) { 
            Vertex adjV = (Vertex) value.next();
            if(!adjV.visited){
                dfs(adjV);
            }
        } 
    }
    
}
```  
