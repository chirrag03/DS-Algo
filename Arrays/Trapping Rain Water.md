## Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.
  

### Example 1:
```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```
 
 ### Optimized Solutioning:
Using 2 pointer approach

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        
        if(n == 0){
            return 0;
        }
        
        int left = 0;
        int right = n-1;
        
        int leftMax = 0;
        int rightMax = 0;
        
        int total = 0;
        while(left <= right){
            if(height[left] < height[right]){
                if(leftMax > height[left]){
                    total += leftMax - height[left];
                }else{
                    leftMax = height[left];
                }
                left++;
            }else{
                if(rightMax > height[right]){
                    total += rightMax - height[right];
                }else{
                    rightMax = height[right];
                }
                right--;
            }
        }
        
        return total;
    }
}
```  
**Time Complexity:** O(N)  
**Space Complexity:**  O(1) 


 ### Solutioning:

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        
        if(n == 0){
            return 0;
        }
        
        int[] leftMax = new int[n];
        int[] rightMax = new int[n];
        
        leftMax[0] = -1;
        for(int i=1;i<n;i++){
            leftMax[i] = Math.max(height[i-1], leftMax[i-1]);
        }
        
        rightMax[n-1] = -1;
        for(int i=n-2;i>=0;i--){
            rightMax[i] = Math.max(height[i+1], rightMax[i+1]);
        }
        
        int total = 0;
        for(int i=0;i<n;i++){
            if(leftMax[i] != -1 && rightMax[i] != -1 
               && height[i] < leftMax[i] && height[i] < rightMax[i]){
                total += Math.min(leftMax[i], rightMax[i]) - height[i];
            }
        }
        
        return total;
    }
}
```  
**Time Complexity:** O(N)  
**Space Complexity:**  O(N) 

