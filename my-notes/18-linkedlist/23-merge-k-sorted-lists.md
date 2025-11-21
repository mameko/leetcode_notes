# 23 Merge k Sorted Lists

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

其实有3种做法，用heap，用divide & conquer和两两merge。这里只写了用heap，其他做法请参照九章答案。T: O(nlogk)

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }

    PriorityQueue<ListNode> heap = new PriorityQueue<>(lists.length, new Comparator<ListNode>() {
        public int compare(ListNode node1, ListNode node2) {
            return node1.val - node2.val;
        }

    });

    for (ListNode listnode : lists) {
        if (listnode != null) {
            heap.offer(listnode);
        }
    }

    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    while (!heap.isEmpty()) {
        ListNode tmp = heap.poll();
        cur.next = tmp;
        if (tmp.next != null) {
            heap.offer(tmp.next);
        } 
        cur = cur.next;
    }

    return dummy.next;
}
```
