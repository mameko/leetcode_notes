# 237 Delete Node in a Linked List

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is`1 -> 2 -> 3 -> 4`and you are given the third node with value`3`, the linked list should become`1 -> 2 -> 4`after calling your function.

把现在的node的下一个node的值复制到现在这个node，然后删它的下一个。啊，这里好像漏了删除最后一个点的情况。如果node.next == null，应该把node变成null ？不过估计要知道pre是什么才能设指针。

```java
public void deleteNode(ListNode node) {
    if (node == null) {
        return;
    }

    node.val = node.next.val;
    node.next = node.next.next;
}
```
