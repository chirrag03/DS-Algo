## Construct Binary Search Tree from Preorder Traversal

Return the root node of a binary search tree that matches the given preorder traversal.
It's guaranteed that for the given test cases there is always possible to find a binary search tree with the given requirements.


### Example 1:
```
Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]

            8
          /   \
         5     10
       /   \      \
      1     7      12
```


> **Constraints:**  
> 1 <= preorder.length <= 100  
> 1 <= preorder[i] <= 10^8  
> The values of preorder are distinct.
 

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
        
    public TreeNode bstFromPreorder(int[] preorder) {
        return constructTree(preorder, 0, preorder.length - 1);
    }
    
    public TreeNode constructTree(int[] preorder, int startIndex, int endIndex){
        if(startIndex > endIndex){
            return null;
        }
        
        TreeNode root = new TreeNode(preorder[startIndex]);
        startIndex++;

        int index=-1;
        for(int i=startIndex ; i<=endIndex; i++){
            if(preorder[i] > root.val){
                index=i;
                break;
            }
        }
        
        if(index != -1){
            root.left = constructTree(preorder, startIndex, index-1);
            root.right = constructTree(preorder, index, endIndex);
        }else{
            root.left = constructTree(preorder, startIndex, endIndex);
        }

        return root;
    }
    
}
```  
