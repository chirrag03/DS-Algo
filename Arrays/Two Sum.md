## Two Sum

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.


### Example 1:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```


 ### Solutioning:

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            map.put(nums[i], i);
        }
        
        for(int i=0;i<nums.length;i++){
            int index = map.getOrDefault(target - nums[i], -1);
            
            if(index != -1 && index != i){
                return new int[]{i, index};
            }
        }
        
        return new int[2];   
    }
}
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(N) 

