# L601 Flatten 2D Vector

Implement an iterator to flatten a 2d vector.

**Example**

Given 2d vector =

```
[
  [1,2],
  [3],
  [4,5,6]
]
```

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be:`[1,2,3,4,5,6]`.

```java
public class Vector2D implements Iterator<Integer> {

    Iterator<List<Integer>> row;
    Iterator<Integer> col;
    public Vector2D(List<List<Integer>> vec2d) {
        row = vec2d.iterator();
        col = null;
    }

    @Override
    public Integer next() {
        return col.next();
    }

    @Override
    public boolean hasNext() {
        while (col == null || (!col.hasNext() && row.hasNext())) {
            col = row.next().iterator();
        }

        return col != null && col.hasNext();
    }

    @Override
    public void remove() {}
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i = new Vector2D(vec2d);
 * while (i.hasNext()) v[f()] = i.next();
 */
```
