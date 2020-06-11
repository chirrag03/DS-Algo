## Remove Nth Node From End of List

Given a linked list, remove the n-th node from the end of list and return its head.


### Example 1:
```
Given linked list: 1->2->3->4->5, and n = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.
```


> **Note:**  
> Given n will always be valid.  
> Could you do this in one pass?  


 ### Solutioning:

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode curr = head;
        
        while(n > 0){
            curr = curr.next;
            n--;
        }
        
        ListNode prev = null;
        ListNode slow = head;
        ListNode fast = curr;
        while(fast != null){
            prev = slow;
            slow = slow.next;
            fast = fast.next;
        }
        
        if(prev == null){
            return head.next;
        }
        
        prev.next = slow.next;
        return head;
    }
}
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(1) 


