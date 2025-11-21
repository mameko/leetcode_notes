# 138 Copy LIst with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

这题有两种做法，方法一：用hashmap，把pointer和点分别复制，跟那个[133 clone gr](../graph/133-clone-graph.md)aph的方法有点像。用了O(N)空间。靠的是分步骤。方法二：是copy一个node然后把node连到被copy的node后边，然后再把list拆开，return复制了的那部分。T:O(n)，S:O(1)

```java
public RandomListNode copyRandomList(RandomListNode head) {
    if (head == null) {
        return null;
    }

    copyNext(head);
    copyRandom(head);
    return splitList(head);
}

private void copyNext(RandomListNode head) {
    RandomListNode cur = head;
    while (cur != null) {
        RandomListNode newNode = new RandomListNode(cur.label);
        RandomListNode tmp = cur.next;
        cur.next = newNode;
        newNode.next = tmp;
        cur = tmp;
    }
}

private void copyRandom(RandomListNode head) {
    RandomListNode cur = head;
    while (cur != null) {
        // 注意，这里可能右没有random的node，不加判断会有nullptr
        if (cur.random != null) {　
            cur.next.random = cur.random.next;
        }
        cur = cur.next.next;
    }
}

private RandomListNode splitList(RandomListNode head) {
    RandomListNode newHead = head.next;
    while (head != null) {
        RandomListNode newCur = head.next;
        head.next = newCur.next;
        head = head.next;
        // 这里是判断我们split到最后一个点没有，如果没有（!= null)才继续下去
        if (newCur.next != null) {            // if (head != null) {
            newCur.next = newCur.next.next;   //    newCur.next = head.next;      
        }                                     // }
    }

    return newHead;
}
```
