# 716 Max Stack

Design a max stack that supports push, pop, top, peekMax and popMax.

1. push(x) -- Push element x onto stack.
2. pop() -- Remove the element on top of the stack and return it.
3. top() -- Get the element on the top.
4. peekMax() -- Retrieve the maximum element in the stack.
5. popMax() -- Retrieve the maximum element in the stack, and remove it. If you find more than one maximum elements, only remove the top-most one.

**Example 1:**

```
MaxStack stack = new MaxStack();
stack.push(5); 
stack.push(1);
stack.push(5);
stack.top(); -> 5
stack.popMax(); -> 5
stack.top(); -> 1
stack.peekMax(); -> 5
stack.pop(); -> 1
stack.top(); -> 5
```

**Note:**

1. -1e7 <= x <= 1e7
2. Number of operations won't exceed 10000.
3. The last four operations won't be called when stack is empty.

这题一看跟[155 min stack](155-min-stack.md)差不多，但多了一个popMax。然而这个function有点麻烦。我只想到了把stack里的东西倒出来，remove掉max后再倒回去。popMax用T:O(N)其他的O(1)， S:O(N)

过了几年再上九章，那里提出了一个算法，是用soft delete来处理，把pop的先用set标记一下，但不真正地删，然后真正pop的时候，才一次性删除。因为要快速找到max，所以还用了maxheap，popMax会变成logn。有空补代码。

```java
class MaxStack {
    Stack<Integer> mainStack;
    Stack<Integer> auxStack;

    /** initialize your data structure here. */
    public MaxStack() {
        mainStack = new Stack<>();
        auxStack = new Stack<>();
    }

    public void push(int x) {
        mainStack.push(x);
        if (!auxStack.isEmpty() && auxStack.peek() > x) {
            auxStack.push(auxStack.peek());
        } else {
            auxStack.push(x);
        }
    }

    public int pop() {
        if (mainStack.isEmpty()) {
            return 0;
        }
        auxStack.pop();
        return mainStack.pop();
    }

    public int top() {
        if (mainStack.isEmpty()) {
            return 0;
        }
        return mainStack.peek();
    }

    public int peekMax() {
        if (mainStack.isEmpty()) {
            return 0;
        }
        return auxStack.peek();
    }

    public int popMax() {
        if (mainStack.isEmpty()) {
            return 0;
        }
        Stack<Integer> stack3 = new Stack<>();
        int max = auxStack.peek();
        while (!mainStack.isEmpty() && mainStack.peek() < max) {
            stack3.push(mainStack.pop());
            auxStack.pop();
        }
        this.pop();
        while (!stack3.isEmpty()) {
            this.push(stack3.pop());
        }

        return max;
    }
}

/**
 * Your MaxStack object will be instantiated and called as such:
 * MaxStack obj = new MaxStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.peekMax();
 * int param_5 = obj.popMax();
 */
```

然后看了答案，答案用了TreeMap，然后把大部分的操作变成T:O（logN）peek还是O（1），然后S：O(N)。抄下来参考：

```java
class MaxStack {
    //存<val，那些node在哪里>，当我们要popMax的时候，可以快速找到那个node然后去掉
    TreeMap<Integer, List<Node>> map;
    DoubleLinkedList dll;

    public MaxStack() {
        map = new TreeMap();
        dll = new DoubleLinkedList();
    }

    public void push(int x) {
        Node node = dll.add(x);//每次push把值加到dll尾部
        if(!map.containsKey(x))
            map.put(x, new ArrayList<Node>());
        map.get(x).add(node);//然后treemap里存一份
    }

    public int pop() {
        int val = dll.pop();// 每次pop从dll尾部拿出来
        List<Node> L = map.get(val);// 同样地，从treemap拉的list后面开始删
        L.remove(L.size() - 1);
        if (L.isEmpty()) map.remove(val);
        return val;
    }

    public int top() {
        return dll.peek();//O(1)看看最尾的数目是多少
    }

    public int peekMax() {
        return map.lastKey();// Treemap自带找最大功能，O(logN)
    }

    public int popMax() { 
        int max = peekMax();// 找到要删的值
        List<Node> L = map.get(max);// 找到这个值的位置s
        Node node = L.remove(L.size() - 1);// 因为有多个的时候只要把最靠近栈顶的那个点pop掉，所以除去最后一个
        dll.unlink(node);//同时记得把这个点从dll（那个stack）里删掉
        if (L.isEmpty()) map.remove(max);//如果只有一个点是这个值的话，把list去掉
        return max;
    }
}

class DoubleLinkedList {
    Node head, tail;

    public DoubleLinkedList() {
        head = new Node(0);
        tail = new Node(0);
        head.next = tail;
        tail.prev = head;
    }

    public Node add(int val) {
        Node x = new Node(val);
        x.next = tail;
        x.prev = tail.prev;
        tail.prev = tail.prev.next = x;
        return x;
    }

    public int pop() {
        return unlink(tail.prev).val;
    }

    public int peek() {
        return tail.prev.val;
    }

    public Node unlink(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
        return node;
    }
}

class Node {
    int val;
    Node prev, next;
    public Node(int v) {val = v;}
}

// 补一个自己写的
class MaxStack {
    TreeMap<Integer, List<Node>> map;
    DLL myStack;

    /** initialize your data structure here. */
    public MaxStack() {
        map = new TreeMap<>();
        myStack = new DLL();
    }

    public void push(int x) {
        Node newNode = new Node(x);
        if (!map.containsKey(x)) {
            map.put(x, new ArrayList<>());
        }
        map.get(x).add(newNode);
        myStack.push(newNode);
    }

    public int pop() {
        if (map.size() == 0) { // empty stack
            return -1;
        }
        int popVal = myStack.pop().val;
        List<Node> nodes = map.get(popVal);
        nodes.remove(nodes.size() - 1);
        if (nodes.size() == 0) {
            map.remove(popVal);
        }
        return popVal;
    }

    public int top() {
        if (map.size() == 0) { // empty stack
            return -1;
        }
        return myStack.top().val;
    }

    public int peekMax() {
        if (map.size() == 0) { // empty stack
            return -1;
        }
        return map.lastKey();
    }

    public int popMax() {
        if (map.size() == 0) { // empty stack
            return -1;
        }        
        int val = peekMax();
        if (!map.containsKey(val)) {
            return -1;
        }
        List<Node> nodes = map.get(val);
        Node popNode = nodes.remove(nodes.size() - 1);
        myStack.unlink(popNode);
        if (nodes.size() == 0) {
            map.remove(val);
        }
        return val;
    }
}

class DLL {
    Node head = new Node(-1);
    Node tail = new Node(-1);

    public DLL() {
        head.next = tail;
        tail.prev = head;
    }

    public Node unlink(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
        return node;
    }

    public void push(Node node) {
        node.next = tail;
        node.prev = tail.prev;
        tail.prev = node;
        node.prev.next = node;
    }

    public Node pop() {
        Node nodeToPop = tail.prev;
        return unlink(nodeToPop);
    }

    public Node top() {
        return tail.prev;
    }
}

class Node {
    Node prev;
    Node next;
    int val;    

    public Node(int val) {
        this.val = val;      
    }
}

/**
 * Your MaxStack object will be instantiated and called as such:
 * MaxStack obj = new MaxStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.peekMax();
 * int param_5 = obj.popMax();
 */
```
