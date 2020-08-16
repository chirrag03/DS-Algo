## Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

### Example 1:
```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

### Example 2:
```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### Example 3:
```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```


 ### Solutioning:

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int max = 0;
        int start = 0;
        
        Set<Character> set = new HashSet<>();
        
        for(int i=0;i<s.length();i++){
            if(!set.contains(s.charAt(i))){
                max = Math.max(max, i - start + 1);
                set.add(s.charAt(i));
            }else{
                while(set.contains(s.charAt(i))){
                    set.remove(s.charAt(start));
                    start++;
                }
                set.add(s.charAt(i));
            }
        }
                        
        return max;
    }
}
```  
**Time Complexity:** O(2n) = O(n) In the worst case each character will be visited twice by start and end   

**Space Complexity:** O(min(m,n))  The size of the Set is upper bounded by the size of the string n and the size of the charset/alphabet m.

