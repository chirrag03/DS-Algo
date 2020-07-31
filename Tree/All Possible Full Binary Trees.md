## All Possible Full Binary Trees

A full binary tree is a binary tree where each node has exactly 0 or 2 children.

Return a list of all possible full binary trees with N nodes.  Each element of the answer is the root node of one possible tree.

Each node of each tree in the answer must have node.val = 0.

You may return the final list of trees in any order. 


### Example 1:
```
Input: 7
Output: [
[0,0,0,null,null,0,0,null,null,0,0],
[0,0,0,null,null,0,0,0,0],
[0,0,0,0,0,0,0],
[0,0,0,0,0,null,null,null,null,0,0],
[0,0,0,0,0,null,null,0,0]
]
```

> **Note:**  
> 1 <= N <= 20  

 ### Solutioning:


```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<TreeNode> allPossibleFBT(int N) {
        if(N % 2 == 0){
            return new ArrayList<>();
        }
        
        return helper(N);
    }
    
    Map<Integer, List<TreeNode>> memo = new HashMap();
    
    private List<TreeNode> helper(int N){
        if (memo.containsKey(N)){
            return memo.get(N);
        }
        
        List<TreeNode> result =  new ArrayList<>();
        if(N <= 0){
            result.add(null);
        }else if(N == 1){
            result.add(new TreeNode(0));
        }else {
            for(int i=1;i<=N-2;i=i+2){
            
                List<TreeNode> leftSubtreeCombinations = helper(i);
                List<TreeNode> rightSubtreeCombinations = helper(N - i - 1);

                for(int j=0;j<leftSubtreeCombinations.size();j++){
                   for(int k=0;k<rightSubtreeCombinations.size();k++){
                       
                        TreeNode root = new TreeNode(0);
                        root.left = leftSubtreeCombinations.get(j);
                        root.right = rightSubtreeCombinations.get(k);
                        result.add(root);
                       
                    } 

                }

            }
        }
        
        memo.put(N, result);
        return result;
    }
}
```  

