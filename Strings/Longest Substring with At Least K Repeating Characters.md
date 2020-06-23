## Longest Substring with At Least K Repeating Characters

Find the length of the longest substring <b>T</b> of a given string (consists of lowercase letters only) such that every character in <b>T</b> appears no less than k times.


### Example 1:
```
Input:
s = "aaabb", k = 3

Output:
3

The longest substring is "aaa", as 'a' is repeated 3 times.
```

### Example 1:
```
Input:
s = "ababbc", k = 2

Output:
5
```


 ### Solutioning:

```java
import java.util.Map.Entry;

class Solution {
    public int longestSubstring(String s, int k) {
        
        if(s.length() < k){
            return 0;
        }
        
        Map<Character, Integer> map = new HashMap<>();
        for(int i=0;i<s.length();i++){
            int charCount = map.getOrDefault(s.charAt(i), 0) + 1;
            map.put(s.charAt(i), charCount);
        }
        
        Set<Character> badCharSet = new HashSet<>();
        for(Entry<Character, Integer> e : map.entrySet()){
            if(e.getValue() < k){
                badCharSet.add(e.getKey());
            }
        }
        
        if(badCharSet.size() == 0){
            return s.length();
        }
        
        int max = 0;
        int start = 0;
        for(int i=0;i<s.length();i++){            
            if(badCharSet.contains(s.charAt(i))){
                max = Math.max(max, longestSubstring(s.substring(start, i), k));
                start = i+1;
            }
        }
        
        max = Math.max(max, longestSubstring(s.substring(start, s.length()), k));
        return max;
    }
}
```  
