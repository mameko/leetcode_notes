# L511 Swap Two Node in Linked List

Given a linked list and two values v1 and v2. Swap the two nodes in the linked list with values v1 and v2. It's guaranteed there is no duplicate values in the linked list. If v1 or v2 does not exist in the given linked list, do nothing.

## Notice

You should swap the two nodes with values v1 and v2. Do not directly swap the values of the two nodes.

**Example**

Given`1->2->3->4->null`and v1 =`2`, v2 =`4`.

Return`1->4->3->2->null`.

注意判断两个点是否相邻，要分开处理。

```java
public ListNode swapNodes(ListNode head, int v1, int v2) {
    if (head == null || head.next == null) {
        return head;
    }

    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode n1pre = dummy;
    ListNode n2pre = dummy;
    int cnt1 = 0;
    int cnt2 = 0;
    while (n1pre.next != null) {
        if (n1pre.next.val == v1) {
            break;
        }
        n1pre = n1pre.next;
        cnt1++;
    }

    if (n1pre.next == null && n1pre.val != v1) {// node with value v1 doesn't exist, do nothing
        return head;
    }

    while (n2pre.next != null) {
        if (n2pre.next.val == v2) {
            break;
        }
        n2pre = n2pre.next;
        cnt2++;
    }

    if (n2pre.next == null && n2pre.val != v2) {// node with value v2 doesn't exist, do nothing
        return head;
    }

    if (v1 == v2) {
        return head;
    }

    if (cnt1 > cnt2) {// make n1pre before n2pre
        ListNode tmp = n1pre;
        n1pre = n2pre;
        n2pre = tmp;
    }

    ListNode n1cur = n1pre.next;
    ListNode n1post = n1cur.next;
    ListNode n2cur = n2pre.next;
    ListNode n2post = n2cur.next;

    if (n1pre.next == n2pre) {// next to each other
        n1cur.next = n2post;
        n1pre.next = n2cur;
        n2cur.next = n1cur;
    } else {// sperate
        n1cur.next = n2post;
        n1pre.next = n2cur;
        n2pre.next = n1cur;
        n2cur.next = n1post;
    }
    return dummy.next;
}
```
