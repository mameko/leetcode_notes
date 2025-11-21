# L493 Implement Queue by Linked List II

Implement a deQueue by linked list. Provide the following basic methods:

push\_front(item). Add a new item to the front of queue.

push\_back(item). Add a new item to the back of the queue.

pop\_front(). Move the first item out of the queue, return it.

pop\_back(). Move the last item out of the queue, return it.

Example:

```java
push_front(1)
push_back(2)
pop_back() // return 2
pop_back() // return 1
push_back(3)
push_back(4)
pop_front() // return 3
pop_front() // return 4
```

```java
public class Dequeue {
    DoublyListNode head;
    DoublyListNode tail;

    public Dequeue() {
        head = new DoublyListNode(-1);
        tail = new DoublyListNode(-1);
        head.next = tail;
        tail.prev = head;
    }

    public void push_front(int item) {
        DoublyListNode newNode = new DoublyListNode(item);
        newNode.next = head.next;
        head.next = newNode;
        newNode.prev = newNode.next.prev;
        newNode.next.prev = newNode;
    }

    public void push_back(int item) {
        DoublyListNode newNode = new DoublyListNode(item);
        newNode.prev = tail.prev;
        tail.prev = newNode;
        newNode.prev.next = newNode;
        newNode.next = tail;
    }

    public int pop_front() {
        int val = head.next.val;
        head.next = head.next.next;
        head.next.prev = head;
        return val;
    }

    public int pop_back() {
        int val = tail.prev.val;
        tail.prev = tail.prev.prev;
        tail.prev.next = tail;
        return val;
    }
}

class DoublyListNode {
    int val;
    DoublyListNode prev;
    DoublyListNode next;

    public DoublyListNode(int v) {
        val = v;
    }
}
```
