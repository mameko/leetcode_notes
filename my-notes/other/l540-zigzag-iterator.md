# L540 Zigzag Iterator

Given two**1d**vectors, implement an iterator to return their elements alternately.

**Example**

Given two 1d vectors:

```
v1 = [1, 2]
v2 = [3, 4, 5, 6]
```

By calling next repeatedly until hasNext returns`false`, the order of elements returned by next should be:`[1, 3, 2, 4, 5, 6]`.

用两个变量来记录长度和遍历到哪里了。轮流移动就ok了。

```java
public class ZigzagIterator {
    /**
     * @param v1 v2 two 1d vectors
     */
     int len1 = 0;
     int i = 0;
     int len2 = 0;
     int j = 0;
     List<Integer> list1 = new ArrayList<>();
     List<Integer> list2 = new ArrayList<>();
    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        len1 = v1.size();
        len2 = v2.size();
        list1 = v1;
        list2 = v2;
    }

    public int next() {
        int res = -1;
        if (i < len1 && j < len2) {
            if (i <= j) {
                res = list1.get(i);
                i++;
            } else {
                res = list2.get(j);
                j++;
            }
        } else if (i == len1 && j < len2) {
            res = list2.get(j);
            j++;
        } else if (i < len1 && j == len2) {
            res = list1.get(i);
            i++;
        }

        return res;
    }

    public boolean hasNext() {
        if (i < len1 || j < len2) {
            return true;
        }

        return false;
    }
}
```
