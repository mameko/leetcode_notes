# gift wrapping algorithm

convex hull...=\_=..果断把伪代码写下来作为参考。

```
p = leftMostBottomPoint
current = p
do {
    // sentinel
    leftmost = l
    for i from 2 to N
        if (left(current, point[i], leftmoset)) {
            leftmost = i;
    current = point[leftmost]
} while (current != p)
```
