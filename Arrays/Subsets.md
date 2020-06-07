## Subsets

Given a set of distinct integers, nums, return all possible subsets (the power set).  

**Note:** The solution set must not contain duplicate subsets.

### Example 1:
```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```


 ### Solutioning:
For a given number, it could be present or absent (i.e. binary choice) in a subset solution. As as result, for N numbers, we would have in total 2^N choices (solutions).   

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        return helper(nums, 0);
    }
    
    public List<List<Integer>> helper(int[] nums, int start) {
        if(start == nums.length){
            List<List<Integer>> output = new ArrayList<>();
            output.add(new ArrayList<>());
            return output; 
        }
        
        
        List<List<Integer>> smallerOutput = helper(nums, start+1);
        
        List<List<Integer>> output = new ArrayList<>();
        for(int i=0;i<smallerOutput.size();i++){
            List<Integer> possibleSubset1 = smallerOutput.get(i);
            
            List<Integer> possibleSubset2 = new ArrayList<>(possibleSubset1);
            possibleSubset2.add(nums[start]);
            
            output.add(possibleSubset1);
            output.add(possibleSubset2);
            
        }
        
        return output;
    }
    
}
```  
**Time Complexity:** O(N * 2^N) to generate all subsets and then copy them into output list.  
**Space Complexity:** O(N * 2^N) i.e exactly the no. of solutions for subsets multiplied by the no. N of elements to keep for each subset.
