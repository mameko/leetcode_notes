# 83 Remove Duplicates from Sorted List

Given a sorted linked list, delete all duplicates such that each element appear onlyonce.

For example,\
Given`1->1->2`, return`1->2`.\
Given`1->1->2->3->3`, return`1->2->3`.

不用dummy node，因为头指针不会变。

```java
public ListNode deleteDuplicates(ListNode head) {
    if (head == null) {
        return head;
    }

    ListNode p1 = head;
    ListNode p2 = head.next;
    while (p2 != null) {
        if (p1.val == p2.val) {
            p1.next = p2.next;
            p2 = p2.next;
        } else {
            p1 = p1.next;
            p2 = p2.next;
        }
    }

    return head;
}
```
