## Find Duplicate Subtrees

Given a binary tree, return all duplicate subtrees. For each kind of duplicate subtrees, you only need to return the root node of any one of them.  

Two trees are duplicate if they have the same structure with same node values.

### Example 1:
```
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
```
The following are two duplicate subtrees:

```
      2
     /
    4
```

```
    4
```
Therefore, you need to return above trees' root in the form of a list.


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
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        if(root == null){
            return new ArrayList<>();
        }
        
        List<TreeNode> result = new ArrayList<>();
        helper(root, new HashMap<>(), result);
        
        return result;
    }
    
    private String helper(TreeNode node, Map<String, Integer> map, List<TreeNode> result){
        if(node == null){
            return "#";
        }
 
        String str = node.val + "," + 
            helper(node.left, map, result) + "," + helper(node.right, map, result); 
        
        map.put(str, map.getOrDefault(str, 0) + 1);
        
        if(map.get(str) == 2){
            result.add(node);
        }
        
        return str;
    }
}
```  
**Time Complexity:** O(N) as we traverse the enitre tree  
**Space Complexity:** O(N) as there can be N subtrees  

 ### Alternate Solutioning:

In above code the string key length in the map is of the number of nodes in the subtree.  
We can bring it down to a constant length. We will assign a uuid to each subtree and now the key in map will be root.val + leftsubtree uuid + rightsubtree uuid


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
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        if(root == null){
            return new ArrayList<>();
        }
        
        List<TreeNode> result = new ArrayList<>();
        helper(root, new HashMap<>(), new HashMap<>(), result);
        
        return result;
    }
    
    int uuid = 1;
    
    private String helper(TreeNode node, Map<String, Integer> subtreeUUIDmap, Map<Integer, Integer> subtreeCountmap, List<TreeNode> result){
        
        if(node == null){
            return "#";
        }
 
        String str = node.val + "," + 
            helper(node.left, subtreeUUIDmap, subtreeCountmap, result) + "," + 
            helper(node.right, subtreeUUIDmap, subtreeCountmap, result); 
        
        subtreeUUIDmap.putIfAbsent(str, uuid++);
        
        int subtreeUUID = subtreeUUIDmap.get(str);
        subtreeCountmap.put(subtreeUUID, subtreeCountmap.getOrDefault(subtreeUUID, 0) + 1);
        
        if(subtreeCountmap.get(subtreeUUID) == 2){
            result.add(node);
        }
        
        return String.valueOf(subtreeUUID);
    }
}
```
