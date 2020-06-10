## Copy List with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.


> **Constraints:**  
> -10000 <= Node.val <= 10000  
> Node.random is null or pointing to a node in the linked list.  
> Number of Nodes will not exceed 1000.  

 ### Solutioning:

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        if(head == null){
            return head;
        }
        
        Node curr = head;
        while(curr != null){
            Node temp = curr.next;
            Node newNode = new Node(curr.val);
            
            curr.next = newNode;
            newNode.next = temp;
            curr = curr.next.next;
        }
        
        curr = head;
        while(curr != null){
            if(curr.random == null){
                curr.next.random = null;
            }else{
                curr.next.random = curr.random.next;    
            }
            curr = curr.next.next;
        }
        
        Node newHead = null;
        Node newTail = null;
        
        curr = head;
        while(curr != null){
            Node extractNode = curr.next;
            curr.next = extractNode.next;
            extractNode.next = null;
            curr = curr.next;
            
            if(newHead == null){
                newHead = extractNode;
                newTail = extractNode;
            }else{
                newTail.next = extractNode;
                newTail = newTail.next;
            }
        }
        
        return newHead;
    }
}
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(1) 

