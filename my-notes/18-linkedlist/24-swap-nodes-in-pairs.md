# 24 Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

For example,\
Given`1->2->3->4`, you should return the list as`2->1->4->3`.

Your algorithm should use only constant space. You may **not** modify the values in the list, only nodes itself can be changed.

T:O(n),S:O(1)

```java
public ListNode swapPairs(ListNode head) {
    if (head == null) {
        return null;
    }

    ListNode dummy = new ListNode(-1);
    dummy.next = head;

    ListNode front = head;// 1st node in swaping pair
    ListNode back = head.next;// 2nd node in swapoing pair
    ListNode pre = dummy;// one node before the swapping pair
    while (front != null && front.next != null) {
        back = front.next;
        front.next = back.next;
        back.next = front;
        pre.next = back;
        pre = front;
        front = front.next;
    }
    return dummy.next;
}
```

```java
public ListNode swapPairs(ListNode head) {
    if (head == null) {
        return head;
    }

    ListNode dummy = new ListNode(-1);
    dummy.next = head;

    ListNode pre = dummy;
    ListNode cur = head;
    while (cur != null && cur.next != null) {
        ListNode next = cur.next;
        ListNode tmp = next.next;
        next.next = cur;
        cur.next = tmp;
        pre.next = next;

        pre = cur;
        cur = cur.next;
    }


    return dummy.next;
}
```
