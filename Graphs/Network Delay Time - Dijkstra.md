## Network Delay Time - Dijkstra

There are N network nodes, labelled 1 to N.

Given times, a list of travel times as directed edges times[i] = (u, v, w), where u is the source node, v is the target node, and w is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node K. How long will it take for all nodes to receive the signal? If it is impossible, return -1.  

### Example 1:
```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2
Output: 2
```

> **Note:**  
> N will be in the range [1, 100].  
> K will be in the range [1, N].  
> The length of times will be in the range [1, 6000].  
> All edges times[i] = (u, v, w) will have 1 <= u, v <= N and 0 <= w <= 100.  

 ### Solutioning:
We use Dijkstra's algorithm to find the shortest path from our source to all targets.  
Dijkstra's algorithm is based on repeatedly making the candidate move that has the least distance travelled.

```java
import java.util.Map.*;

class Solution {
    public int networkDelayTime(int[][] times, int N, int K) {
        
        //Construct Graph Start
        Map<Integer, List<Integer[]>> graph = new HashMap<>();
        for(int i=1;i<=N;i++){
            graph.put(i, new ArrayList<>());
        }
        
        for(int i=0;i<times.length;i++){
            int src = times[i][0];
            int dest = times[i][1];
            int wt = times[i][2];
            
            graph.get(src).add(new Integer[]{dest, wt});
        }
        //Construct Graph End
        
        
        //Dijkstra Start
        PriorityQueue<Integer[]> heap = new PriorityQueue<>((i1, i2) -> (i1[1] - i2[1]));
        heap.add(new Integer[]{K, 0});
        
        Map<Integer, Integer> shortestDistMap = new HashMap<>();
        
        while(!heap.isEmpty()){
            Integer[] curr = heap.poll();
            int node = curr[0];
            int dist = curr[1];
            
            if(shortestDistMap.containsKey(node)){
                continue;
            }
            
            shortestDistMap.put(node, dist);
            for(int i=0;i<graph.get(node).size();i++){
                Integer[] adj = graph.get(node).get(i);
                int adjNode = adj[0];
                int adjDist = adj[1];
                
                if(!shortestDistMap.containsKey(adjNode)){
                    heap.add(new Integer[]{adjNode, dist + adjDist});
                }
            }
        }
        //Dijkstra End
        
        int maxTime = 0;
        if(shortestDistMap.size() == N){
            for(Entry<Integer, Integer> e : shortestDistMap.entrySet()){
                maxTime = Math.max(maxTime, e.getValue());
            }
            return maxTime;
        }
        
        return -1;

    }
}
```  
**Time Complexity:** O(ElogE) in the heap implementation, as potentially every edge gets added to the heap.  
**Space Complexity:**  O(N+E), the size of the graph O(E), plus the size of the other objects used O(N)

