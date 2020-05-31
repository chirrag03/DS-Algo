## Subarray Sum Equals K

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

    public int countSubarraySumEqualsK(int[] nums, int k) {
        
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

**Variation : Max size subarray with sum k**  
Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.


```java
class Solution {
    public int maxSubarrayLenWithSumEqualsK(int[] nums, int k) {
        
        // Map to store number of subarrays starting from index zero having particular value of sum. 
        Map<Integer, Integer> map = new HashMap<>();
        
        //Running sum
        int sum = 0;
        
        int largestSubarraySize = 0;       
        for(int i=0;i<nums.length;i++){
            sum += nums[i];
            
            if(sum == k){
                largestSubarraySize = Math.max(largestSubarraySize, i + 1);
            }
            
            if(map.containsKey(sum-k)){
                largestSubarraySize = Math.max(largestSubarraySize, i - map.get(sum-k)); 
            }
            
            //We never update the existing index value present for a sum key
            //Only insert the current index if it was not present in the map for this sum
            map.put(sum, map.getOrDefault(sum, i));
        }
        
        return largestSubarraySize;
    }
}
```
