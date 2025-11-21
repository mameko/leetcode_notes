# 61 Rotate List

Given a list, rotate the list to the right bykplaces, wherekis non-negative.

For example:\
Given`1->2->3->4->5->NULL`andk=`2`,\
return`4->5->1->2->3->NULL`.

为了避免k过大，所以要数数到底list有多长。用了很像delete n node from the end的方法找到断点。把链表切开，然后接上。

```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null) {
        return null;
    }

    // try to make sure k is smaller than n
    int n = 0;
    ListNode cur = head;
    while (cur != null) {
        n++;
        cur = cur.next;
    }

    k = k % n;
    if (k == 0) {
        return head;
    }

    // cur linkedlist at k, then splice them together
    ListNode fast = head;
    ListNode slow = head;

    while (k > 0) {
        fast = fast.next;
        k--;
    }

    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }

    cur = slow.next;
    fast.next = head;
    head = cur;
    slow.next = null;

    return head;
}
```
