## Implement Trie (Prefix Tree)

Implement a trie with insert, search, and startsWith methods.


### Example 1:
```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

> **Note:**  
> You may assume that all inputs are consist of lowercase letters a-z.  
> All inputs are guaranteed to be non-empty strings.  


 ### Solutioning:

```java
class TrieNode{
    char data;
    boolean isTerminal;
    Map<Character, TrieNode> children;
    
    public TrieNode(char c){
        this.data = c;
        this.children = new HashMap<>();
    }
}
class Trie {

    TrieNode root;
    
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode(' ');    
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode curr = this.root;
        
        for(int i=0;i<word.length();i++){
            Map<Character, TrieNode> childMap = curr.children;
            
            TrieNode child = childMap.get(word.charAt(i));
            if(child == null){
                TrieNode newNode = new TrieNode(word.charAt(i));
                childMap.put(word.charAt(i), newNode);
                child = newNode;
            }
            curr =  childMap.get(word.charAt(i));
        }
        
        curr.isTerminal = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode curr = this.root;
        
        for(int i=0;i<word.length();i++){
            Map<Character, TrieNode> childMap = curr.children;
            
            TrieNode child = childMap.get(word.charAt(i));
            if(child == null){
                return false;
            }
            curr =  childMap.get(word.charAt(i));
        }
        
        return curr.isTerminal;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode curr = this.root;
        
        for(int i=0;i<prefix.length();i++){
            Map<Character, TrieNode> childMap = curr.children;
            
            TrieNode child = childMap.get(prefix.charAt(i));
            if(child == null){
                return false;
            }
            curr =  childMap.get(prefix.charAt(i));
        }
        
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```  

**Complexity Analysis**  
**Insertion:        Time Complexity:** O(m) where m is the key length. **Space Complexity:** O(m)   
**Search:           Time Complexity:** O(m) where m is the key length. **Space Complexity:** O(1)   
**Search Prefix:    Time Complexity:** O(m) where m is the key length. **Space Complexity:** O(1)   
