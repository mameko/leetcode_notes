# 39 Combination Sum (L135)

Given a **set** of candidate numbers (**C**)**(without duplicates)**&#x61;nd a target number (**T**), find all unique combinations in **C** where the candidate numbers sums to **T**.

The **same** repeated number may be chosen from **C** unlimited number of times.

**Note:**

* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

For example, given candidate set`[2, 3, 6, 7]`and target`7`,\
A solution set is:

```
[
  [7],
  [2, 2, 3]
]
```

每次从candidate里把数逐个取出来试，如果比target大的话，我们可以跳过因为不应该把target减成负数。最后减到target为0时，把tmp加到答案里。这里因为同一个数可以取多遍，所以进入下一层递归时，还是从现在这数i开始。start是用来记录现在递归到哪里了。n:array长度, m: target大小，worst case：每个数字重复取了m次，例如数组里有1，target = 7， 1会被取7次(m次)。T:O(n^m)，S:O(m)，递归栈的深度。

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (candidates == null || candidates.length == 0) {
        return res;
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    dfsHelper(tmp, res, target, 0, candidates);

    return res;
}

private void dfsHelper(ArrayList<Integer> tmp, List<List<Integer>> res, 
                       int target, int start, int[] candidates) {
    if (target == 0) {
        res.add(new ArrayList<>(tmp));
        return;
    }

    // i begins from start, because we pick previous numbers already. 
    // now need to pick following numbers
    for (int i = start; i < candidates.length; i++) {
        // if bigger than target, target will be neg, so we skip them
        if (candidates[i] > target) {
            continue;// not break, because the set is not ordered
        }

        tmp.add(candidates[i]);
        // recur from i, bucause same number can use more than once
        dfsHelper(tmp, res, target - candidates[i], i, candidates);
        tmp.remove(tmp.size() - 1);
    }
}
```

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (candidates == null || candidates.length == 0) {
        return res;
    }

    List<Integer> tmp = new ArrayList<>();
    dfs(res, tmp, candidates, 0, target);

    return res;
}

private void dfs(List<List<Integer>> res, List<Integer> tmp, 
                 int[] nums, int start, int target) {
    if (target <= 0) {
        res.add(new ArrayList<>(tmp));
        return;
    }

    for (int i = start; i < nums.length; i++) {
        if (target < nums[i]) {
            continue;
        }

        tmp.add(nums[i]);
        dfs(res, tmp, nums, i, target - nums[i]);
        tmp.remove(tmp.size() - 1);
    }
}
```
