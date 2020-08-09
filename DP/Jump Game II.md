## Jump Game II

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.


### Example 1:
```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```


> **NOTE:**  
> You can assume that you can always reach the last index.  
 

### Solutioning
```java
class Solution {
    public int jump(int[] nums) {
        
        //Populate arrays specifying  
        //whether a location is reachable from start 
        boolean[] reachableFromStart = new boolean[nums.length];
        int[] minJumpsFromStart = new int[nums.length];
        
        reachableFromStart[0] = true;
        minJumpsFromStart[0] = 0;
        
        for(int i=1; i<nums.length; i++){
            minJumpsFromStart[i] = Integer.MAX_VALUE;
            
            for(int j=i-1; j>=0; j--){
                if(reachableFromStart[j] && nums[j] >= i - j){
                    reachableFromStart[i] = true;
                    minJumpsFromStart[i] = 
                        Math.min(minJumpsFromStart[i], minJumpsFromStart[j] + 1);
                }
            }
        }
       
        return minJumpsFromStart[nums.length-1];
    }
}
```  
**Time Complexity:** O(N<sup>2</sup>)  
**Space Complexity:**  O(N)   


### Optimized Solutioning
```java
class Solution {
    public int jump(int[] nums) {
        int n = nums.length;
        
        int maxCoverageSoFar = 0;
        int lastJumpEndIndex = 0;  //endIndex that can be reached from prev jump under review
        int minJumps = 0;
        
        for(int i=0; i<nums.length; i++){
            
            if(i == n-1){
                return minJumps;
            }
            
            maxCoverageSoFar = Math.max(maxCoverageSoFar, i + nums[i]);
            
            //When we reach the end index that could be reached with prev jump
            if(i == lastJumpEndIndex){
                lastJumpEndIndex = maxCoverageSoFar;
                minJumps++;
            }
        }
        
        return minJumps;
    }
}
```
**Time Complexity:** O(N)  
**Space Complexity:**  O(1)   
