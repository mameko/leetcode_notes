# L495 Implement Stack

Implement a stack. You can use any data structure inside a stack except stack itself to implement it.

```
push(1)
pop()
push(2)
top()  // return 2
pop()
isEmpty() // return true
push(3)
isEmpty() // return false
```

```java
class Stack {

    LinkedList<Integer> storage = new LinkedList<>();
    // Push a new item into the stack
    public void push(int x) {
        storage.add(x);
    }

    // Pop the top of the stack
    public void pop() {
        storage.removeLast();
    }

    // Return the top of the stack
    public int top() {
        return storage.peekLast();
    }

    // Check the stack is empty or not.
    public boolean isEmpty() {
        return storage.isEmpty();
    }    
}
```
