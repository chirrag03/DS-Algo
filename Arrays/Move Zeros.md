## Move Zeroes

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.



### Example :
```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

> **Note:** You must do this in-place without making a copy of the array. Minimize the total number of operations.  
> **Note:** The number of nodes in the given list will be between 1 and 100.
 

```java
class Solution {
    public void moveZeroes(int[] nums) {

        int startIndex = 0;
        for(int i=0;i<nums.length;i++){
            if(nums[i] != 0){
                nums[startIndex++] = nums[i];
            }
        }
        
        for(int i=startIndex;i<nums.length;i++){
            nums[i] = 0;
        }
        
        
    }
}
```  

**Alternate Soltution: Similar to dutch flag problem**  
```java
class Solution {
    public void moveZeroes(int[] nums) {

        int nextNonZeroIndex = 0;
        
        int currIndex = 0;
        while(currIndex < nums.length){
            if(nums[currIndex] != 0){
                int temp = nums[nextNonZeroIndex];
                nums[nextNonZeroIndex] = nums[currIndex];
                nums[currIndex] = temp;
                
                nextNonZeroIndex++;
                currIndex++;
            }else{
                currIndex++;
            }
        }
        
        
    }
}
```  
