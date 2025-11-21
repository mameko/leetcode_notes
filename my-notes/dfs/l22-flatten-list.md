# L22 Flatten List

Given a list, each element in the list can be a list or integer. flatten it into a simply list with integers.

## Notice

If the element in the given list is a list, it can contain list too.

**Example**

Given`[1,2,[1,2]]`, return`[1,2,1,2]`.

Given`[4,[3,[2,[1]]]]`, return`[4,3,2,1]`.

[**Challenge**](http://www.lintcode.com/en/problem/flatten-list/#challenge)

Do it in non-recursive.

感觉challenge的用deque。。。

```java
public List<Integer> flatten(List<NestedInteger> nestedList) {
    List<Integer> res = new ArrayList<>();
    if (nestedList == null) {
        return res;
    }

    for (NestedInteger ni : nestedList) {
        if (ni.isInteger()) {
            res.add(ni.getInteger());
        } else {
            res.addAll(flatten(ni.getList()));
        }
    }

    return res;
}
```
