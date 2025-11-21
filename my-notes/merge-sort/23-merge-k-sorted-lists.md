# 23 Merge K sorted Lists

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

其实有3种做法，用heap，用divide & conquer和两两merge。

heap(priorityqueue)来处理，logk时间找到min来merge，时间都是：T:O(nlogk)，S:O(k)

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }

    PriorityQueue<ListNode> heap = new PriorityQueue<>(lists.length, new Comparator<ListNode>() {
        public int compare(ListNode node1, ListNode node2) {
            return node1.val - node2.val;
        }

    });

    for (ListNode listnode : lists) {
        if (listnode != null) {
            heap.offer(listnode);
        }
    }

    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    while (!heap.isEmpty()) {
        ListNode tmp = heap.poll();
        cur.next = tmp;
        if (tmp.next != null) {
            heap.offer(tmp.next);
        } 
        cur = cur.next;
    }

    return dummy.next;
}
```

divide & conquer,九章答案：用了递归， top down

```java
public ListNode mergeKLists(List<ListNode> lists) {
    if (lists.size() == 0) {
        return null;
    }
    return mergeHelper(lists, 0, lists.size() - 1);
}

private ListNode mergeHelper(List<ListNode> lists, int start, int end) {
    if (start == end) {
        return lists.get(start);
    }

    int mid = start + (end - start) / 2;
    ListNode left = mergeHelper(lists, start, mid);
    ListNode right = mergeHelper(lists, mid + 1, end);
    return mergeTwoLists(left, right);
}

private ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    while (list1 != null && list2 != null) {
        if (list1.val < list2.val) {
            tail.next = list1;
            tail = list1;
            list1 = list1.next;
        } else {
            tail.next = list2;
            tail = list2;
            list2 = list2.next;
        }
    }
    if (list1 != null) {
        tail.next = list1;
    } else {
        tail.next = list2;
    }

    return dummy.next;
}
```

两两merge，九章答案：buttom up

```java
public ListNode mergeKLists(List<ListNode> lists) {  
    if (lists == null || lists.size() == 0) {
        return null;
    }

    while (lists.size() > 1) {
        List<ListNode> new_lists = new ArrayList<ListNode>();
        for (int i = 0; i + 1 < lists.size(); i += 2) {
            ListNode merged_list = merge(lists.get(i), lists.get(i+1));
            new_lists.add(merged_list);
        }
        if (lists.size() % 2 == 1) {
            new_lists.add(lists.get(lists.size() - 1));
        }
        lists = new_lists;
    }

    return lists.get(0);
}

private ListNode merge(ListNode a, ListNode b) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    while (a != null && b != null) {
        if (a.val < b.val) {
            tail.next = a;
            a = a.next;
        } else {
            tail.next = b;
            b = b.next;
        }
        tail = tail.next;
    }

    if (a != null) {
        tail.next = a;
    } else {
        tail.next = b;
    }

    return dummy.next;
}
```
