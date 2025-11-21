# 147 Insertion Sort List

Sort a linked list using insertion sort.

通过两遍循环来做，前半是已经排好序的，每次选排好序后面的元素，插入到前半有序的部分。所以叫插入排序。

```java
public ListNode insertionSortList(ListNode head) {
    if (head == null) {
        return null;
    }

    // this is the new head, don't need to dummy.next = head;
    ListNode dummy = new ListNode(-1);

    // consider first half is sorted
    // pick next one and insert it in there
    while (head != null) {
        ListNode node = dummy;// node before the insertion point
        while (node.next != null && node.next.val < head.val) {
            node = node.next;
        }

        ListNode tmp = head.next;
        head.next = node.next;
        node.next = head;
        head = tmp;
    }

    return dummy.next;
}
```
