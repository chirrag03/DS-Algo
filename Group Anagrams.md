## Group Anagrams

Given an array of strings, group anagrams together.


### Example :
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

> **Note:**  
> All inputs will be in lowercase.  
> The order of your output does not matter.  
 

```java
import java.util.*;
import java.util.Map.Entry;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (int i=0;i<strs.length;i++){
            char[] chars = strs[i].toCharArray();
            Arrays.sort(chars);
            String sortedStr = new String(chars);
            
            if(!map.containsKey(sortedStr)){
                List<String> anagramList = new ArrayList<>();
                anagramList.add(strs[i]);
                map.put(sortedStr, anagramList);
            } else {
                List<String> anagramList = map.get(sortedStr);
                anagramList.add(strs[i]);
                map.put(sortedStr, anagramList);
            }
        }
        
        List<List<String>> output = new ArrayList<>();
        for (Entry<String, List<String>> e : map.entrySet()){
            output.add(e.getValue());
        }
        return output;
    }
}
```  
