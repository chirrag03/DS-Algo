## Maximum Sum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.


### Example:
```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```
 

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        int subArraySum = 0;
        
        for(int i=0;i<nums.length;i++){
            subArraySum += nums[i];
            maxSum = Math.max(maxSum, subArraySum);
            if(subArraySum < 0){
                subArraySum = 0;
            }
        }
        
        return maxSum;
    }
}
```  
