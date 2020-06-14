## Word Break

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

### Example 1:
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

### Example 2:
```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

### Example 3:
```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

> **Note:**   
> The same word in the dictionary may be reused multiple times in the segmentation.    
> You may assume the dictionary does not contain duplicate words.


 ### Solutioning:

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return helper(s, new HashSet<>(wordDict), new HashMap<>());
    }
    
    public boolean helper(String s, Set<String> wordDict, Map<String, Boolean> dp) {
        if(s.length() == 0){
            return true;
        }
        
        String str = "";
        for(int i=0;i<s.length();i++){
            str = str + s.charAt(i);
            if(wordDict.contains(str)){
                if(!dp.containsKey(s.substring(i+1))){
                    dp.put(s.substring(i+1), helper(s.substring(i+1), wordDict, dp));
                }
                
                if(dp.get(s.substring(i+1))){
                    return true;
                }                
            }
        }
        
        return false;
    }
}
```  

**Recursion**  
**Time Complexity:** O(n<sup>n</sup>) Consider the worst case where s="aaaaaaa" and every prefix of s is present in the dictionary of words, then the recursion tree can grow upto n<sup>n</sup>      
**Space Complexity:** O(n) The depth of the recursion tree can go upto n   

**Recursion with memoization**  
**Time Complexity:** O(n<sup>2</sup>) Size of recursion tree can go up to n<sup>2</sup>.  
**Space Complexity:** O(n) The depth of recursion tree can go up to n. 


