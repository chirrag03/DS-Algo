## Next Permutation

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

### Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column:
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```


 ### Solutioning:

```java
class Solution {
    public void nextPermutation(int[] nums) {
        
        int n = nums.length;        
        
        //Find the first decreasing element from the right
        for(int i=n-1;i>0;i--){
            if(nums[i-1] < nums[i]){
                int firstDecreasingElement = nums[i-1];
                
                //Find the index of element that is just greater than the found element
                int j = indexOfNextGreaterElement(nums, i, n-1, firstDecreasingElement);
                
                swap(nums, i-1, j);
                reverse(nums, i, n-1);
                return;
            }
        }

        //We reach here if the entire array is in decreasing order...eg: [3, 2, 1]
        reverse(nums, 0, n-1);
    }
    
    //Returns the index of element that is just greater than x
    private int indexOfNextGreaterElement(int[] nums, int start, int end, int x){
        
        for(int i=end;i>=start;i--){
            if(nums[i] > x){
                return i;
            }
        }
        
        //This will never occur
        return -1;
    }
    
    private void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    
    private void reverse(int[] nums, int start, int end){
        
        while(start <= end){
            swap(nums, start, end);
            start++;
            end--;
        }
    }
}
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(1) 
