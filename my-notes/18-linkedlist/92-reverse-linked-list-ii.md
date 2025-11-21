# 92 Reverse Linked List II

Reverse a linked list from positionmton. Do it in-place and in one-pass.

For example:\
Given`1->2->3->4->5->NULL`,m= 2 andn= 4,

return`1->4->3->2->5->NULL`.

**Note:**\
Given m,n satisfy the following condition:\
1 ≤m≤n≤ length of list.

分步骤，完全不能bug free...郁闷

```java
public ListNode reverseBetween(ListNode head, int m, int n) {
    if (head == null || head.next == null || m == n) {
        return head;
    }
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode pre = dummy;// 1 node before m

    int cnt = 0;
    // find m
    while (pre.next != null && cnt != (m  - 1)) {
        pre = pre.next;
        cnt++;
    }

    ListNode mNode = pre.next;
    ListNode post = mNode;// 1 node after n
    cnt++;

    // find n
    while (post != null && cnt != n) {
        post = post.next;
        cnt++;
    }

    ListNode nNode = post;
    post = post.next;

    // reveser m -> n
    ListNode pb = mNode;
    ListNode pm = pb.next;
    ListNode pf = pb.next.next;

    while (!pb.equals(nNode)) {
        pm.next = pb;
        pb = pm;
        pm = pf;
        if (pf != null) {
                pf = pf.next;
            }
    }

    // rewire nodes
    pre.next = pb;
    mNode.next = post;

    return dummy.next;
}
```
