# 369 Plus One Linked List

Given a non-negative integer represented as**non-empty**a singly linked list of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.

**Example:**

```
Input:
1->2->3

Output:
1->2->4
```

感觉跟前一题Add Number II好像，估计也是那两种方法做。这里写个递归吧。

```java
public ListNode plusOne(ListNode head) {
    if (head == null) {
        return null;
    }

    int carry = plusOneRec(head);
    if (carry == 1) {
        ListNode newNode = new ListNode(1);
        newNode.next = head;
        return newNode;
    }

    return head;
}

private int plusOneRec(ListNode node) {
    if (node == null) {
        return 1;
    }

    int carry = plusOneRec(node.next);
    int sum = carry + node.val;
    node.val = sum % 10;
    int res = sum / 10;

    return res;
}
```
