## Count Univalue Subtrees

Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.

### Example 1:
```
Input:  root = [5,1,5,5,5,null,5]

              5
             / \
            1   5
           / \   \
          5   5   5
          
Output: 4
```


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
    public int countUnivalSubtrees(TreeNode root) {
        return helper(root, new HashSet<>());
    }
    
    public int helper(TreeNode root, Set<Integer> values) {
        if(root == null){
            return 0;
        }
        
        values.add(root.val);
        if(root.left == null && root.right == null){
            return 1;
        }
        
        int totalUniValueTrees = 0; 
        
        Set<Integer> valuesInLeft = new HashSet();
        totalUniValueTrees += helper(root.left, valuesInLeft);
        
        Set<Integer> valuesInRight = new HashSet();
        totalUniValueTrees += helper(root.right, valuesInRight);
        
        values.addAll(valuesInLeft);
        values.addAll(valuesInRight);
        
        if(values.size() == 1){
            return totalUniValueTrees + 1;
        }
        
        return totalUniValueTrees;
    }
}
```  
**Time Complexity:** O(N<sup>2</sup>) as we traverse the enitre tree and at each node add all subtree node values to a set    
**Space Complexity:** O(N) as there can be N distinct nodes  

 ### Optimized Solutioning:
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
    public int countUnivalSubtrees(TreeNode root) {
        isUniValue(root);
        return count;
    }
    
    int count = 0;
    
    public boolean isUniValue(TreeNode root) {
        if(root == null){
            return true;
        }
        
        if(root.left == null && root.right == null){
            count++;
            return true;
        }
        
        boolean isLeftUniValue = isUniValue(root.left);
        boolean isRightUniValue = isUniValue(root.right);

        if(!isLeftUniValue || !isRightUniValue){
            return false;
        }
        
        if((root.left == null || root.val == root.left.val) 
           && (root.right == null || root.val == root.right.val)){
            count++;
            return true;
        }
        
        return false;
    }
}
```

**Time Complexity:** O(N)      
**Space Complexity:** O(h)  
