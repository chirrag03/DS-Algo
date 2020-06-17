## 3Sum

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

### Example 1:
```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```


 ### Solutioning:

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        
        Arrays.sort(nums);
        
        List<List<Integer>> result = new ArrayList<>();
        
        for(int i=0;i<nums.length;i++){
            
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            
            int target = -nums[i];
            
            int start = i+1;
            int end = nums.length - 1;
            
            while(start < end){
                
                if(start > i+1 && nums[start] == nums[start-1]){
                    start++;
                    continue;
                }
                
                if(end < nums.length - 1 && nums[end] == nums[end+1]){
                    end--;
                    continue;
                }
                
                int sum = nums[start] + nums[end];
                
                if(sum < target){
                    start++;
                }else if(sum > target){
                    end--;
                }else{
                    result.add(Arrays.asList(nums[start], nums[i], nums[end]));
                    start++;
                    end--;
                }
            }
        }
        
        return result;
    }
}
```  
**Time Complexity:** O(n*log(n) + n<sup>2</sup>) = O(n<sup>2</sup>)  
**Space Complexity:** O(log(n)) for sorting

