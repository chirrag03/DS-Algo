## First Missing Positive

Given an unsorted integer array, find the smallest missing positive integer.


### Example 1:
```
Input: [1,2,0]
Output: 3
```

### Example 2:
```
Input: [3,4,-1,1]
Output: 2
```


### Example 3:
```
Input: [7,8,9,11,12]
Output: 1
```

> **Note:**  
> Your algorithm should run in O(n) time and uses constant extra space.   


 ### Solutioning:


```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        
        //Assume that starting from the lastIndex all numbers are less than or equal to 0
        int lastIndex = nums.length;
        
        //Shifting all numbers (less than or equal to 0) to the end of array 
        int j = 0;
        while(j < lastIndex){
            if(nums[j] <= 0){
                int temp = nums[j];
                nums[j] = nums[lastIndex-1];
                nums[lastIndex-1] = temp;
                lastIndex--;
            }else{
                j++; 
            }
        }
        
        if(lastIndex == 0){
            return 1;
        }
        
        //Now here we know that between start and end index all numbers are greater than equal to 1
        int start = 0;
        int end = lastIndex - 1;
        
        int min = Integer.MAX_VALUE;
        
        for(int i=start;i<=end;i++){
            min = Math.min(min, nums[i]);
        }
        
        if(min > 1){
            return 1;
        }
        
        //We reach here only if min is 1
        for(int i=start;i<=end;i++){
            int index = Math.abs(nums[i]) - 1;
            if(index <= end && nums[index] > 0){
                nums[index] = - nums[index];    
            }
        }
        
        int firstPositiveIndex = -1;
        for(int i=start;i<=end;i++){
            if(nums[i] > 0){
                firstPositiveIndex = i;
                break;
            }
        }
        if(firstPositiveIndex == -1){
            firstPositiveIndex = end+1;
        }
        
        return firstPositiveIndex + 1;
    }
}`
```  
**Time Complexity:** O(n)   
**Space Complexity:** O(1) 

