# L166 Nth to Last Node in List

Find the nth to last element of a singly linked list.

The minimum number of nodes in list is n.

**Example**

Given a List 3->2->1->5->null and n = 2, return node whose value is 1.

```java
// 2023再刷，多了点checks，不然有些edge case过不了
public ListNode removeNthFromEnd(ListNode head, int n) {
    if (head == null || n < 1) {
        return head;
    }

    ListNode counter = head;
    int size = 0;
    while (counter != null) {
        size++;
        counter = counter.next;
    }

    int actualN = n % size;        
    if (actualN == 0) { // 那些刚好要delete头节点的，譬如，head = [1, 2], n = 2的
        return head.next;
    }

    ListNode dummy = new ListNode();
    dummy.next = head;
    ListNode fast = head;
    while (actualN > 0) {
        fast = fast.next;
        actualN--;
    }

    ListNode slow = head;
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }

    slow.next = slow.next.next;
    return dummy.next;
}


ListNode nthToLast(ListNode head, int n) {
    if (head == null || n <= 0) {
        return null;
    }

    ListNode fast = head;
    ListNode slow = head;

    int count = 0;
    while (fast != null && count < n) {
        fast = fast.next;
        count++;
    }

    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }

    return slow;
}
```
