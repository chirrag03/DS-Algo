## Kth Largest Element in an Array

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.


### Example 1:
```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

### Example 2:
```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

> **Note:** You may assume k is always valid, 1 ≤ k ≤ array's length.


 ### Solutioning:

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for(int i=0;i<nums.length;i++){
            if(i < k){
                q.add(nums[i]);
                continue;
            }
            
            if(nums[i] > q.peek()){
                q.poll();
                q.add(nums[i]);
            }
        }
        
        return q.poll();
    }
}
```  
**Time Complexity:** O(Nlogk)   
**Space Complexity:** O(k) 
