## Decode String

Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

### Example 1:
```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

### Example 2:
```
Input: s = "3[a2[c]]"
Output: "accaccacc"
```

### Example 3:
```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

### Example 4:
```
Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"
```


 ### Solutioning:


```java
class Solution {
    public String decodeString(String s) {
        
        //Key: opening bracket index, value: closing bracket index
        Map<Integer, Integer> map = new HashMap<>();
        
        Stack<Integer> stack = new Stack<>();
        for(int i=0;i<s.length();i++){
            if(s.charAt(i) == '['){
                stack.push(i);
            }else if(s.charAt(i) == ']'){
                map.put(stack.pop(), i);
            }
        }
        
        String result = "";
        
        int i=0;
        while(i < s.length()){
            if(s.charAt(i) >= '0' && s.charAt(i) <= '9'){
                int numIndexEnd = i+1;
                while(s.charAt(numIndexEnd) >= '0' && s.charAt(numIndexEnd) <= '9'){
                    numIndexEnd++;
                }
                
                int repeat = Integer.parseInt(s.substring(i, numIndexEnd));
                
                int bracketIndexStart = numIndexEnd;
                int bracketIndexEnd = map.get(bracketIndexStart);
                
                String substrResult = decodeString(s.substring(bracketIndexStart+1, bracketIndexEnd));
                
                for(int j=0;j<repeat;j++){
                    result = result + substrResult;
                }
                
                i = bracketIndexEnd + 1;
            }else{
                result = result + s.charAt(i);
                i++;
            }
        }
        
        return result;
    }
    
}
```  
**Time Complexity:** O(N<sup>2</sup>)  

**Space Complexity:**  O(N) i.e. depth of recursion might be near to N/2

