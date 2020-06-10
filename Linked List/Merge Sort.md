## Merge Sort 

Sort a linked list in O(n log n) time using constant space complexity.

### Example 1:
```
Input: 4->2->1->3
Output: 1->2->3->4
```

### Example 2:
```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```


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
    public ListNode sortList(ListNode head) {
        if(head == null){
            return null;
        }
        if(head.next == null){
            return head;
        }

        ListNode slow = head;
        ListNode fast = head;
        
        while(fast != null && fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode mid = slow;
        ListNode secondHalfHead = slow.next;
        mid.next = null;
        
        ListNode sortedFirstHalf = sortList(head);
        ListNode sortedSecondHalf = sortList(secondHalfHead);

        return merge(sortedFirstHalf, sortedSecondHalf);
    }
    
    public ListNode merge(ListNode head1, ListNode head2) {
        if(head1 == null){
            return head2;
        }
        if(head2 == null){
            return head1;
        }

        ListNode head = null;
        ListNode tail = null;
        
        while(head1 != null && head2 != null){
            ListNode extractNode = null;
            
            if(head1.val < head2.val){
                extractNode = head1;
                head1 = head1.next;
            }else{
                extractNode = head2;
                head2 = head2.next;
            }
            
            if(head == null){
                head = extractNode;
                tail = extractNode;
            }else{
                tail.next = extractNode;
                tail = tail.next;
            }
        }
        
        if(head1 != null){
            tail.next = head1;
        }
        if(head2 != null){
            tail.next = head2;
        }
        
        return head;
    }
}
```  
**Time Complexity:** O(n log n)   
**Space Complexity:** O(1) 

