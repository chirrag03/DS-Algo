## Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Example 1:
```
Input: 
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }

        ListNode head = null;
        ListNode tail = null;
        
        ListNode head1 = l1;
        ListNode head2 = l2;
        
        int carry = 0;
        while(head1 != null || head2 != null){
            int sum = 0;
            if(head1 != null){
                sum += head1.val;
                head1 = head1.next;
            }
            
            if(head2 != null){
                sum += head2.val;
                head2 = head2.next;
            }
            
            sum += carry;
            
            carry = sum / 10;
            sum = sum % 10;
            
            ListNode newNode = new ListNode(sum);
            if(head == null){
                head = newNode;
                tail = newNode;
            }else{
                tail.next = newNode;
                tail = newNode;
            }
        }
        
        if(carry > 0){
            ListNode newNode = new ListNode(carry);
            tail.next = newNode;
            tail = newNode;
        }
        return head;
    }
    
}
```  
**Time Complexity:** O(max(m,n)) Assume that m and n represents the length of l1 and l2 respectively, the algorithm above iterates at most max(m,n) times.  

**Space Complexity:** O(max(m,n)) The length of the new list is at most max(m,n) + 1  


