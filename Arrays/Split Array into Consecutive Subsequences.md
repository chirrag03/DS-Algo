## Split Array into Consecutive Subsequences

Given an array nums sorted in ascending order, return true if and only if you can split it into 1 or more subsequences such that each subsequence consists of consecutive integers and has length at least 3.

  

### Example 1:
```
Input: [1,2,3,3,4,5]
Output: True
Explanation:
You can split them into two consecutive subsequences : 
1, 2, 3
3, 4, 5
```


### Example 2:
```
Input: [1,2,3,3,4,4,5,5]
Output: True
Explanation:
You can split them into two consecutive subsequences : 
1, 2, 3, 4, 5
3, 4, 5
```


### Example 3:
```
Input: [1,2,3,4,4,5]
Output: False
```

> **Constraints:**  
> 1 <= nums.length <= 10000  

 ### Solutioning:

```java
class Solution {
    public boolean isPossible(int[] nums) {
        
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        //Key: Ending number of a subsequence
        //Value: Count of subsequence ending with this number
        Map<Integer, Integer> endingMap = new HashMap<>();
        
        for(int num : nums){
            if(map.get(num) == 0){
                continue;
            }
            
            //Can this number be added to any existing subsequence
            if(endingMap.getOrDefault(num-1, 0) > 0){
                map.put(num, map.get(num) - 1);
                endingMap.put(num-1, endingMap.get(num-1) - 1);
                endingMap.put(num, endingMap.getOrDefault(num, 0) + 1);
                continue;
            }

            //Can a new subsequence of size 3 be created starting from this number
            if(map.get(num) > 0 && map.getOrDefault(num+1, 0) > 0 && map.getOrDefault(num+2, 0) > 0){
                map.put(num, map.get(num) - 1);
                map.put(num + 1, map.get(num + 1) - 1);
                map.put(num + 2, map.get(num + 2) - 1);
                endingMap.put(num + 2, endingMap.getOrDefault(num + 2, 0) + 1);
                continue;
            }
              
            return false;
            
        }
        
        return true;
    }
}
```  
**Time Complexity:** O(N)  
**Space Complexity:**  O(N)  

