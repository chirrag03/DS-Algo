## Move Zeros Ones Twos

Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: You are not suppose to use the library's sort function for this problem.


### Example 1:
```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```



> **Follow up:**  
> A rather straight forward solution is a two-pass algorithm using counting sort. First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.  
> Could you come up with a one-pass algorithm using only constant space?

 ### Solutioning:
Let's use here three pointers to track the rightmost boundary of zeros, the leftmost boundary of twos and the current element under the consideration. The idea of solution is to move curr pointer along the array, if nums[curr] = 0 - swap it with nums[p0], if nums[curr] = 2 - swap it with nums[p2].

Algorithm

- Initialise the rightmost boundary of zeros : p0 = 0. During the algorithm execution nums[idx < p0] = 0.
- Initialise the leftmost boundary of twos : p2 = n - 1. During the algorithm execution nums[idx > p2] = 2.
- Initialise the index of current element to consider : curr = 0.
- While curr <= p2 :  
If nums[curr] = 0 : swap currth and p0th elements and move both pointers to the right.  
If nums[curr] = 2 : swap currth and p2th elements. Move pointer p2 to the left.  
If nums[curr] = 1 : move pointer curr to the right.  


```java
class Solution {
    public void sortColors(int[] nums) {
        int nextZeroIndex = 0;
        int nextTwoIndex = nums.length - 1;
        
        int currIndex = 0;
        while(currIndex <= nextTwoIndex){
            if(nums[currIndex] == 0){
                int temp = nums[nextZeroIndex];
                nums[nextZeroIndex] = nums[currIndex];
                nums[currIndex] = temp;
                
                nextZeroIndex++;
                currIndex++;
            }else if(nums[currIndex] == 2){
                int temp = nums[nextTwoIndex];
                nums[nextTwoIndex] = nums[currIndex];
                nums[currIndex] = temp;
                
                nextTwoIndex--;
            }else{
                currIndex++;
            }
        }
        
    }
}
```  
**Time Complexity:** O(n)   **single pass  
**Space Complexity:** O(1) 
