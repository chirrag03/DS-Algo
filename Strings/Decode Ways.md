## Decode Ways

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a non-empty string containing only digits, determine the total number of ways to decode it.


### Example 1:
```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

### Example 1:
```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```


 ### Solutioning:

```java
class Solution {
    public int numDecodings(String s) {
        if(s.length() == 0){
            return 0;
        }
        
        int[] dp = new int[s.length()];
        
        dp[s.length() - 1] = s.charAt(s.length() - 1) >= '1' 
            && s.charAt(s.length() - 1) <= '9' ? 1 : 0;
        
        for(int i=s.length()-2;i>=0;i--){
            if(s.charAt(i) >= '1' && s.charAt(i) <= '9'){
                dp[i] += dp[i+1];
                
                String subStr = s.substring(i, i+2);
                char c = (char)(Integer.parseInt(subStr) + 64);
                if(c >= 'A' && c <= 'Z'){
                    dp[i] += i+2 < dp.length ? dp[i+2] : 1;
                }
            }   
        }
       
        return dp[0];
    }

}
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(N) 

**Optimizzed Solution**  
In above Approach we are using an array dp to save the results for future. As we move ahead character by character of the given string, we look back only two steps. For calculating dp[i] we need to know dp[i-1] and dp[i-2] only. Thus, we can easily cut down our O(N) space requirement to O(1) by using only two variables to store the last two results.

```java
class Solution {
    public int numDecodings(String s) {
        if(s.length() == 0){
            return 0;
        }
              
        int prev_prev_result = -1;
        
        int prev_result = s.charAt(s.length() - 1) >= '1' 
            && s.charAt(s.length() - 1) <= '9' ? 1 : 0;
        
        for(int i=s.length()-2;i>=0;i--){
            int curr_result = 0;
            
            if(s.charAt(i) >= '1' && s.charAt(i) <= '9'){
                curr_result += prev_result;
                
                String subStr = s.substring(i, i+2);
                char c = (char)(Integer.parseInt(subStr) + 64);
                if(c >= 'A' && c <= 'Z'){
                    curr_result += prev_prev_result == -1 ? 1 : prev_prev_result;
                }
            }   
            
            prev_prev_result = prev_result;
            prev_result = curr_result;
            
        }
       
        return prev_result;
    }

}
```  

**Time Complexity:** O(N)   
**Space Complexity:** O(1) 
