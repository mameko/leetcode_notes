# 225 Implement Stack using Queues

```java
class Stack {
private Queue<Integer> queue1;
private Queue<Integer> queue2;

public Stack() {
    queue1 = new LinkedList<Integer>();
    queue2 = new LinkedList<Integer>();
}

private void moveItems() {
    while (queue1.size() != 1) {
        queue2.offer(queue1.poll());
    }
}

private void swapQueues() {
    Queue<Integer> temp = queue1;
    queue1 = queue2;
    queue2 = temp;
}

/**
 * push a new item into the stack
 */
public void push(int value) {
    queue1.offer(value);
}

/**
 * return the top of the stack
 */
public int top() {
    moveItems();
    int item = queue1.poll();
    swapQueues();
    queue1.offer(item);
    return item;
}

/**
 * pop the top of the stack and return it
 */
public void pop() {
    moveItems();
    queue1.poll();
    swapQueues();
}

/**
 * check the stack is empty or not.
 */
public boolean isEmpty() {
    return queue1.isEmpty();
}
}

//2022.09又写了一次,push, O(1), pop, O(n)
class MyStack {
    Queue<Integer> queue1 = new LinkedList<>();;
    Queue<Integer> queue2 = new LinkedList<>();;
    Queue<Integer> currentQueue;
    Queue<Integer> theOtherQueue;
    public MyStack() {
        currentQueue = queue1;
        theOtherQueue = queue2;
    }
    
    public void push(int x) {
        currentQueue.offer(x);
    }
    
    public int pop() {
        if(empty()) {
            return -1;
        }
        
        while (currentQueue.size() > 1) {
            theOtherQueue.offer(currentQueue.poll());
        }            
        
        int result = currentQueue.poll();
        Queue<Integer> tmpQueue = currentQueue;
        currentQueue = theOtherQueue;
        theOtherQueue = tmpQueue;
        return result;      
    }
    
    public int top() {
        if(empty()) {
            return -1;
        }
        while (currentQueue.size() > 1) {
            theOtherQueue.offer(currentQueue.poll());
        }            
        
        int result = currentQueue.poll();
        Queue<Integer> tmpQueue = currentQueue;
        currentQueue = theOtherQueue;
        theOtherQueue = tmpQueue;
        currentQueue.offer(result);
        return result;
    }
    
    public boolean empty() {
        return currentQueue.isEmpty() && theOtherQueue.isEmpty();
    }
}
// 这题还有个follow up，用1个queue怎么实现？push, O(n), pop, O(1)
// 思路，因为stack与queue刚好相反，所以每一次insert，我们就把前面元素拿出来，从尾巴插进去。
// 每次插入都调整一次顺序，那么pop的时候，就可以直接从队头拿了
private LinkedList<Integer> q1 = new LinkedList<>();

// Push element x onto stack.
public void push(int x) {
    q1.add(x);
    int sz = q1.size();
    while (sz > 1) {
        q1.add(q1.remove());
        sz--;
    }
}

// Removes the element on top of the stack.
public void pop() {
    q1.remove();
}
// Return whether the stack is empty.
public boolean empty() {
    return q1.isEmpty();
}
// Get the top element.
public int top() {
    return q1.peek();
}
```
