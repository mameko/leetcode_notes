# 86 Partition List

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example**

Given`1->4->3->2->5->2->null`and x =`3`,\
return`1->2->2->4->3->5->null`.

因为是链表，所以可以拆开两条，来partition，最后再合成一条。

```java
public ListNode partition(ListNode head, int x) {
    if (head == null) {
        return null;
    }

    ListNode leftHead = new ListNode(-1);
    ListNode leftcur = leftHead;
    ListNode rightHead = new ListNode(-1);
    ListNode rightcur = rightHead;

    while (head != null) {
        if (head.val < x) {
            leftcur.next = head;
            leftcur = leftcur.next;
        } else {
            rightcur.next = head;
            rightcur = rightcur.next;
        }
        head = head.next;
    }

    leftcur.next = rightHead.next;
    rightcur.next = null;
    return leftHead.next;
}
```
