# L217 Remove Duplicate from unsorted List

因为改ladder了，所以只能如题了。

解法有3种:

解法一，hashSet T:O(n), S:O(n),

```java
public ListNode removeDuplicates(ListNode head) { 
    if (head == null) {
        return null;
    }

    HashSet<Integer> existingNodes = new HashSet<>();

    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode cur = dummy;

    while (cur.next != null) {
        if (existingNodes.contains(cur.next.val)) {
            cur.next = cur.next.next;
        } else {
            existingNodes.add(cur.next.val);
            cur = cur.next;
        }
    }

    return dummy.next;
}
```

解法二：排序再remove，T：O(nlogn), S:O(1)

解法三：直接上，两层循环找dup，可以调用[203 Remove Linked List Element](203-remove-linked-list-elements.md)
