# 148 Sort List

Sort a linked list in O(nlogn) time using constant space complexity.

merge sort，分步骤

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    return mergeSortList(head);
}

private ListNode mergeSortList(ListNode listPart) {

    if (listPart == null || listPart.next == null) {
        return listPart;
    }

    ListNode mid = findMid(listPart);
    ListNode l1 = listPart;
    ListNode l2 = mid.next;
    mid.next = null;
    l1 = mergeSortList(l1);
    l2 = mergeSortList(l2);
    return merge(l1, l2);
}

private ListNode merge(ListNode l1, ListNode l2) {
    if (l1 == null && l2 != null) {
        return l2;
    } else if (l1 != null && l2 == null) {
        return l1;
    }
    ListNode cur = null;
    ListNode p1 = l1;
    ListNode p2 = l2;

    if (p1.val < p2.val) {
        cur = p1;
        p1 = p1.next;
    } else {
        cur = p2;
        p2 = p2.next;
    }

    ListNode head = cur;
    while (p1 != null && p2 != null) {
        if (p1.val < p2.val) {
            cur.next = p1;
            p1 = p1.next;
        } else {
            cur.next = p2;
            p2 = p2.next;
        }
        cur = cur.next;
    }

    if (p1 != null) {
        cur.next = p1;
    } else if (p2 != null) {
        cur.next = p2;
    }
    return head;
}

private ListNode findMid(ListNode start) {
    ListNode fast = start;
    ListNode slow = start;

    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}
```
