## Word Ladder

Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time.
Each transformed word must exist in the word list.  

### Example 1:
```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```


### Example 2:
```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

> **Follow up:** 
> Return 0 if there is no such transformation sequence.  
> All words have the same length.  
> All words contain only lowercase alphabetic characters.  
> You may assume no duplicates in the word list.  
> You may assume beginWord and endWord are non-empty and are not the same.  

 ### Solutioning:
We are given a beginWord and an endWord. Let these two represent start node and end node of a graph. We have to reach from the start node to the end node using some intermediate nodes/words. The intermediate nodes are determined by the wordList given to us. The only condition for every step we take on this ladder of words is the current word should change by just one letter.

We will essentially be working with an undirected and unweighted graph with words as nodes and edges between words which differ by just one letter. The problem boils down to finding the shortest path from a start node to a destination node, if there exists one. Hence it can be solved using Breadth First Search approach.

One of the most important step here is to figure out how to find adjacent nodes i.e. words which differ by one letter. To efficiently find the neighboring nodes for any given word we do some pre-processing on the words of the given wordList.  


```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        wordList.add(beginWord);
        Set<String> wordSet = new HashSet<>(wordList);
        
        Map<String, List<String>> allComboDict = new HashMap<>();

        for(String currWord : wordList){
            for(int i=0;i<currWord.length();i++){
                for(int j=0;j<26;j++){
            
                    char subs = (char) (97+j);
                    if(subs == currWord.charAt(i)){
                        continue;
                    }
                    String newWord = currWord.substring(0, i) + subs + 
                        currWord.substring(i + 1, currWord.length());

                    if(wordSet.contains(newWord)){
                        List<String> transformations = 
                            allComboDict.getOrDefault(currWord, new ArrayList<>());
                        
                        transformations.add(newWord);                       
                        allComboDict.put(currWord, transformations);
                    }
                }
              
            }
            
        }
        
        Queue<String> q = new LinkedList<>();
        q.add(beginWord);
        q.add(null);
        
        Map<String, Boolean> visited = new HashMap<>();
        visited.put(beginWord, true);
        
        String prev = null;
        int level = 1;
        while(!q.isEmpty()){
            String curr = q.poll();
            if(curr != null){
                
                 for (String adjacentWord : allComboDict.getOrDefault(curr, new ArrayList<>())) {
                      // If at any point if we find what we are looking for
                      // i.e. the end word - we can return with the answer.
                      if (adjacentWord.equals(endWord)) {
                        return level + 1;
                      }
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
        

        return 0;
    }
}
```  
**Time Complexity:** O(26*M<sup>2</sup>*N) where M is the length of each word and N is the total number of words in the input word list.  
- For each word in the word list, we iterate over its length to find all the intermediate words corresponding to it. Since the length of each word is M and we have N words, the total number of iterations the algorithm takes to create all_combo_dict is 26xM×N. Additionally, forming each of the intermediate word takes O(M) time because of the substring operation used to create the new string.  
- Breadth first search in the worst case might go to each of the N words. For each word, we need to examine M possible intermediate words/combinations. As a result, the time complexity of BFS traversal would also be O(M<sup>2</sup>*N)  

**Space Complexity:**  O(M*N) As Each word in the word list would have M intermediate combinations.

