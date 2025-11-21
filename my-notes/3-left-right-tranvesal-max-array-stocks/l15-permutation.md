# L15 Permutation

Given a list of numbers, return all possible permutations.

## Notice

You can assume that there is no duplicate numbers in the list.

**Example**

For nums =`[1,2,3]`, the permutations are:

```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

[**Challenge**](http://www.lintcode.com/en/problem/permutations/#challenge)

Do it without recursion.

这题是模板，递归搜索所有的permutation。O(n! \* n)

注意：要带上visited来确定哪些用过哪些没用过。做法是\[1, 2, 3]，先挑1，然后进入for循环，再挑2，再挑3，得到\[1, 2, 3]。然后递归上一层。去掉3 （【1， 2】）然后跳出循环。etc...\[1, 3, 2] -> \[2, 1, 3] -> \[2, 3, 1] -> \[3, 1, 2] ->\[3, 2, 1]

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        res.add(new ArrayList<Integer>());
        return res;
    }

    boolean[] visited = new boolean[nums.length];
    List<Integer> cur = new ArrayList<Integer>();
    recurhelper(nums, res, cur, visited, 0);

    return res;
}

private void recurhelper(int[] nums, List<List<Integer>> res, 
                         List<Integer> cur, boolean[] visited, int index) {
    // 递归结束条件是当我们得到原来长度
    if (index == nums.length) {
        res.add(new ArrayList<>(cur));
        return;
    }

    for (int i = 0; i < nums.length; i++) {//每次得重头开始选
        // 因为在不同的递归层数会以不同的顺序取数，所以得带个visited看看哪个没，然后选上
        if (visited[i] == false) {
            cur.add(nums[i]);
            visited[i] = true;
            // 每次递归进下一层时，开始坐标加一，表示挑下一个数
            recurhelper(nums, res, cur, visited, index + 1);
            cur.remove(cur.size() - 1);
            visited[i] = false;
        }
    }
}
```

还有另外一种方法，cc189里介绍的。循环来找。具体方式是：先加一个数字进去oneRound，然后每次把下一个数子插进现在结果的每个位置。eg{ \[**1**] } -> {\[1, **2**], \[**2**, 1]} -> {\[**3**, 1, 2], \[**3**, 2, 1], \[1, **3**, 2], \[2, **3**, 1], \[1, 2, **3**], \[2, 1, **3**]}

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    res.add(new ArrayList<Integer>());
    if (nums == null || nums.length == 0) {
        return res;
    }

    for (int i = 0; i < nums.length; i++) {
        List<List<Integer>> oneRound = new ArrayList<>();
        for (List<Integer> cur : res) {
            // because you need to insert from front to end so + 1
            for (int j = 0; j < cur.size() + 1; j++) {
                cur.add(j, nums[i]);

                List<Integer> tmp = new ArrayList<Integer>(cur);
                oneRound.add(tmp);

                cur.remove(j);
            }

        }
        res = oneRound;
    }

    return res;
}
```
