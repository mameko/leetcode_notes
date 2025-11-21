# 160 Intersection of Two Linked Lists

Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:

```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```

begin to intersect at node c1.

**Notes:**

* If the two linked lists have no intersection at all, return `null`.
* The linked lists must retain their original structure after the function returns.
* You may assume there are no cycles anywhere in the entire linked structure.
* Your code should preferably run in O(n) time and use only O(1) memory.

这题有两种方法。方法一：把B连到A的末尾，还有A接到B的结尾，两个指针最后会停在同一个地方，如果存在环的话，会停在两个链表接入的地方，如果没环的话，两个指针会停在null （走A.len+B.len次后停在null）。方法二：查两个表的长度，然后把多出的部分剪掉，再比较剩下相同长度的部分是否有相同，有的话，返回第一个点。估计方法三还可以用hashmap。

方法一：

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) {
        return null;
    }

    ListNode a = headA;
    ListNode b = headB;

    while (a != b) {
        a = a != null ? a.next : headB;
        b = b != null ? b.next : headA;
    }

    return a;
}
```

方法二：

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) {
        return null;
    }

    int m = 0;
    int n = 0;
    ListNode p1 = headA.next;
    ListNode p2 = headB.next;

    while (p1 != null) {
        p1 = p1.next;
        m++;
    }

    while (p2 != null) {
        p2 = p2.next;
        n++;
    }

    p1 = headA;
    p2 = headB;

    int diff = Math.abs(m - n);
    while (diff > 0) {
        if (m > n) {
            p1 = p1.next;
        } else {
            p2 = p2.next;
        }
        diff--;
    }

    while (p1 != p2) {
        p1 = p1.next;
        p2 = p2.next;
    }

    return p1;
}
```
