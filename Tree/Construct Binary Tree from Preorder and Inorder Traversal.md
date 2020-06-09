## Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.


### Example 1:
```
Input: 
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]

Output: 
    3
   / \
  9  20
    /  \
   15   7
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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return constructTree(preorder, inorder, 0, preorder.length-1, 0, inorder.length-1);
    }
    
    private TreeNode constructTree(int[] preorder, int[] inorder, int preStart, int preEnd, int inStart, int inEnd){
        if(preStart > preEnd){
            return null;
        }
        
        int rootData  = preorder[preStart];
        int rootIndex = -1;
        for(int i=0;i<inorder.length;i++){
            if(rootData == inorder[i]){
                rootIndex = i;
                break;
            }
        }
        
        int countLeftSubtreeElements = rootIndex - inStart;
        int countRtSubtreeElements = inorder.length - 1 - rootIndex;
        
        TreeNode root = new TreeNode(rootData);
        
        root.left = constructTree(preorder, inorder, preStart + 1, preStart + countLeftSubtreeElements, inStart, rootIndex - 1);
        
        root.right = constructTree(preorder, inorder, preStart + countLeftSubtreeElements + 1, preEnd, rootIndex + 1, inEnd);
        
        return root;
    }
}
``` 

For finding the rootIndex, we can convert inorder array to a Map<Integer, Integer> where key is the treeData and value is the index and pass this as an argument. As the elements are unique, this will enable us to find the rootIndex in O(1) time.
Then the complexity will be:  

**Time Complexity:** O(N)   
**Space Complexity:** O(N) 
