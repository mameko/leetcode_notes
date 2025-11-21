# 77 Combinations

Given two integers n and k, return all possible combinations of k numbers out of 1 ...n.

For example,\
If n= 4 and k= 2, a solution is:

```
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

这题跟前一题很像，其实他们都很像。也是取k个就好了，所以可以用tmp的size == k作为递归结束条件。然后每个数只能取一次，所以进入下一层时i + 1。然后因为是1到n inclusive，所以循环的时候要取到=n。

```java
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> res = new ArrayList<>();
    if (n < 0 || k < 1) {
        return res;
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    dfsHelper(tmp, res, 1, n, k);

    return res;
}

private void dfsHelper(ArrayList<Integer> tmp, List<List<Integer>> res, 
                       int start, int n, int k) {
    if (k == tmp.size()) {// 取够k个了
        res.add(new ArrayList<>(tmp));
        return;
    }

    for (int i = start; i <= n; i++) {
        tmp.add(i);
        dfsHelper(tmp, res, i + 1, n, k);// 每个数字只取1次，下次 从i+1开始取
        tmp.remove(tmp.size() - 1);
    }
}
```
