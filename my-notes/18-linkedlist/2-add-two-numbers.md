# 2 Add Two Numbers

You are given two**non-empty**linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Input:**(2 -> 4 -> 3) + (5 -> 6 -> 4)\
**Output:**&#x37; -> 0 -> 8

简洁版：

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    int carry = 0;
    while (l1 != null || l2 != null) {
        int n1 = l1 == null ? 0 : l1.val;
        int n2 = l2 == null ? 0 : l2.val;
        int sum = n1 + n2 + carry;
        carry = sum / 10;
        ListNode newNode = new ListNode(sum % 10);
        cur.next = newNode;
        cur = cur.next;
        if (l1 != null ) {
            l1 = l1.next;
        }
        if (l2 != null) {
            l2 = l2.next;
        }
    }

    if (carry != 0) {
        ListNode newNode = new ListNode(1);
        cur.next = newNode;
    }

    return dummy.next;
}
```

详细版：

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    if (l1 == null && l2 == null) {
        return null;
    } else if (l1 == null && l2 != null) {
        return l2;
    } else if (l1 != null && l2 == null) {
        return l1;
    }

    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    int carry = 0;
    while (l1 != null && l2 != null) {
        int sum = l1.val + l2.val + carry;
        carry = sum / 10;
        ListNode newNode = new ListNode(sum % 10);
        cur.next = newNode;
        cur = cur.next;
        l1 = l1.next;
        l2 = l2.next;
    }

    while (l1 != null) {
        int sum = l1.val + carry;
        carry = sum / 10;
        ListNode newNode = new ListNode(sum % 10);
        cur.next = newNode;
        cur = cur.next;
        l1 = l1.next;
    }

    while (l2 != null) {
        int sum = l2.val + carry;
        carry = sum / 10;
        ListNode newNode = new ListNode(sum % 10);
        cur.next = newNode;
        cur = cur.next;
        l2 = l2.next;
    }

    if (carry != 0) {
        ListNode newNode = new ListNode(1);
        cur.next = newNode;
    }

    return dummy.next;
}
```
