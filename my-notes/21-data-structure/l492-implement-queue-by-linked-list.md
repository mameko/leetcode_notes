# L492 Implement Queue by Linked LIst

mplement a Queue by linked list. Support the following basic methods:

enqueue(item). Put a new item in the queue.

dequeue(). Move the first item out of the queue, return it.

Example:

```
enqueue(1)
enqueue(2)
enqueue(3)
dequeue() //return 1
enqueue(4)
dequeue() // return 2
```

enqueue from tail, dequeue from dummy head

```java
public class Queue {

    SingleListNode tail;
    SingleListNode dummy;

    public Queue() {
        dummy = new SingleListNode(-1);
    }

    public void enqueue(int item) {
        SingleListNode newNode = new SingleListNode(item);
        if (dummy.next == null) {
            dummy.next = newNode;
            tail = newNode;
        } else {
            tail.next = newNode;
            tail = tail.next;
        }
    }

    public int dequeue() {
        int val = dummy.next.val;
        dummy = dummy.next;
        return val;
    }
}

class SingleListNode {
    int val;
    SingleListNode next;

    public SingleListNode(int v) {
        val = v;
    }
}
```
