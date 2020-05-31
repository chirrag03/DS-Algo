## Count Subarray Sum Equals K

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

### Example:
```
Input:nums = [1,1,1], k = 2
Output: 2
```

> **Constraints:**  
> The length of the array is in range [1, 20,000].  
> The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].
 

**Possible Variations of Count subarrays with sum k:** 
* Max size subarray with sum k
* Count subarrays with sum 0
* Max size subarray with sum 0


```java
class Solution {

    public int subarraySum(int[] nums, int k) {
        
        // Map to store number of subarrays starting from index zero having particular value of sum. 
        Map<Integer, Integer> map = new HashMap<>();
               
        //Running sum
        int sum = 0;       
         
        int count = 0;
        for(int i=0;i<nums.length;i++){
            sum += nums[i];
           
            if(sum == k){
                count++;
            }
            
            if(map.containsKey(sum-k)){
                count += map.get(sum-k);
            }
            
            map.put(sum, map.getOrDefault(sum, 0)+1);
        }
        
        return count;
    }

}
```  
