## Odd Even Linked List

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

### Example:
```
Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL
```

### Example:
```
Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
```

> **Constraints:**  
> The relative order inside both the even and odd groups should remain as it was in the input.  
> The first node is considered odd, the second node even and so on ...  
> The length of the linked list is between [0, 10^4].


```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null){
            return null;
        }
        
        ListNode oddHead = null;
        ListNode oddTail = null;
        
        ListNode evenHead = null;
        ListNode evenTail = null;
        
        int count = 1;
        while(head != null){
            ListNode toBeAdded = head;
            head = head.next;
            toBeAdded.next = null;
            
            if(count % 2 == 1){
                if(oddHead == null){
                    oddHead = toBeAdded;
                    oddTail = toBeAdded;
                }else{
                    oddTail.next = toBeAdded;
                    oddTail = oddTail.next;
                }
            }else{
                if(evenHead == null){
                    evenHead = toBeAdded;
                    evenTail = toBeAdded;
                }else{
                    evenTail.next = toBeAdded;
                    evenTail = evenTail.next;
                }
            }
            count++;
        }
        
        oddTail.next = evenHead;
        return oddHead;
    }
    
}
```  
