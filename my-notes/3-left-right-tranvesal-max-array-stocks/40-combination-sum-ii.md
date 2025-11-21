# 40 Combination Sum II

Given a collection of candidate numbers (**C**) and a target number (**T**), find all unique combinations in **C** where the candidate numbers sums to **T**.

Each number in **C** may only be used **once** in the combination.

**Note:**

* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

For example, given candidate set`[10, 1, 2, 7, 6, 1, 5]`and target`8`,\
A solution set is:

```
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

这题跟39很像，不过每个数只能取一次，所以在进入下一层递归时，我们要传i + 1下去，表示从下一个开始取。这题还有一个需要注意的地方，因为输入的set会有重复，然后答案不能有重复，我们要去重。如果不去重的话我们就会有这种情况：例如题目的例子，我们在递归的过程中，会把【1‘，1’‘，6】和【1’‘，1’，6】都加到结果里。所以就不对了。去重还是跟模板题一样，先排序。

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (candidates == null || candidates.length == 0) {
        return res;
    }

    Arrays.sort(candidates);// sort to put dup number next to each other
    ArrayList<Integer> tmp = new ArrayList<>();
    dfsHelper(tmp, res, target, 0, candidates);

    return res;
}

private void dfsHelper(ArrayList<Integer> tmp, List<List<Integer>> res, 
                       int target, int start, int[] candidates) {
    if (target == 0) {// 把target减到0的时候把tmp加进结果
        res.add(new ArrayList<>(tmp));
        return;
    }

    for (int i = start; i < candidates.length; i++) {
        // 跟前面一样的去重代码
        if (i != start && candidates[i] == candidates[i - 1]) {
            continue;
        }
        // 因为排好序，所以一旦发现会把target减成负数的candidate我们就break
        if (candidates[i] > target) {
            break;
        }

        tmp.add(candidates[i]);
        // candidate只能取一次，所以下次从i + 1开始
        dfsHelper(tmp, res, target - candidates[i], i + 1, candidates);
        tmp.remove(tmp.size() - 1);
    }
}
```
