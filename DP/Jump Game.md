## Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.


### Example 1:
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

### Example 2:
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

> **Constraints:**  
> 1 <= nums.length <= 3 * 10^4
> 0 <= nums[i][j] <= 10^5
 

### Solutioning
```java
class Solution {
    
    public boolean canJump(int[] nums) {
        
        //Populate arrays specifying  
        //whether a location is reachable from start 
        //whether a location can jump to end
        boolean[] reachableFromStart = new boolean[nums.length];
        reachableFromStart[0] = true;
        
        for(int i=0; i<nums.length; i++){
            
            boolean canJumpToEnd = false;
            if(nums[i] >= nums.length - 1 - i){
                canJumpToEnd = true;
            }
            
            
            for(int j=i-1; j>=0; j--){
                if(reachableFromStart[j] && nums[j] >= i - j){
                    reachableFromStart[i] = true;
                    break;
                }
            }
            
            if(reachableFromStart[i] && canJumpToEnd){
                return true;
            }
        }
       
        return false;
    }
    
}
```  
**Time Complexity:** O(N<sup>2</sup>)  
**Space Complexity:**  O(N)   

### Optimized Solutioning
This approach is greedy.  
The idea of greedy algorithm is to pick locally optimal move at each step, that will lead to globally optimal solution.  

```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;

        int maxCoverageSoFar = 0;        
        for(int i=0; i<nums.length; i++){
            
            if(i == n-1){
                return true;
            }
            
            maxCoverageSoFar = Math.max(maxCoverageSoFar, i + nums[i]);
            
            //If the max coverage after including the current jump is till the current index i
            if(i == maxCoverageSoFar){
                return false;
            }
        }
        
        return true;
    }
}
```
**Time Complexity:** O(N)  
**Space Complexity:**  O(1)   
