## Longest Palindromic Substring

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.


### Example 1:
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

### Example 2:
```
Input: "cbbd"
Output: "bb"
```


 ### Solutioning:  
 Expand Around Center

```java
class Solution {
    public String longestPalindrome(String s) {
        int maxLength = 0;
        int start = 0;
        int end = -1;
        
        for(int i=0;i<s.length();i++){
            int left = i;
            int right = i;
            
            while(left >= 0 && right < s.length()){
                if(s.charAt(left) == s.charAt(right) && right - left + 1 > maxLength){
                    maxLength = right - left + 1;
                    start = left;
                    end = right;
                }else if(s.charAt(left) != s.charAt(right)){
                    break;
                }
                left--;
                right++;
            }
        }
        
        for(int i=0;i<s.length()-1;i++){
            int left = i;
            int right = i+1;
            
            while(left >= 0 && right < s.length()){
                if(s.charAt(left) == s.charAt(right) && right - left + 1 > maxLength){
                    maxLength = right - left + 1;
                    start = left;
                    end = right;
                }else if(s.charAt(left) != s.charAt(right)){
                    break;
                }
                left--;
                right++;
            }
        }
        
        return s.substring(start, end+1);
    }
}
```  
**Time Complexity:** O(n<sup>2</sup>)   
**Space Complexity:** O(1) 

