# 234 Palindrome Linked List

Given a singly linked list, determine if it is a palindrome.

**Follow up:**\
Could you do it in O(n) time and O(1) space?

两种解法，解法一是用stack，一边tranverse链表一边把前半部分放进栈里。这里因为list的长度可能是基数，所以中间会判断一下然后pop掉多余的一个。最后是判断栈里的值是否与后半段一样。用了O(n)的extra space。

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) {
        return true;
    }

    Stack<ListNode> stack = new Stack<>();
    ListNode fast = head.next;
    ListNode slow = head;
    while (fast != slow) {
        if (fast == null || fast.next == null) {
            break;
        }

        stack.push(slow);
        fast = fast.next.next;
        slow = slow.next;
    }

    stack.push(slow);

    if (stack.peek().val != slow.next.val) {
        stack.pop();
    }

    slow = slow.next;
    while (!stack.isEmpty() && slow != null) {
        ListNode tmp = stack.pop();
        if (tmp.val != slow.val) {
            return false;
        }
        slow = slow.next;
    }

    if ((slow == null && !stack.isEmpty()) || (slow != null && stack.isEmpty())) {
        return false;
    }

    return true;
}
```

解法二：首先找到中点，切开，然后把后半段翻转，然后再比较两条链元素的值。这个只用了O(1) space。

```java
public boolean isPalindrome(ListNode head) {
    if (head == null) {
        return true;
    }

    // use 2 pointer to find later half
    ListNode fast = head.next;
    ListNode slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
    }

    // reverse later half of list
    ListNode laterHalf = slow.next;
    slow.next = null;
    laterHalf = reverse(laterHalf);

    // compare 2 part to see if we have palindrome list
    while (laterHalf != null) {
        if (head.val != laterHalf.val) {
            return false;
        }
        laterHalf = laterHalf.next;
        head = head.next;
    }

    return true;
}

private ListNode reverse(ListNode node) {
    ListNode pre = null;

    while (node != null) {
        ListNode tmp = node.next;
        node.next = pre;
        pre = node;
        node = tmp;
    }

    return pre;
}
```
