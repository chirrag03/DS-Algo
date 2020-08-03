## Serialize and Deserialize N-ary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize an N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that an N-ary tree can be serialized to a string and this string can be deserialized to the original tree structure.  


> **Note:**  
> The height of the n-ary tree is less than or equal to 1000  
> The total number of nodes is between [0, 10^4]  
> Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.  


 ### Solutioning:
Use BFS

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Codec {
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if(root == null){
            return "null";    
        }
        
        List<String> list = new ArrayList<>();
        
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        list.add(String.valueOf(root.val));
        
        while(!queue.isEmpty()){
            Node curr = queue.poll();
            
            if(curr != null){
                for(int i=0;i<curr.children.size();i++){
                    queue.add(curr.children.get(i));
                    list.add(String.valueOf(curr.children.get(i).val));
                }
                queue.add(null);
                list.add("null");
            }
        }
        
        return list.stream().collect(Collectors.joining(","));
    }
	
    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        List<String> list = Arrays.stream(data.split(",")).collect(Collectors.toList());
        if(list.get(0).equals("null")){
            return null;
        }
        
        Queue<Node> queue = new LinkedList<>();
        Node root = new Node(Integer.parseInt(list.get(0)));
        queue.add(root);
        
        int index = 1;
        while(!queue.isEmpty()){
            Node curr = queue.poll();
            
            List<Node> children = new ArrayList<>();
            while(!list.get(index).equals("null")){
                Node child = new Node(Integer.parseInt(list.get(index)));
                queue.add(child);
                children.add(child);
                index++;
            }
            curr.children = children;
            index++;
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

