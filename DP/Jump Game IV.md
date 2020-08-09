## Jump Game IV

Given an array of integers arr, you are initially positioned at the first index of the array.

In one step you can jump from index i to index:  

- **i + 1** where: i + 1 < arr.length.  
- **i - 1** where: i - 1 >= 0.  
- **j** where: arr[i] == arr[j] and i != j.   

Return the minimum number of steps to reach the last index of the array.  

Notice that you can not jump outside of the array at any time.  


### Example 1:
```
Input: arr = [100,-23,-23,404,100,23,23,23,3,404]
Output: 3
Explanation: You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.
```


### Example 2:
```
Input: arr = [7]
Output: 0
Explanation: Start index is the last index. You don't need to jump.
```


### Example 3:
```
Input: arr = [7,6,9,6,9,6,9,7]
Output: 1
Explanation: You can jump directly from index 0 to index 7 which is last index of the array.
```


### Example 4:
```
Input: arr = [6,1,9]
Output: 2
```


### Example 5:
```
Input: arr = [11,22,7,7,7,7,7,7,7,22,13]
Output: 3
```

> **Constraints:**  
> 1 <= arr.length <= 5 * 10^4  
> -10^8 <= arr[i] <= 10^8  

 

### Solutioning

BFS  

```java
class Solution {
    public int minJumps(int[] arr) {
        
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for(int i=0; i<arr.length; i++){
            Set<Integer> set = map.getOrDefault(arr[i], new HashSet<>());
            set.add(i);
            map.put(arr[i], set);
        }
        
        Queue<Integer> queue = new LinkedList<>();
        Queue<Integer> stepsQueue = new LinkedList<>();
        Set<Integer> visited = new HashSet<>();
        
        queue.add(0);
        stepsQueue.add(0);
        visited.add(0);
        
        while(!queue.isEmpty()){
            int currIndex = queue.poll();
            int currSteps = stepsQueue.poll();
            if(currIndex == arr.length-1){
                return currSteps;
            }
            
            int prevIndex = currIndex - 1;
            int nextIndex = currIndex + 1;
            
            if(prevIndex >= 0 && !visited.contains(prevIndex)){
                queue.add(prevIndex);
                stepsQueue.add(currSteps+1);
                visited.add(prevIndex);
            }
            
            if(nextIndex < arr.length && !visited.contains(nextIndex)){
                queue.add(nextIndex);
                stepsQueue.add(currSteps+1);
                visited.add(nextIndex);
            }
            
            Set<Integer> set = map.get(arr[currIndex]);
            set.removeAll(visited);
            set.remove(currIndex);
            for(Integer index : set){
                if(!visited.contains(index)){
                    queue.add(index);
                    stepsQueue.add(currSteps+1);
                    visited.add(index);
                }
            }
        }
        
        return -1;
    }
}
```
**Time Complexity:** O(N)  
**Space Complexity:**  O(N)   
