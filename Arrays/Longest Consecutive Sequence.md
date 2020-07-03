## Longest Consecutive Sequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in O(n) complexity.

### Example 1:
```
Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```


 ### Solutioning:


```java
class Solution {
    public int longestConsecutive(int[] nums) {
        
        Set<Integer> set = new HashSet<>();
        for(int i=0;i<nums.length;i++){
            set.add(nums[i]);
        }
        
        int maxLength = 0;
        for(int i=0;i<nums.length;i++){
            int length = set.contains(nums[i]) ? 1 : 0;
            
            int next = nums[i] + 1;
            while(set.contains(next)){
                length++;
                set.remove(next);
                next++;
            }
            
            int prev = nums[i] - 1;
            while(set.contains(prev)){
                length++;
                set.remove(prev);
                prev--;
            }
            maxLength = Math.max(maxLength, length);
        }
        
        return maxLength;
    }
    
}
```  
**Time Complexity:** O(N)  
**Space Complexity:**  O(N) 

