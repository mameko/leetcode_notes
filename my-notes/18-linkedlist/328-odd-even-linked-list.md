# 328 Odd Even Linked List

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

**Example:**\
Given`1->2->3->4->5->NULL`,\
return`1->3->5->2->4->NULL`.

**Note:**\
The relative order inside both the even and odd groups should remain as it was in the input.\
The first node is considered odd, the second node even and so on ...

这题我用了跟86 partition list相似的方法。网上还有很多更加简洁的。

```java
public ListNode oddEvenList(ListNode head) {
    if (head == null) {
        return null;
    }

    ListNode oddLocHead = new ListNode(-1);
    ListNode evenLocHead = new ListNode(-2);
    ListNode oddcur = oddLocHead;
    ListNode evencur = evenLocHead; 

    ListNode cur = head;
    boolean oddLoc = true;

    while (cur != null) {
        if (oddLoc) {
            oddcur.next = cur;
            oddcur = oddcur.next;
            oddLoc = false;
        } else {
            evencur.next = cur;
            evencur = evencur.next;
            oddLoc = true;
        }

        cur = cur.next;
    }

    oddcur.next = evenLocHead.next;
    evencur.next = null;

    return oddLocHead.next;
}
```
