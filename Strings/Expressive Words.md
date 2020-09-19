## Expressive Words

Sometimes people repeat letters to represent extra feeling, such as "hello" -> "heeellooo", "hi" -> "hiiii".  In these strings like "heeellooo", we have groups of adjacent letters that are all the same:  "h", "eee", "ll", "ooo".

For some given string S, a query word is stretchy if it can be made to be equal to S by any number of applications of the following extension operation: choose a group consisting of characters c, and add some number of characters c to the group so that the size of the group is 3 or more.

For example, starting with "hello", we could do an extension on the group "o" to get "hellooo", but we cannot get "helloo" since the group "oo" has size less than 3.  Also, we could do another extension like "ll" -> "lllll" to get "helllllooo".  If S = "helllllooo", then the query word "hello" would be stretchy because of these two extension operations: query = "hello" -> "hellooo" -> "helllllooo" = S.

Given a list of query words, return the number of words that are stretchy.   

### Example 1:
```
Example:
Input: 
S = "heeellooo"
words = ["hello", "hi", "helo"]
Output: 1
Explanation: 
We can extend "e" and "o" in the word "hello" to get "heeellooo".
We can't extend "helo" to get "heeellooo" because the group "ll" is not size 3 or more.
```



> **Constraints:** 
> 0 <= len(S) <= 100    
> 0 <= len(words) <= 100    
> 0 <= len(words[i]) <= 100.    
> S and all words in words consist only of lowercase letters  

 ### Solutioning:
Use RunlengthEncoding  

```java
class RunlengthEncoding{
    List<Character> charList = new LinkedList<>();
    List<Integer> countList = new LinkedList<>();
    
    public RunlengthEncoding(String S){
        int i=0;
        while(i < S.length()){
            char c = S.charAt(i);
            int count = 1;
            while(i+1 < S.length() && S.charAt(i) == S.charAt(i+1)){
                count++;
                i++;
            }
            i++;
            charList.add(c);
            countList.add(count);
        }
    }
}
class Solution {
    public int expressiveWords(String S, String[] words) {
        
        RunlengthEncoding encodedResult = new RunlengthEncoding(S);
        List<Character> givenCharList = encodedResult.charList;
        List<Integer> givenCountList = encodedResult.countList;
        
        int output = 0;
        for(int i=0;i<words.length;i++){
            
            RunlengthEncoding encodedResult1 = new RunlengthEncoding(words[i]);
            List<Character> charList = encodedResult1.charList;
            List<Integer> countList = encodedResult1.countList;
            
            if(isStretchy(givenCharList, givenCountList, charList, countList)){
                output++;
            }
            
        }
        
        return output;
    }
    
    private boolean isStretchy(List<Character> givenCharList, List<Integer> givenCountList, 
                               List<Character> charList, List<Integer> countList){
        
        if(givenCharList.size() != charList.size()){
            return false;
        }

        for(int j=0;j<givenCharList.size();j++){

            if(givenCharList.get(j) != charList.get(j)){
                return false;
            }

            if(givenCountList.get(j) < countList.get(j)){
                return false;
            }

            if(givenCountList.get(j) != countList.get(j) && givenCountList.get(j) < 3){
                return false;
            }
        }

        return true;
    }
}
```  
**Time Complexity:** O(N*k) where N is the total number of words and k is the max length of a word.   
**Space Complexity:**  O(k)

