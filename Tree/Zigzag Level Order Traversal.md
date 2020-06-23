## Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).


### Example 1:
```
Input: 
    3
   / \
  9  20
    /  \
   15   7
Output: 
[
  [3],
  [20,9],
  [15,7]
]
```

 ### Solutioning:
BFS (Breadth-First Search)

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        q.add(null);
        
        List<List<Integer>> resultList = new LinkedList<>();
        
        List<Integer> levelList = new LinkedList<>();
        TreeNode prev = null;
        
        while(!q.isEmpty()){
            TreeNode curr = q.poll();
            
            if(curr != null){
                levelList.add(curr.val);
                if(curr.left != null){
                    q.add(curr.left);
                }
                if(curr.right != null){
                    q.add(curr.right);
                }
            }else if(prev != null){
                resultList.add(levelList);
                levelList = new LinkedList<>();
                q.add(null);
            }else{
                break;
            }
            
            prev = curr;
        }
        
        for(int i=0;i<resultList.size();i++){
            List<Integer> list = resultList.get(i);
            if(i % 2 != 0){
                Collections.reverse(list);    
            }
        }
        
        return resultList;
    }
}
```  
**Time Complexity:** O(N)   

**Space Complexity:** O(N) Memory taken by the Queue. Since we have a binary tree, the level that contains the most nodes could occur to consist all the leave nodes in a full binary tree, which is roughly L= N/2  


**Optimimzed Approach**  
DFS (Depth-First Search)

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> resultList = new LinkedList<>();
        
        if(root == null){
            return resultList;
        }
       
        dfs(root, 1, resultList);
        
        return resultList;
    }
    
    public void dfs(TreeNode root, int level, List<List<Integer>> resultList) {
        
        if(resultList.size() < level){
             resultList.add(new LinkedList<>());
        }
        
        List<Integer> list = resultList.get(level-1);
        if(level % 2 != 0){
            list.add(root.val);    
        }
        if(level % 2 == 0){
            list.add(0, root.val);
        }
        
        if(root.left != null){
            dfs(root.left, level + 1, resultList);
        }
        if(root.right != null){
            dfs(root.right, level + 1, resultList);
        }

    }
}
```
**Time Complexity:** O(N)   

**Space Complexity:** O(h) where h is height of the tree  
