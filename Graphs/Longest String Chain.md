## Longest String Chain  

Given a list of words, each word consists of English lowercase letters.

Let's say word1 is a predecessor of word2 if and only if we can add exactly one letter anywhere in word1 to make it equal to word2.  For example, "abc" is a predecessor of "abac".

A word chain is a sequence of words [word_1, word_2, ..., word_k] with k >= 1, where word_1 is a predecessor of word_2, word_2 is a predecessor of word_3, and so on.

Return the longest possible length of a word chain with words chosen from the given list of words.  

### Example 1:
```
Input: ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: one of the longest word chain is "a","ba","bda","bdca".
```


> **Note:** 
> 1 <= words.length <= 1000     
> 1 <= words[i].length <= 16    
> words[i] only consists of English lowercase letters.  


 ### Solutioning:

We will essentially be working with an undirected and unweighted graph with words as nodes and edges between words by adding just one letter. The problem boils down to finding the longest path from any start node to a destination node, if there exists one. 

One of the most important step here is to figure out how to find adjacent nodes i.e. words which can be made by adding just one letter. To efficiently find the neighboring nodes for any given word we do some pre-processing on the words of the given wordList.  


```java
class Solution {
    public int longestStrChain(String[] words) {
        List<String> wordList = Arrays.stream(words).collect(Collectors.toList());
        
        Set<String> wordSet = new HashSet<>(wordList);
        Map<String, List<String>> allComboDict = new HashMap<>();

        for(String currWord : wordList){
            for(int i=0;i<=currWord.length();i++){
                for(int j=0;j<26;j++){
            
                    char subs = (char) (97+j);
                    
                    String newWord = "";
                    if(i != currWord.length()){
                        newWord = currWord.substring(0, i) + subs + 
                        currWord.substring(i, currWord.length());
                    }else{
                        newWord = currWord + subs;
                    }
                    

                    if(wordSet.contains(newWord)){
                        List<String> transformations = 
                            allComboDict.getOrDefault(currWord, new ArrayList<>());
                        
                        transformations.add(newWord);                       
                        allComboDict.put(currWord, transformations);
                    }
                }
              
            }
            
        }
        
        int max = 0;
        
        for(String currWord : wordList){
            int level = bfs(currWord, allComboDict);
            max = Math.max(max, level);
        }
        return max;
        
    }
    
    private int bfs(String beginWord, Map<String, List<String>> allComboDict){
        Queue<String> q = new LinkedList<>();
        q.add(beginWord);
        q.add(null);
        
        Map<String, Boolean> visited = new HashMap<>();
        visited.put(beginWord, true);
        
        String prev = null;
        int level = 0;
        while(!q.isEmpty()){
            String curr = q.poll();
            if(curr != null){
                
                 for (String adjacentWord : allComboDict.getOrDefault(curr, new ArrayList<>())) {
                    
                      // Otherwise, add it to the BFS Queue. Also mark it visited
                      if (!visited.containsKey(adjacentWord)) {
                        visited.put(adjacentWord, true);
                        q.add(adjacentWord);
                      }
                }
            }else if(prev != null){
                q.add(null);
                level++;
            }else{
                break;
            }
            
            prev = curr;
        }
        

        return level;
    }
}
```  
**Time Complexity:** O(26*M<sup>2</sup>*N + N<sup>2</sup>) where M is the length of each word and N is the total number of words in the input word list.  
- For each word in the word list, we iterate over its length to find all the intermediate words corresponding to it. Since the length of each word is M and we have N words, the total number of iterations the algorithm takes to create all_combo_dict is 26xM×N. Additionally, forming each of the intermediate word takes O(M) time because of the substring operation used to create the new string.  
- Breadth first search in the worst case might go to each of the N words. For each word, we need to do bfs to check if a longest path exists starting from that word. As a result, the time complexity of BFS traversal would also be O(N<sup>2</sup>)  


 ### Recursive Solutioning:
 
 ```java
 class Solution {
    public int longestStrChain(String[] words) {
        
        List<String> wordList = Arrays.stream(words).collect(Collectors.toList());
        
        Map<String, Integer> map = new HashMap<>();
        for(String currWord : wordList){
            map.put(currWord, -1);
        }
        

        int max = 0;        
        for(String currWord : wordList){
            int level = helper(currWord, map);
            max = Math.max(max, level);
        }
        return max;
        
    }
    
    private int helper(String word, Map<String, Integer> map){
    
        int level = 1;
        for(int i=0;i<word.length();i++){
            String newWord = word.substring(0, i);
            if(i+1 < word.length()){
                newWord = newWord + word.substring(i+1, word.length());
            }


            if(map.containsKey(newWord)){
                int predecessorLevel = helper(newWord, map);
                level = Math.max(level, predecessorLevel+1);
            }

        }
        
        return level;
    }
}
 ```
 **Time Complexity:** O(N<sup>2</sup>) or exponential (TO CONFIRM THIS)  
 **Space Complexity:** O(N) i.e depth of recursion

 
 
 ### Recursive Solutioning With Memomization:
 
 ```java
 class Solution {
    public int longestStrChain(String[] words) {
        List<String> wordList = Arrays.stream(words).collect(Collectors.toList());
        
        Map<String, Integer> map = new HashMap<>();
        for(String currWord : wordList){
            map.put(currWord, -1);
        }
        

        int max = 0;        
        for(String currWord : wordList){
            int level = helper(currWord, map);
            max = Math.max(max, level);
        }
        return max;
        
    }
    
    private int helper(String word, Map<String, Integer> map){
    
        if(map.get(word) != -1){
            return map.get(word);
        }
        
        int level = 1;
        for(int i=0;i<word.length();i++){
            String newWord = word.substring(0, i);
            if(i+1 < word.length()){
                newWord = newWord + word.substring(i+1, word.length());
            }


            if(map.containsKey(newWord)){
                int predecessorLevel = helper(newWord, map);
                level = Math.max(level, predecessorLevel+1);
            }

        }
        
        map.put(word, level);
        return level;
    }
}
 ```
 **Time Complexity:** O(N)   
 **Space Complexity:** O(N) i.e depth of recursion
