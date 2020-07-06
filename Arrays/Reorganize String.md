## Reorganize String

Given a string S, check if the letters can be rearranged so that two characters that are adjacent to each other are not the same.

If possible, output any possible result.  If not possible, return the empty string.  

### Example 1:
```
Input: S = "aab"
Output: "aba"
```


### Example 2:
```
Input: S = "aaab"
Output: ""
```

> **Note:**  
> S will consist of lowercase letters and have length in range [1, 500].   

 ### Solutioning:

```java
import java.util.Map.*;
class Item{
    char c;
    int count;
    
    public Item(char c, int count){
        this.c = c;
        this.count = count;
    }
}
class Solution {
    public String reorganizeString(String S) {
        
        if(S.length() <= 1){
            return S;
        }
        
        Map<Character, Integer> freqMap = new HashMap<>();
        for(int i=0;i<S.length();i++){
            freqMap.put(S.charAt(i), freqMap.getOrDefault(S.charAt(i), 0) + 1);
        }
        
        PriorityQueue<Item> q = new PriorityQueue<>((i1, i2) -> i2.count - i1.count);
        for(Entry<Character, Integer> e: freqMap.entrySet()){
            q.add(new Item(e.getKey(), e.getValue()));
        }

        
        String result = "";
        while(q.size() >= 2){      
            Item i1 = q.poll();
            Item i2 = q.poll();
            
            result = result + i1.c + i2.c;
            i1.count--;
            i2.count--;
            
            if(i1.count > 0){
                q.add(i1);
            }
            if(i2.count > 0){
                q.add(i2);
            }
            
        }
        
        if(q.size() == 1){
            Item i1 = q.poll();
            
            if(i1.count == 1){
                result = result + i1.c;
            }else{
                return "";
            }
        }
        
        return result;
    }
}
```  
**Time Complexity:** O(N*log(A)) where N is the length of S, and A is the size of the alphabet. If A is fixed, this complexity is O(N).  

**Space Complexity:**  O(A) As A is fixed, this complexity is O(1). 

