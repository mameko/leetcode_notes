# L528 Flatten Nested List Iterator

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

## Notice

You don't need to implement the remove method.

**Example**

* Given the list`[[1,1],2,[1,1]]`, By calling next repeatedly until hasNext returns false, the order of elements returned by next should be:`[1,1,2,1,1]`.
* Given the list`[1,[4,[6]]]`, By calling next repeatedly until hasNext returns false, the order of elements returned by next should be:`[1,4,6]`.

要用deque来把nested的list元素倒出来。中间还得用stack来保证顺序。

```java
public class NestedIterator implements Iterator<Integer> {
    Deque<NestedInteger> deque = new LinkedList<>();

    public NestedIterator(List<NestedInteger> nestedList) {
        for (NestedInteger ni : nestedList) {
            deque.offerLast(ni);
        }
    }

    // @return {int} the next element in the iteration
    @Override
    public Integer next() {
        return deque.pollFirst().getInteger();
    }

    // @return {boolean} true if the iteration has more element or false
    @Override
    public boolean hasNext() {
        if (deque.size() == 0) {
            return false;
        } else {
            if (deque.peekFirst().isInteger()) {
                return true;
            } else {
                NestedInteger cur = deque.pollFirst();

                while (cur != null && !cur.isInteger()) {
                    Stack<NestedInteger> stack = new Stack<>();
                    for (NestedInteger ni : cur.getList()) {
                        stack.push(ni);
                    }

                    while (!stack.isEmpty()) {
                        deque.offerFirst(stack.pop());
                    }
                    cur = deque.pollFirst();
                }

                 if (cur != null) {
                    deque.offerFirst(cur);
                    return true;
                 }
                 return false;
            }
        }
    }

    @Override
    public void remove() {}
}
```

在leetcode上重写这题的时候，改了一下代码，整洁一点：

```java
public class NestedIterator implements Iterator<Integer> {
    Deque<NestedInteger> dq = new LinkedList<>();

    public NestedIterator(List<NestedInteger> nestedList) {
        for (NestedInteger ni : nestedList) {
            dq.offer(ni);
        }
    }

    @Override
    public Integer next() {
        return dq.poll().getInteger();
    }

    @Override
    public boolean hasNext() {
        if (dq.isEmpty()) {
            return false;
        }

        while (dq.size() > 0 && !dq.peek().isInteger()) {
           List<NestedInteger> tmp = dq.poll().getList();
           //if (tmp.size() == 0) {
           //    continue;
           //}
           Stack<NestedInteger> stack = new Stack<>();
           for (NestedInteger ni : tmp) {
               stack.push(ni);
           }

           while (!stack.isEmpty()) {
               dq.offerFirst(stack.pop());
           }
        }
        return dq.size() != 0;
    }
}
```
