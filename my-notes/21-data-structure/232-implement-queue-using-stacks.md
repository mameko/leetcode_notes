# 232 Implement Queue using Stacks

As the title described, you should only use two stacks to implement a queue's actions.

The queue should support`push(element)`,`pop()`and`top()`where pop is pop the first(a.k.a front) element in the queue.

Both pop and top methods should return the value of first element.

**Example**

```
push(1)
pop()     // return 1
push(2)
push(3)
top()     // return 2
pop()     // return 2
```

[**Challenge**](http://www.lintcode.com/en/problem/implement-queue-by-two-stacks/#challenge)

implement it by two stacks, do not use any other data structure and push, pop and top should be O(1) b&#x79;_&#x41;VERAGE_.

```java
public class Queue {
    private Stack<Integer> stack1;
    private Stack<Integer> stack2;

    public Queue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }

    public void push(int element) {
        stack2.push(element);
    }

    public int pop() {
        if (stack1.isEmpty() && stack2.isEmpty()) {
            // throws new Exception("empty queue");
        }

        if (!stack1.isEmpty()) {
            return stack1.pop();
        } else if (!stack2.isEmpty()) {
            move2To1();
        }
        return stack1.pop();
    }

    public int top() {
        if (stack1.isEmpty() && stack2.isEmpty()) {
            // throws new Exception("empty queue");
        }

        if (!stack1.isEmpty()) {
            return stack1.peek();
        } else if (!stack2.isEmpty()) {
            move2To1();
        }
        return stack1.peek();
    }

    private void move2To1(){
        while (!stack2.isEmpty()) {
            stack1.push(stack2.pop());
        }
    }
}
```
