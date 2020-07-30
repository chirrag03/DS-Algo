## Split Array Largest Sum

Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to **minimize** the **largest sum** among these m subarrays.


### Example 1:
```
Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```

> **Constraints:**  
> If n is the length of array, assume the following constraints are satisfied:  
> 1 ≤ n ≤ 1000  
> 1 ≤ m ≤ min(50, n)  


 ### Solutioning:

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        
        long max = Integer.MIN_VALUE;
        long sum = 0;
        for(int i : nums){
            sum += i;
            max = Math.max(max, i);
        }
                
        long start = max;
        long end = sum;
        
        //Least weight capacity will be between start and end 
        long result = end;
        
        while(start <= end){
            long mid = end + (start - end) / 2;
            
            if(canSplitInArrays(nums, m, mid)){
                result = mid;
                end = mid - 1;
            }else{
                start = mid + 1;
            }
        }
        
        return (int) result;
    }
    
    //Return true if the nums can be splitted in M parts using the given maxSum
    private boolean canSplitInArrays(int[] nums, int m, long maxSum) {
        
        int splits = 1;
        long currSum = 0;
        for(int i=0;i<nums.length;i++){
            currSum += nums[i];
            if(currSum > maxSum){
                currSum = nums[i];
                splits++;
            }
        }
                
        return splits <= m;
    }
}
```  
**Time Complexity:** O(n*log(R)) where R is the range of numbers  
**Space Complexity:** O(log(R)) stack space  

