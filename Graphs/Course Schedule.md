## Course Schedule

There are a total of numCourses courses you have to take, labeled from 0 to numCourses-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?  

### Example 1:
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```

### Example 2:
```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

> **Constraints:** 
> You may assume that there are no duplicate edges in the input prerequisites.  
> 1 <= numCourses <= 10^5  


 ### Solutioning:
DFS (Depth-First Search)  

```java
import java.util.Map.Entry;
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if(prerequisites.length == 0){
            return true;
        }
        
        //Map of Vertex and its adjVertices
        Map<Integer, List<Integer>> adjVerticesMap = new HashMap<>();
        
        //Map of Vertex and its State
        //Possible Values: 0->unvisited, 1->visited, 2->visit_completed
        Map<Integer, Integer> statesMap = new HashMap<>();
        
        
        for(int i=0;i<numCourses;i++){
            adjVerticesMap.put(i, new ArrayList<>());
            statesMap.put(i, 0);
        }
        
        for(int i=0;i<prerequisites.length;i++){
            int srcCourseNum = prerequisites[i][1];     //prerequiste course
            int destCourseNum = prerequisites[i][0];    //course
            
            List<Integer> adjVerticesOfDest = adjVerticesMap.get(destCourseNum);
            
            //Creating an edge: (prerequiste course) ---> (course)
            adjVerticesOfDest.add(srcCourseNum);
        }
        
        for(Integer srcVertex : adjVerticesMap.keySet()){
            if(statesMap.get(srcVertex) == 0){
                if(directedDFS(srcVertex, statesMap, adjVerticesMap)){
                    return false;
                }
            }
        }
        
        return true;
    }
    
    public boolean directedDFS(Integer src, Map<Integer, Integer> statesMap, Map<Integer, List<Integer>> adjVerticesMap){
        
        statesMap.put(src, 1);
        
        List<Integer> adjVertices = adjVerticesMap.get(src);
        for(int i=0;i<adjVertices.size();i++){
            Integer adjVertex = adjVertices.get(i);
            if(statesMap.get(adjVertex) == 1){
                return true;
            }
            
            boolean isCycle = directedDFS(adjVertex, statesMap, adjVerticesMap);
            if(isCycle){
                return true;
            }
        }
        
        statesMap.put(src, 2);
        return false;
    }
    
}
```  
**Time Complexity:** O(E + V) where ∣V∣ is the number of courses, and ∣E∣ is the number of dependencies.  
- It would take us V + E time complexity to build a graph in the first step.  
- Since we perform a postorder DFS traversal in the graph, we visit each vertex and each edge once and only once in the worst case, i.e. ∣E∣+∣V∣.   

**Space Complexity:** O(E + V)
- We built a graph data structure in the algorithm, which would consume ∣E∣+∣V∣ space.   
- We used a state map to keep track of the visited and completed path, which consumes ∣V∣ space.  
- Finally, the memory consumption on call stack is ∣V∣ times i.e In the worst case where all nodes chained up in a line.  
Hence the overall space complexity of the algorithm would be O(∣E∣+3⋅∣V∣)=O(∣E∣+∣V∣). 

