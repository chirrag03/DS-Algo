## Accounts Merge

Given a list accounts, each element accounts[i] is a list of strings, where the first element accounts[i][0] is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails in sorted order. The accounts themselves can be returned in any order.  


### Example 1:
```
Input: 
accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]

Output: [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]

Explanation: 
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
```

> **Note:**  
> The length of accounts will be in the range [1, 1000].  
> The length of accounts[i] will be in the range [1, 10].  
> The length of accounts[i][j] will be in the range [1, 30].  

 ### Solutioning:

Draw an edge between two emails if they occur in the same account. The problem comes down to finding connected components of this graph.  

For each account, draw the edge from the first email to all other emails. Additionally, we'll remember a map from emails to names on the side. After finding each connected component using a depth-first search, we'll add that to our answer.


```java
import java.util.Map.*;

class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        
        Map<String, List<String>> graph = new HashMap<>();
        Map<String, String> emailNameMap = new HashMap<>();
                
        for(List<String> account : accounts){
            String name = account.get(0);
            
            for(int i=1;i<account.size();i++){
                String email = account.get(i);
                emailNameMap.put(email, name);
                
                String u = email;
                String v = account.get(1);
    
                graph.putIfAbsent(u, new ArrayList<>());
                graph.get(u).add(v);
                
                graph.putIfAbsent(v, new ArrayList<>());
                graph.get(v).add(u);
                
            }   
        }
        
        Set<String> visited = new HashSet<>();
        
        List<List<String>> result = new ArrayList<>();
        for(String srcEmail : graph.keySet()){
            
            if(!visited.contains(srcEmail)){
                TreeSet<String> component = new TreeSet<>();
                dfs(srcEmail, graph, visited, component);
                
                List<String> list = new ArrayList<>(component);
                list.add(0, emailNameMap.get(srcEmail));
                result.add(list);
            }
            
        }
        
        return result;
    }
    
    private void dfs(String srcEmail, Map<String, List<String>> graph, 
                     Set<String> visited, TreeSet<String> component){
        
        visited.add(srcEmail);
        component.add(srcEmail);
        
        for(String destEmail : graph.get(srcEmail)){
            if(!visited.contains(destEmail)){
                dfs(destEmail, graph, visited, component);
            }
        }
    }
}
```  
**Time Complexity:** O(summation(a<sub>i</sub>*log(a<sub>i</sub>)))   
where a<sub>i</sub> is the length of accounts[i]. Without the log factor, this is the complexity to build the graph and search for each component. The log factor is for sorting each component at the end.  
  
**Space Complexity:** O(summation(a<sub>i</sub>)) the space used by our graph and our search.

