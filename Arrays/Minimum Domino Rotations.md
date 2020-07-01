## Minimum Domino Rotations

In a row of dominoes, A[i] and B[i] represent the top and bottom halves of the i-th domino.  (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)

We may rotate the i-th domino, so that A[i] and B[i] swap values.

Return the minimum number of rotations so that all the values in A are the same, or all the values in B are the same.

If it cannot be done, return -1.


### Example 1:
```
Input: A = [2,1,2,4,2,2], B = [5,2,6,2,3,2]
Output: 2
Explanation: 
The first figure represents the dominoes as given by A and B: before we do any rotations.
If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2.
```


### Example 2:
```
Input: A = [3,5,1,2,3], B = [3,6,3,3,4]
Output: -1
Explanation: 
In this case, it is not possible to rotate the dominoes to make one row of values equal.
```

> **Note:** 
> 1 <= A[i], B[i] <= 6  
> 2 <= A.length == B.length <= 20000.  

 ### Solutioning:


```java
class Solution {
    public int minDominoRotations(int[] A, int[] B) {
        
        //Key: No. on the domino half and Value: Set of indices at which this no. occurs
        Map<Integer, Set<Integer>> upperHalfMap = new TreeMap<>();
        Map<Integer, Set<Integer>> bottomHalfMap = new TreeMap<>();
        
        for(int i=0;i<A.length;i++){
            Set<Integer> indicesSet = upperHalfMap.getOrDefault(A[i], new HashSet<>());
            indicesSet.add(i);
            upperHalfMap.put(A[i], indicesSet);
            
            indicesSet = bottomHalfMap.getOrDefault(B[i], new HashSet<>());
            indicesSet.add(i);
            bottomHalfMap.put(B[i], indicesSet);
        }
        
        for(int i=1;i<=6;i++){
            Set<Integer> indicesSetA = upperHalfMap.getOrDefault(i, new HashSet<>());
            Set<Integer> indicesSetB = bottomHalfMap.getOrDefault(i, new HashSet<>());
            
            Set<Integer> union = new HashSet<>(indicesSetA); 
            union.addAll(indicesSetB);
            
            if(union.size() == A.length){
                Set<Integer> intersection = new HashSet<>(indicesSetA); 
                intersection.retainAll(indicesSetB);
                
                indicesSetA.removeAll(intersection);
                indicesSetB.removeAll(intersection);                
                return Math.min(indicesSetA.size(), indicesSetB.size());
            }
        }
        
        return -1;
    }
}
```  
**Time Complexity:** O(N)   
**Space Complexity:**  O(N) 

