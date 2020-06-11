## Increasing Triplet Subsequence

Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:  
> Return true if there exists i, j, k  
> such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.  

**Note:** Your algorithm should run in O(n) time complexity and O(1) space complexity.


### Example 1:
```
Input: [1,2,3,4,5]
Output: true
```

### Example 1:
```
Input: [5,4,3,2,1]
Output: false
```


 ### Solutioning:  
As we want to find an increasing subsequence of length 3, we can just   
1. Track potential candidates   
I use variable 'a' and 'b' to represent the first and second element respectively of the best potential candidate we found.   
If we find an element greater than 'b', then there is an Increasing Triplet Subsequence.  
    
2. Try to find better candidates   
If we traverse to an element('c') less than 'a' then its a better candidate for 'a' and hence we replace 'a' with 'c'.  
If we traverse to an element('c') greater than 'a' but smaller than 'b', then its a better candidate for 'b' and hence we replace 'b' with 'c'.   
Then, we get a better candidate as it has a more loose requirement to become an Increasing Triplet Subsequence.  

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums.length < 3){
            return false;
        }
        
        int a = Integer.MAX_VALUE;
        int b = Integer.MAX_VALUE;
        
        //If we find some element such that a < b < nums[i] then return true:
        for(int i=0;i<nums.length;i++){
            if(nums[i] < a){
                a = nums[i];
            }else if(nums[i] > a && nums[i] < b){
                b = nums[i];
            }else if(nums[i] > b){
                return true;
            }
        }
        
        return false;
    }
}
```  
**Time Complexity:** O(n)   
**Space Complexity:** O(1) 

