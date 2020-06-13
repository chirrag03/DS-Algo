## Largest Number

Given a list of non negative integers, arrange them such that they form the largest number.


### Example 1:
```
Input: [10,2]
Output: "210"
```

### Example 2:
```
Input: [3,30,34,5,9]
Output: "9534330"
```

> **Note:** 
> The result may be very large, so you need to return a string instead of an integer.

 ### Solutioning:
Consider the case [45, 9]....  
Normal sorting in decreasing order is useseless here. We need a comparison where first we compare the digit starting from the leftmost.  
It seems that decreasing lexicographic ordering is a fine choice which gives [9, 45].  

But Consider the case [95, 9]....It seems that decreasing lexicographic ordering fails as it gives [95, 9].
Thus it causes problems for sets of numbers with the same leading digit because is characters are same then the string with smaller length is smaller.   

Therefore, for each pairwise comparison during the sort, we compare the numbers achieved by concatenating the pair in both orders.  

Once the array is sorted, the most "signficant" number will be at the front. There is a minor edge case that comes up when the array consists of only zeroes, so if the most significant number is 00, we can simply return 00. Otherwise, we build a string out of the sorted array and return it.



```java
class Solution {
    public String largestNumber(int[] nums) {
        
        List<String> stringArrayList = Arrays.stream(nums)
        .mapToObj(String::valueOf)
        .sorted((s2, s1) ->(s1+s2).compareTo(s2+s1))
        .collect(Collectors.toList());

        if((stringArrayList.get(0)).charAt(0) == '0'){
            return "0";
        }
        
        return stringArrayList.stream().collect(Collectors.joining(""));
    }
}
```  
**Time Complexity:** O(n*log(n))   
Although we are doing extra work in our comparator, it is only by a constant factor. Therefore, the overall runtime is dominated by the complexity of sort.  

**Space Complexity:** O(n) additional space to store the copy of nums

