## Diameter of Binary Tree 

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.


### Example:
Given a binary tree 

```
          1
         / \
        2   3
       / \     
      4   5    
```

Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

> Note: The length of path between two nodes is represented by the number of edges between them.

 ### Solutioning:

Dia of tree = max of all possible dia i.e. max{d1, d2, d3....dn}  
where possible diameter, d = treating each node as root taking sum of left and rt tree

```java
public class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;
  TreeNode() {}
  TreeNode(int val) { this.val = val; }
  TreeNode(int val, TreeNode left, TreeNode right) {
      this.val = val;
      this.left = left;
      this.right = right;
  }
}

class OptimizedSolution {
    
    int dia = 0;
    public int height(TreeNode root){
        if(root == null){
            return 0;
        }
        if(root.left == null && root.right == null){
            return 1;
        }
        
        int leftHeight = height(root.left);
        int rtHeight = height(root.right);
        
        int possibleDia = leftHeight + rtHeight;
        dia = Math.max(dia, possibleDia);
        
        return Math.max(leftHeight, rtHeight) + 1;
    }
    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null){
            return 0;
        }
        if(root.left == null && root.right == null){
            return 0;
        }
        
        height(root);
        
        return dia;
    }
}
```

```java
class Solution {

    public int height(TreeNode root){
        if(root == null){
            return 0;
        }
        if(root.left == null && root.right == null){
            return 1;
        }
        return Math.max(height(root.left), height(root.right)) + 1;
    }
    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null){
            return 0;
        }
        if(root.left == null && root.right == null){
            return 0;
        }
        return Math.max(Math.max(diameterOfBinaryTree(root.left), diameterOfBinaryTree(root.right)), height(root.left) + height(root.right));
    }
}
```  
