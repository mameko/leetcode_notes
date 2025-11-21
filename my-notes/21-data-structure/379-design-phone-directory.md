# 379 Design Phone Directory

Design a Phone Directory which supports the following operations:

1. `get`: Provide a number which is not assigned to anyone.
2. `check`: Check if a number is available or not.
3. `release`: Recycle or release a number.

**Example:**

```
// Init a phone directory containing a total of 3 numbers: 0, 1, and 2.
PhoneDirectory directory = new PhoneDirectory(3);

// It can return any available phone number. Here we assume it returns 0.
directory.get();

// Assume it returns 1.
directory.get();

// The number 2 is available, so return true.
directory.check(2);

// It returns 2, the only number that is left.
directory.get();

// The number 2 is no longer available, so return false.
directory.check(2);

// Release number 2 back to the pool.
directory.release(2);

// Number 2 is available again, return true.
directory.check(2);
```

这题虽然分在链表里，但发现用queue的话意外地好做。主要注意的是各种非法操作。release的时候要判断传入数字是否真的已经被用了。如果不是的话可以跳过忽略。

```java
boolean[] hm;
Queue<Integer> storage;
/** Initialize your data structure here
    @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
public PhoneDirectory(int maxNumbers) {
    if (maxNumbers < 1) {
        // throw exception    
    }

    hm = new boolean[maxNumbers];
    Arrays.fill(hm, true);
    storage = new LinkedList<>();
    for (int i = 0; i < maxNumbers; i++) {
        storage.offer(i);
    }
}

/** Provide a number which is not assigned to anyone.
    @return - Return an available number. Return -1 if none is available. */
public int get() {
    if (storage.isEmpty()) {
        return -1;
    }

    int num = storage.poll();
    hm[num] = false;

    return num;
}

/** Check if a number is available or not. */
public boolean check(int number) {
    if (number < 0 || number >= hm.length) {
        return false;
    }
    return hm[number];
}

/** Recycle or release a number. */
public void release(int number) {
    if (number < 0 || number >= hm.length) {
        // throw exception
        return;
    }

    if (!hm[number]) {
        storage.offer(number);
        hm[number] = true;
    }
}
```
