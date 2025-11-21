# 216 Combination Sum III

Find all possible combinations of **k** numbers that add up to a number **n**, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Example 1:**

Input:**k**= 3,**n**= 7

Output:

```
[[1,2,4]]
```

**Example 2:**

Input:**k**= 3,**n**= 9

Output:

```
[[1,2,6], [1,3,5], [2,3,4]]
```

这题跟前两题很相似，不过我们只要把k个元素装到tmp就ok了。所以递归结束条件是tmp.size（） == k，另外我们还是找combination sum，所以还得判断一下target（n） 是否为0，如果是，才把tmp加到结果里。这里可以取的数从1到9 inclusive，所以loop的时候到=9。同样，会令target为负的，我们跳过。因为有序，所以可以break跳过后面全部。因为每个数字只能取一次，所以进入递归时传入i+1.

```java
public List<List<Integer>> combinationSum3(int k, int n) {
    List<List<Integer>> res = new ArrayList<>();
    if (k == 0 || n < 0) {
        return res;
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    dfsHelper(tmp, res, k, n, 1);// 从1到9 inclusive，所以从1开始取
    return res;
}

private void dfsHelper(ArrayList<Integer> tmp, List<List<Integer>> res, 
                       int k, int n, int start) {
    if (tmp.size() == k) {// 要取k个数
        if (n == 0) {// target要减到0
            res.add(new ArrayList<>(tmp));
        }
        return;
    }

    for (int i = start; i <= 9; i++) {
        if (i > n) {// 跳过会把target减为负数的数
            break;
        }

        tmp.add(i);
        // 每个数只能取1次，下次递归从i + 1开始     
        dfsHelper(tmp, res, k, n - i, i + 1);
        tmp.remove(tmp.size() - 1);
    }

}
```
