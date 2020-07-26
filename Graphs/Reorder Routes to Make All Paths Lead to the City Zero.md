## Reorder Routes to Make All Paths Lead to the City Zero

There are n cities numbered from 0 to n-1 and n-1 roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.  

Roads are represented by connections where connections[i] = [a, b] represents a road from city a to b.  

This year, there will be a big event in the capital (city 0), and many people want to travel to this city.  

Your task consists of reorienting some roads such that each city can visit the city 0. Return the **minimum** number of edges changed.  

It's **guaranteed** that each city can reach the city 0 after reorder.  


### Example 1:
```
Input: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
Output: 3
Explanation: Change the direction of edges [0,1],[1,3],[4,5] such that each node can reach the node 0 (capital).
```

### Example 2:
```
Input: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
Output: 2
Explanation: Change the direction of edges [1,2],[3,4] such that each node can reach the node 0 (capital).
```

### Example 3:
```
Input: n = 3, connections = [[1,0],[2,0]]
Output: 0`
```

> **Constraints:**  
> 2 <= n <= 5 * 10^4  
> connections.length == n-1  
> connections[i].length == 2  
> 0 <= connections[i][0], connections[i][1] <= n-1  
> connections[i][0] != connections[i][1]  


 ### Solutioning:


```java
class Solution {
    public int minReorder(int n, int[][] connections) {
     
        Map<Integer, ArrayList<Integer>> inMap = new HashMap<>();
        Map<Integer, ArrayList<Integer>> outMap = new HashMap<>();
        
        for(int[] connection : connections){
            int src = connection[0];
            int dest = connection[1];
            
            ArrayList<Integer> inList = inMap.getOrDefault(dest, new ArrayList<>());
            inList.add(src);
            inMap.put(dest, inList);
            
            ArrayList<Integer> outList = outMap.getOrDefault(src, new ArrayList<>());
            outList.add(dest);
            outMap.put(src, outList);
        }
                
        return helper(0, inMap, outMap, new HashSet<>());
    }
    
    private int helper(int src, Map<Integer, ArrayList<Integer>> inMap, Map<Integer, ArrayList<Integer>> outMap, Set<Integer> visited){
    
        visited.add(src);
        
        int count = 0;
        
        for(Integer i : outMap.getOrDefault(src, new ArrayList<>())){
            if(!visited.contains(i)){
                count += 1 + helper(i, inMap, outMap, visited);   
            }   
        }
        
        for(Integer i : inMap.getOrDefault(src, new ArrayList<>())){
            if(!visited.contains(i)){
                count += helper(i, inMap, outMap, visited);  
            }     
        }
        
        return count;
    }
       
}
```  
**Time Complexity:** O(V + E) as we visit every vertex and every edge     

**Space Complexity:** O(V + E) as we all edges and visited vertices   

