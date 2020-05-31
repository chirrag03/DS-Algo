## Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand (i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).  
You are given a target value to search. If found in the array return its index, otherwise return -1.  
You may assume no duplicate exists in the array.  

Your algorithm's runtime complexity must be in the order of O(log n).


### Example 1:
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

### Example 2:
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```
 

```java
class Solution {
    public int search(int[] nums, int target) {
        
        int start = 0;
        int end = nums.length - 1;
        
        while(start <= end){
            int mid = (start + end)/2;
            
            //Required because below you check with mid + 1 in some case
            if(start==end){
                return nums[mid] == target ? mid : -1;
            }
            
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > nums[start]){
                if(target >= nums[start] && target < nums[mid]){
                    end = mid - 1;
                }else{
                    start = mid + 1;
                }
            }else{
                //Else part executed when nums[mid] < nums[start]  
                //or nums[mid] == nums[start] i.e. when start == mid
            
                //These condition to be in this order o/w indexOutOfbound can occur
                if(target <= nums[end] && target >= nums[mid+1]){
                    start = mid + 1;
                }else{
                    end = mid - 1;
                }
            }
        }
        
        return -1;
    }
}
```  
