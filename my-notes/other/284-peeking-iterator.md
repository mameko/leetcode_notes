# 284 Peeking Iterator

Given an Iterator class interface with methods: `next()` and `hasNext()`, design and implement a PeekingIterator that support the `peek()` operation -- it essentially peek() at the element that will be returned by the next call to next().

**Example:**

```
Assume that the iterator is initialized to the beginning of the list: [1,2,3].

Call next() gets you 1, the first element in the list.
Now you call peek() and it returns 2, the next element. Calling next() after that still return 2. 
You call next() the final time and it returns 3, the last element. 
Calling hasNext() after that should return false.
```

**Follow up**: How would you extend your design to be generic and work with all types, not just integer?

这题，其实没看懂要考啥？一开始自己就拿着Deque做，因为打算，先取出再插入，因为在同一头，所以deque的linkedlist比较高效。然后呢，看了答案，好像只需要在iterator外面套一层就ok了，不用太纠结。

```java
class PeekingIterator implements Iterator<Integer> {
    Deque<Integer> storage = new LinkedList<>();
	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
	    while (iterator.hasNext()) {
            storage.add(iterator.next());
        }
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return storage.getFirst();
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    return storage.removeFirst();
	}
	
	@Override
	public boolean hasNext() {
	    return !storage.isEmpty();
	}
}

// 参考答案
class PeekingIterator implements Iterator<Integer> {
	Iterator<Integer> itr = null;
    Integer next = null;
    public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
        if (iterator.hasNext()) {
            next = iterator.next();
        }
        itr = iterator;	    
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return next;
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {        
	    if (next == null) {
            return null;
        }
        Integer val = next;
        next = null;
        if (itr.hasNext()) {
            next = itr.next();
        }
        return val;
	}
	
	@Override
	public boolean hasNext() {
	    return next != null;
	}
}
```
