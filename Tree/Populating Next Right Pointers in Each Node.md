## Populating Next Right Pointers in Each Node

You are given a perfect binary tree where all leaves are on the same level, and every parent has two children.  

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

### Example 1:
```
      5
   /     \
  1       4
 / \     / \
4   8   3   6

Output:
      5 --> null
   /     \
  1  -->  4 --> null
 / \     / \
4-->8-->3-->6 --> null
```

> **Follow up:** 
> You may only use constant extra space.  
> Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.  


 ### Solutioning:

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        Queue<Node> q = new LinkedList<>();
        q.add(root);
        q.add(null);
                
        Node prev = null;
        
        while(!q.isEmpty()){
            Node curr = q.poll();
            
            if(curr != null){
                if(curr.left != null){
                    q.add(curr.left);
                }
                if(curr.right != null){
                    q.add(curr.right);
                }
            }else if(prev != null){
                q.add(null);
            }else{
                break;
            }
            
            if(prev != null){
                prev.next = curr;
            }
            
            prev = curr;
        }
        
        return root;
    }
}
```  
**Time Complexity:** O(N) since we process each node exactly once. Note that processing a node in this context means popping the node from the queue and then establishing the next pointers.  

**Space Complexity:** O(N) This is a perfect binary tree which means the last level contains N/2 nodes. The space complexity for breadth first traversal is the space occupied by the queue which is dependent upon the maximum number of nodes in particular level. So, in this case, the space complexity would be O(N).   


**Follow Up Optimized Iterative Solution**  

```java
class Solution {
    public Node connect(Node root) {
        
        Node leftmost = root;
        
        while(leftmost != null && leftmost.left != null && leftmost.right != null){
            
            Node curr = leftmost;
            while(curr != null){
                
                //Connect the children of curr Node
                curr.left.next = curr.right;
                
                //Connect the right child of curr to the left child of next curr
                if(curr.next != null){
                    curr.right.next = curr.next.left;
                }
                
                curr = curr.next;
            }
            
            //Move to leftmost of next level
            leftmost = leftmost.left;
        }
        
        return root;
    }
}
```  
**Time Complexity:** O(N)  
**Space Complexity:** O(1)
