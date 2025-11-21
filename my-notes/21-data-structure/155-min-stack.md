# 155 Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element.
* getMin() -- Retrieve the minimum element in the stack.

**Example:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

用两个栈。T:O(1),S:O(n),有constant space的解法，有空补。

```java
public class MinStack {
    Stack<Integer> auxS;
    Stack<Integer> mainS;
    /** initialize your data structure here. */
    public MinStack() {
        auxS = new Stack<>();
        mainS = new Stack<>();
    }

    public void push(int x) {
        mainS.push(x);
        if (auxS.isEmpty() || auxS.peek() > x) {
            auxS.push(x);    
        } else {
            auxS.push(auxS.peek());
        }
    }

    public void pop() {
        mainS.pop();
        auxS.pop();
    }

    public int top() {
        return mainS.peek();
    }

    public int getMin() {
        return auxS.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
