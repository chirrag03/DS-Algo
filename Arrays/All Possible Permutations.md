## All Possible Permutations

Given a collection of distinct integers, return all possible permutations.


### Example:
```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
] 
```


```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        if(nums.length == 0){
            return new ArrayList<>();
        }
        return helper(nums, 0);
    }
 
    public List<List<Integer>> helper(int[] nums, int start) {
        
        if(start == nums.length -1){
            List<List<Integer>> result = new ArrayList<>();
            result.add(Arrays.asList(nums[start]));
            return result;
        }
        
        List<List<Integer>> finalOutput = new ArrayList<>();
        
        List<List<Integer>> smallerOutput = helper(nums, start+1);
        for(int i=0;i<smallerOutput.size();i++){
            List<Integer> smallPermutation = smallerOutput.get(i);
            
            for(int j=0;j<smallPermutation.size();j++){
                List<Integer> nextPermutation = new ArrayList<>(smallPermutation);
                nextPermutation.add(j, nums[start]);
                finalOutput.add(nextPermutation);
            }
            
            List<Integer> nextPermutation = new ArrayList<>(smallPermutation);
            nextPermutation.add(nums[start]);
            finalOutput.add(nextPermutation);
        }
        
        return finalOutput;
    }
    
}
```  
**Time Complexity:** Better than O(N × N!) and a bit slower than O(N!)   
**Space Complexity:** O(N × N!)
