## Iterative Inorder Traversal

Given a binary tree, return the inorder traversal of its nodes' values.


### Example:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```


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
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null){
            return new ArrayList<>();
        }
        
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> s = new Stack<>();
        
        TreeNode currNode = root;
        
        while(currNode != null || !s.isEmpty()){
            while(currNode != null){
                s.push(currNode);
                currNode = currNode.left;
            }
            currNode = s.pop();
            result.add(currNode.val);
            currNode = currNode.right;
        }
        
        return result;
    }
}
```  
