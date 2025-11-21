# 119 Pascal s Triangle II

Given an indexk, return thekthrow of the Pascal's triangle.

For example, givenk= 3,\
Return`[1,3,3,1]`.

**Note:**\
Could you optimize your algorithm to use onlyO(k) extra space?

这题如果下标控制得好的话，可以不用多开一个数组。一开始想的时候觉得在原数组上改会把前面的覆盖掉所以躲开了一列存last。但其实如果从后面开始往前更的话，就会没有这个问题。啊，还有，这题跟[118 Pascal's triangle I](118-pascals-triangle.md)的不同是，这里从0开始。

```java
public List<Integer> getRow(int rowIndex) {
    List<Integer> res = new ArrayList<>();
    if (rowIndex < 0) {
        return res;
    }

    res.add(1);
    List<Integer> last = res;
    for (int i = 1; i <= rowIndex; i++) {
        res = new ArrayList<>();
        res.add(1);
        for (int j = 1; j < last.size(); j++) {
            int sum = last.get(j) + last.get(j - 1);
            res.add(sum);
        }
        res.add(1);
        last = res;
    }

    return res;
}

// 再写
public List<Integer> getRow(int rowIndex) {
    List<Integer> res = new ArrayList<>();
    if (rowIndex < 0) {
        return res;
    }

    res.add(1);
    List<Integer> last = res;

    for (int i = 1; i <= rowIndex; i++) {
        res = new ArrayList<>();
        res.add(1);
        for (int j = 1; j <= i - 1; j++) {
            res.add(last.get(j) + last.get(j - 1));
        }
        res.add(1);
        last = res;
    }

    return res;
}
```

下面是program creek的不多开变量版：从后面开始更新。

```java
public List<Integer> getRow(int rowIndex) {
    ArrayList<Integer> result = new ArrayList<Integer>();

    if (rowIndex < 0)
        return result;

    result.add(1);
    for (int i = 1; i <= rowIndex; i++) {
        for (int j = result.size() - 2; j >= 0; j--) {
            result.set(j + 1, result.get(j) + result.get(j + 1));
        }
        result.add(1);
    }
    return result;
}
```
