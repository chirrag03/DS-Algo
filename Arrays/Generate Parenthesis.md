## Generate Parenthesis

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

### Example:
```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```


```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        result.addAll(helper(n));
        return result;
    }
    
    public Set<String> helper(int n) {
        if(n==0){
            return new HashSet<>();
        }
        if(n==1){
            return new HashSet<>(Arrays.asList("()"));
        }
        
        Set<String> smallOutput = helper(n-1);
        
        Set<String> finalOutput = new HashSet<>();
        Iterator iter = smallOutput.iterator();
        
        while(iter.hasNext()){
            String str = (String) iter.next();
        
            finalOutput.add("()" + str);
            for(int j=0;j<str.length();j++){
                if(str.charAt(j) == '('){
                    finalOutput.add(str.substring(0, j+1) + "()" + str.substring(j+1));
                }
            }
        }
        return finalOutput;
    }
}
```  
Why did we take set? because for the permutations (()()) and ((())) we'lll be generating same permutations by inserting parenthesis () at the places (()(..)) and (..(())) respectively.

**Time Complexity:** If we conside parenthesis () as n=1 then we are basically computing all permutations of the same. Thus, better than O(N * N!) but slower than O(N!)   
**Space Complexity:** Remember due to duplicates this no. may slightly be less than total permutations. Sligtly less than O(N × N!)
