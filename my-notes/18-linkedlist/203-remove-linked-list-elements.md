# 203 Remove Linked List Elements

Remove all elements from a linked list of integers that have value**val**.

**Example**\
**Given:**&#x31; --> 2 --> 6 --> 3 --> 4 --> 5 --> 6,**val**= 6\
**Return:**&#x31; --> 2 --> 3 --> 4 --> 5

```java
public ListNode removeElements(ListNode head, int val) {
    if (head == null) {
        return head;
    }

    ListNode dummy = new ListNode(-1);// need to delete, so we point cursor to prev node
    dummy.next = head;

    ListNode cur = dummy;
    while (cur.next != null) {
        if (cur.next.val == val) {
            cur.next = cur.next.next;
        } else {
            cur = cur.next;
        }
    }

    return dummy.next;
}
```
