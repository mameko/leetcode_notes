# L541Zigzag Iterator II

Follow up[Zigzag Iterator](http://www.lintcode.com/en/problem/zigzag-iterator/): What if you are given`k`1d vectors? How well can your code be extended to such cases? The "Zigzag" order is not clearly defined and is ambiguous for`k > 2`cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic".

**Example**

Given`k = 3`1d vectors:

```
[1,2,3]
[4,5,6,7]
[8,9]
```

Return`[1,4,8,2,5,9,3,6,7]`.

这题是上一题的扩展。这里我用了一条list来记录所有行的iterator。然后一个一个call（用mod来控制cycle）。主要注意打完了一行以后得处理一下怎样取模。

```java
public class ZigzagIterator2 {
    /**
     * @param vecs a list of 1d vectors
     */

    ArrayList<Iterator<Integer>> iterators;
    int nextList = 0;

    public ZigzagIterator2(ArrayList<ArrayList<Integer>> vecs) {
        iterators = new ArrayList<>();
        for (ArrayList<Integer> list : vecs) {
            if (list.size() > 0) {
                iterators.add(list.iterator());
            }
        }
    }

    public int next() {
        Iterator<Integer> curIterator = iterators.get(nextList);
        int res = curIterator.next();
        if (!curIterator.hasNext()) {
            iterators.remove(nextList);
            nextList--;
        }
        if (this.hasNext()) {
            nextList = (nextList + 1) % iterators.size();
        }

        return res;
    }

    public boolean hasNext() {
        return iterators.size() != 0;
    }

}
```
