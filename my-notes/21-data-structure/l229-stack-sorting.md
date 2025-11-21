# L229 Stack Sorting

Sort a stack in ascending order (with biggest terms on top).

You may use at most one additional stack to hold items, but you may not copy the elements into any other data structure (e.g. array).

**Example**

```
Given stack =

| |
|3|
|1|
|2|
|4|
 -

return :
| |
|4|
|3|
|2|
|1|
 -
```

The data will be serialized to \[4,2,1,3]. The last element is the element on the top of the stack.

我的解法：

```java
public class Solution {
    /**
     * @param stack an integer stack
     * @return void
     */
    public void stackSorting(Stack<Integer> stack) {
        if (stack == null || stack.size() == 0) {
            return;
        }

        Stack<Integer> aux = new Stack<>();
        int i = 0;
        int n = stack.size();
        while (!stack.isEmpty()) {
            int tmp = stack.pop();

            while (!stack.isEmpty() && stack.peek() > tmp) {
                aux.push(stack.pop());
            }

            aux.push(tmp);

            if (stack.size() <= i) {
                while (!aux.isEmpty()) {
                    stack.push(aux.pop());
                }

                i++;
            }

            if (i == n) {
                break;
            }

        }
    }
}
```

ctci189里简洁的解法：

```java
while (!stack.isEmpty()) {
    int tmp = stack.pop();
    while (!aux.isEmpty() && aux.peek() < tmp) {
        stack.push(aux.pop());
    }
    aux.push(tmp);
}

while (!aux.isEmpty()) {
    stack.push(aux.pop());
}
```
