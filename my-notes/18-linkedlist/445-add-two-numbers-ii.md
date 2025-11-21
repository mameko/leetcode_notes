# 445 Add Two Numbers II

You are given two**non-empty**linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up:**\
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

**Example:**

```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

方法一：把两条链表翻转，然后用add two number 1的方法加起来，然后再翻转，用O(n)时间,O(1)空间。

```java
public ListNode addLists2(ListNode l1, ListNode l2) {
    if (l1 == null || l2 == null) {
        return null;
    } else if (l1 == null && l2 != null) {
        return l2;
    } else if (l1 != null && l2 == null) {
        return l1;
    }

    ListNode l1pie = reverse(l1);
    ListNode l2pie = reverse(l2);

    ListNode resultRev = add2List(l1pie, l2pie);
    return reverse(resultRev);
}  

private ListNode reverse(ListNode list) {
    ListNode prev = null;
    while (list != null) {
        ListNode tmp = list.next;
        list.next = prev;
        prev = list;
        list = tmp;
    }

    return prev;
}

private ListNode add2List(ListNode l1, ListNode l2) {
    if (l1 == null || l2 == null) {
        return null;
    } else if (l1 == null && l2 != null) {
        return l2;
    } else if (l1 != null && l2 == null) {
        return l1;
    }

    ListNode dummy = new ListNode(-1);
    ListNode sumcur = dummy;
    int sum = 0;
    int carry = 0;
    while (l1 != null && l2 != null) {
        sum = carry + l1.val + l2.val;
        ListNode newNode = new ListNode(sum % 10);
        sumcur.next = newNode;
        sumcur = sumcur.next;
        carry = sum / 10;
        l1 = l1.next;
        l2 = l2.next;
    }

    while (l1 != null) {
        sum = carry + l1.val;
        ListNode newNode = new ListNode(sum % 10);
        sumcur.next = newNode;
        sumcur = sumcur.next;
        carry = sum / 10;
        l1 = l1.next;
    }

    while (l2 != null) {
        sum = carry + l2.val;
        ListNode newNode = new ListNode(sum % 10);
        sumcur.next = newNode;
        sumcur = sumcur.next;
        carry = sum / 10;
        l2 = l2.next;
    }

    if (carry == 1) {
        ListNode newNode = new ListNode(1);
        sumcur.next = newNode;
    }

    return dummy.next;
}
```

方法二：用栈来把值倒过来，然后逐个出栈相加。连到结果时要注意每次从头插入，最后别忘了判断carry是否为1，为1的话要多加一个点。

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    // method 2 :use stack
    Stack<ListNode> s1 = new Stack<>();
    Stack<ListNode> s2 = new Stack<>();

    while (l1 != null) {
        s1.push(l1);
        l1 = l1.next;
    }

    while (l2 != null) {
        s2.push(l2);
        l2 = l2.next;
    }

    int carry = 0;
    ListNode dummy = new ListNode(-1);
    ListNode next = dummy.next;
    while (!s1.isEmpty() || !s2.isEmpty()) {
        int num1 = s1.isEmpty() ? 0 : s1.pop().val;
        int num2 = s2.isEmpty() ? 0 : s2.pop().val;

        int sum = carry + num1 + num2;
        carry = sum / 10;

        ListNode newNode = new ListNode(sum % 10);
        newNode.next = next;
        dummy.next = newNode;
        next = newNode;
    }

    if (carry != 0) {
        ListNode newNode = new ListNode(carry);
        newNode.next = next;
        dummy.next = newNode;
    }

    return dummy.next;
}
```
