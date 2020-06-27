## Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

### Example 1:
```
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```


> **Clarification:**  
> You do not necessarily need to follow the above format, so please be creative and come up with different approaches yourself  

> **Note:**  
> Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.


 ### Solutioning:
Use BFS

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        List<String> list = new ArrayList<>();
        
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        
        while(!q.isEmpty()){
            TreeNode curr = q.poll();
            if(curr != null){
                list.add(String.valueOf(curr.val));
                q.add(curr.left);
                q.add(curr.right);
            }else{
                list.add("null");
            }
        }
        
        return list.stream().collect(Collectors.joining(","));
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        List<String> list = Arrays.stream(data.split(",")).collect(Collectors.toList());
        if("null".equals(list.get(0))){
            return null;
        }
        
        TreeNode root = new TreeNode(Integer.parseInt(list.get(0)));
        
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        
        int index = 1;
        while(!q.isEmpty()){
            TreeNode curr = q.poll();
            
            if("null".equals(list.get(index))){
                curr.left = null;
            }else{
                curr.left = new TreeNode(Integer.parseInt(list.get(index)));
                q.add(curr.left);
            }
            
            if("null".equals(list.get(index+1))){
                curr.right = null;
            }else{
                curr.right = new TreeNode(Integer.parseInt(list.get(index+1)));
                q.add(curr.right);
            }
            index = index + 2;
        }
        
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(N) as N/2 is the max queue size

