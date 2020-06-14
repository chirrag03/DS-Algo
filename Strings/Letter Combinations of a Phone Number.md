## Letter Combinations of a Phone Number

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

### Example 1:
```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```


> **Note:** 
> Although the above answer is in lexicographical order, your answer could be in any order you want.

 ### Solutioning:

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0){
            return new ArrayList<>();
        }
        return helper(digits);
    }
    
    public List<String> helper(String digits) {
        if(digits.length() == 0){
            return new ArrayList<>(Arrays.asList(""));
        }
        
        List<String> smallerOutput = helper(digits.substring(1));
        List<String> largerOutput = new ArrayList<>();
        String mapping = getMapping(digits.substring(0, 1));
        for(int i=0;i<mapping.length();i++){
            for(String smallCombination : smallerOutput){
                largerOutput.add(mapping.charAt(i) + smallCombination);
            }
        }
        return largerOutput;
    }
    
    public String getMapping(String s){
        String[] storage = new String[]{"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        return storage[Integer.parseInt(s) - 2];
    }
}
```  
**Time Complexity:** O(3<sup>N</sup>*4<sup>M</sup>) where N is the number of digits in the input that maps to 3 letters (e.g. 2,3,4,5,6,8) and M is the number of digits in the input that maps to 4 letters (e.g. 7,9) and N+M is the total number digits in the input.  

**Space Complexity:** O(3<sup>N</sup>*4<sup>M</sup>)
