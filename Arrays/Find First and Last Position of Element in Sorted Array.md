## Find First and Last Position of Element in Sorted Array

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].


### Example 1:
```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

### Example 2:
```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```


 ### Solutioning:

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        return new int[]{
            findFirst(nums, target, 0, nums.length-1),
            findLast(nums, target, 0, nums.length-1)
        };
    }
    
    public int findFirst(int[] nums, int target, int startIndex, int endIndex){
        if(startIndex > endIndex){
            return -1;
        }
        
        if(target < nums[startIndex] || target > nums[endIndex]){
            return -1;
        }
        
        int mid = (startIndex + endIndex) / 2;
        if(target == nums[mid]){
            int leftmost = findFirst(nums, target, startIndex, mid - 1);
            if(leftmost != -1){
                return leftmost;
            }
            return mid;
        }else if(target < nums[mid]){
            return findFirst(nums, target, startIndex, mid - 1);
        }else{
            return findFirst(nums, target, mid + 1, endIndex);   
        }
    }
        
    public int findLast(int[] nums, int target, int startIndex, int endIndex){
        if(startIndex > endIndex){
            return -1;
        }
        
        if(target < nums[startIndex] || target > nums[endIndex]){
            return -1;
        }
        
        int mid = (startIndex + endIndex) / 2;
        if(target == nums[mid]){
            int rightmost = findLast(nums, target, mid + 1, endIndex);
            if(rightmost != -1){
                return rightmost;
            }
            return mid;
        }else if(target < nums[mid]){
            return findLast(nums, target, startIndex, mid - 1);
        }else{
            return findLast(nums, target, mid + 1, endIndex);   
        }
    }
}
```  
**Time Complexity:** O(log(n))   
**Space Complexity:** O(1) 

**Do you know**  
Doing int mid = (lo + hi) / 2; is prone to overflow. Instead int mid = lo + (hi - lo) / 2.

