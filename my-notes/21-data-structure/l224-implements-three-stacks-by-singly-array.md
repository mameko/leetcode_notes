# L224 Implements Three Stacks by Singly Array

嘛，如题...

自己很各种勉强地做了

```java
public class ThreeStacks {
    DLLNode h1;
    DLLNode t1;
    DLLNode h2;
    DLLNode t2;
    DLLNode h3;
    DLLNode t3;
    int totalSize;

    public ThreeStacks(int size) {
        totalSize = size;
        h1 = new DLLNode(0);
        t1 = new DLLNode(-1);
        h1.next = t1;
        t1.prev = h1;
        h2 = new DLLNode(0);
        t2 = new DLLNode(-1);
        h2.next = t2;
        t2.prev = h2;
        h3 = new DLLNode(0);
        t3 = new DLLNode(-1);
        h3.next = t3;
        t3.prev = h3;
    }

    public void push(int stackNum, int value) {
        DLLNode newNode = new DLLNode(value);
        switch (stackNum) {
            case 0:
                t1.prev.next = newNode;
                newNode.prev = t1.prev;
                newNode.next = t1;
                t1.prev = newNode;
                h1.val = h1.val + 1;
                break;
            case 1:
                t2.prev.next = newNode;
                newNode.prev = t2.prev;
                newNode.next = t2;
                t2.prev = newNode;
                h2.val = h2.val + 1;
                break;
            case 2:
                t3.prev.next = newNode;
                newNode.prev = t3.prev;
                newNode.next = t3;
                t3.prev = newNode;
                h3.val = h3.val + 1;
                break;
        }
    }

    public int pop(int stackNum) {
        int ans = -1;
        switch (stackNum) {
            case 0:
                ans = t1.prev.val;
                t1.prev.prev.next = t1;
                t1.prev = t1.prev.prev;
                h1.val = h1.val - 1;
                break;
            case 1:
                ans = t2.prev.val;
                t2.prev.prev.next = t2;
                t2.prev = t2.prev.prev;
                h2.val = h2.val - 1;
                break;
            case 2:
                ans = t3.prev.val;
                t3.prev.prev.next = t3;
                t3.prev = t3.prev.prev;
                h3.val = h3.val - 1;
                break;
        }
        return ans;
    }

    public int peek(int stackNum) {
         int ans = -1;
         switch (stackNum) {
            case 0:
                ans = t1.prev.val;
                break;
            case 1:
                ans = t2.prev.val;
                break;
            case 2:
                ans = t3.prev.val;
                break;
        }        
        return ans;
    }

    public boolean isEmpty(int stackNum) {
          switch (stackNum) {
            case 0:
                return h1.val == 0;
            case 1:
                return h2.val == 0;
            case 2:
                return h3.val == 0;
        } 
        return false;
    }
}

class DLLNode {
    int val;
    DLLNode prev;
    DLLNode next;

    public DLLNode(int v) {
        val = v;
    }

}
```

下面是9章的答案：

```java
public class ThreeStacks {
    public int stackSize;
    public int indexUsed;
    public int[] stackPointer;
    public StackNode[] buffer;

    public ThreeStacks(int size) {
        // do intialization if necessary
        stackSize = size;
        stackPointer = new int[3];
        for (int i = 0; i < 3; ++i)
            stackPointer[i] = -1;
        indexUsed = 0;
        buffer = new StackNode[stackSize * 3];
    }

    public void push(int stackNum, int value) {
        // Write your code here
        // Push value into stackNum stack
        int lastIndex = stackPointer[stackNum];
        stackPointer[stackNum] = indexUsed;
        indexUsed++;
        buffer[stackPointer[stackNum]] = new StackNode(lastIndex, value, -1);
    }

    public int pop(int stackNum) {
        // Write your code here
        // Pop and return the top element from stackNum stack
        int value = buffer[stackPointer[stackNum]].value;
        int lastIndex = stackPointer[stackNum];
        if (lastIndex != indexUsed - 1)
            swap(lastIndex, indexUsed - 1, stackNum);

        stackPointer[stackNum] = buffer[stackPointer[stackNum]].prev;
        if (stackPointer[stackNum] != -1)
            buffer[stackPointer[stackNum]].next = -1;

        buffer[indexUsed-1] = null;
        indexUsed --;
        return value;
    }

    public int peek(int stackNum) {
        // Write your code here
        // Return the top element
        return buffer[stackPointer[stackNum]].value;
    }

    public boolean isEmpty(int stackNum) {
        // Write your code here
        return stackPointer[stackNum] == -1;
    }

    public void swap(int lastIndex, int topIndex, int stackNum) {
        if (buffer[lastIndex].prev == topIndex) {
            int tmp = buffer[lastIndex].value;
            buffer[lastIndex].value = buffer[topIndex].value;
            buffer[topIndex].value = tmp;
            int tp = buffer[topIndex].prev;
            if (tp != -1) {
                buffer[tp].next = lastIndex;
            }
            buffer[lastIndex].prev = tp;
            buffer[lastIndex].next = topIndex;
            buffer[topIndex].prev = lastIndex;
            buffer[topIndex].next = -1;
            stackPointer[stackNum] = topIndex;
            return;
        }

        int lp = buffer[lastIndex].prev;
        if (lp != -1)
            buffer[lp].next = topIndex;

        int tp = buffer[topIndex].prev;
        if (tp != -1)
            buffer[tp].next = lastIndex;

        int tn = buffer[topIndex].next;
        if (tn != -1)
            buffer[tn].prev = lastIndex;
        else {
            for (int i = 0; i < 3; ++i)
                if (stackPointer[i] == topIndex)
                    stackPointer[i] = lastIndex;
        }

        StackNode tmp = buffer[lastIndex];
        buffer[lastIndex] = buffer[topIndex];
        buffer[topIndex] = tmp;
        stackPointer[stackNum] = topIndex;
    }
}

class StackNode {
    public int prev, next;
    public int value;
    public StackNode(int p, int v, int n) {    
        value = v;
        prev = p;
        next = n;
    }
}
```
