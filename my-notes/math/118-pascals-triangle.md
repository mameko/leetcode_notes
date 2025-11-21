# 118 Pascal s Triangle

GivennumRows, generate the firstnumRowsof Pascal's triangle.

For example, givennumRows= 5,\
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

再写，感觉其实直接当数组来就好了，不用last，只要把triangle的左边看成直角边，这样下标就不容易算错了。

```java
public List<List<Integer>> generate(int numRows) {
    List<List<Integer>> res = new ArrayList<>();
    if (numRows < 1) {
        return res;
    }

    List<Integer> first = new ArrayList<>();
    first.add(1);
    res.add(first);

    for (int i = 1; i < numRows; i++) {
        List<Integer> row = new ArrayList<>();
        row.add(1);
        for (int j = 1; j <= i - 1; j++) {
            row.add(res.get(i - 1).get(j - 1) + res.get(i - 1).get(j));
        }
        row.add(1);               
        res.add(row);
    }

    return res;
}
```

用一个last来装着之前一行的结果，然后再generate下一行的。

```java
public List<List<Integer>> generate(int numRows) {
    List<List<Integer>> res = new ArrayList<>();
    if (numRows < 1) {
        return res;
    }

    ArrayList<Integer> last = new ArrayList<>();
    last.add(1);
    res.add(last);

    for (int i = 1; i < numRows; i++) {
        ArrayList<Integer> newRow = new ArrayList<>();
        newRow.add(1);
        for (int j = 1; j < last.size(); j++) {
            int sum = last.get(j) + last.get(j - 1);
            newRow.add(sum);
        }
        newRow.add(1);

        res.add(newRow);
        last = newRow;
    }

    return res;
}
```
