# 1721 Swapping Nodes in a Linked List

You are given the `head` of a linked list, and an integer `k`.

Return _the head of the linked list after **swapping** the values of the_ `kth` _node from the beginning and the_ `kth` _node from the end (the list is **1-indexed**)._

**Example 1:**![](https://assets.leetcode.com/uploads/2020/09/21/linked1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [1,4,3,2,5]
```

**Example 2:**

```
Input: head = [7,9,6,6,7,8,3,0,9,5], k = 5
Output: [7,9,6,6,8,7,3,0,9,5]
```

**Example 3:**

```
Input: head = [1], k = 1
Output: [1]
```

**Example 4:**

```
Input: head = [1,2], k = 1
Output: [2,1]
```

**Example 5:**

```
Input: head = [1,2,3], k = 2
Output: [1,2,3]
```

**Constraints:**

* The number of nodes in the list is `n`.
* `1 <= k <= n <= 105`
* `0 <= Node.val <= 100`

这题一开始还以为是要sawp那个node，后来才发现swap value就好了。然后用快慢指针把back指针移动到正确位置。弄快慢指针的时候，顺便弄了front指针。最后swap就好了。

```java
public ListNode swapNodes(ListNode head, int k) {
    if (head == null) {
        return head;
    }
    
    ListNode fast = head;
    ListNode front = head;
    while (k > 1) {
        fast = fast.next;
        front = front.next;
        k--;
    }
    
    ListNode back = head;
    while (fast.next != null) {
        back = back.next;
        fast = fast.next;
    }
    
    int val = front.val;
    front.val = back.val;
    back.val = val;
    
    return head;
}
```
