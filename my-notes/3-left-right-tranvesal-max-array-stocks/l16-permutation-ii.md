# L16 Permutation II

Given a list of numbers with duplicate number in it. Find all **unique** permutations.

**Example**

For numbers`[1,2,2]`the unique permutations are:

```
[
  [1,2,2],
  [2,1,2],
  [2,2,1]
]
```

[**Challenge**](http://www.lintcode.com/en/problem/permutations-ii/#challenge)

Using recursion to do it is acceptable. If you can do it without recursion, that would be great!

基本格式跟上题permutation一样的做法，这里得去重。先排序把相同的数字排到一起，然后选代表，同样的数字选第一个出现的。

```java
public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        res.add(new ArrayList<Integer>());
        return res;
    }

    Arrays.sort(nums);//先排序，把相同的排在一起
    boolean[] visited = new boolean[nums.length];
    List<Integer> cur = new ArrayList<Integer>();
    recurhelper(nums, res, cur, visited, 0);

    return res;
}

private void recurhelper(int[] nums, List<List<Integer>> res, List<Integer> cur, 
                         boolean[] visited, int index) {
    //结束挑还是挑够数字，然后加入结果返回
    if (index == nums.length) {
        res.add(new ArrayList<>(cur));
    }

    for (int i = 0; i < nums.length; i++) {//还是从0开始选
        if (visited[i] == false) {
            //这一小段就是去重代码，如果我们在挑的数已经出现过，
            //而且前面的那个还没被选进现在的buffer里。
            //我们跳过不选这个，选前面后的。选前面的
            if (i > 0 && nums[i] == nums[i - 1] && visited[i - 1] == false) {
                continue;
            }
            cur.add(nums[i]);
            visited[i] = true;
            recurhelper(nums, res, cur, visited, index + 1);//递归选下个数
            cur.remove(cur.size() - 1);
            visited[i] = false;
        }
    }
}
```

这里的方法2还是用ctci的循环，去重方法是利用set，每次加的时候自动去重。

```java
public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<Integer>());
        if (nums == null || nums.length == 0) {
            return res;
        }

        for (int i = 0; i < nums.length; i++) {
            // use set to automatically remove duplicates
            Set<List<Integer>> oneRound = new LinkedHashSet<>();
            for (List<Integer> cur : res) {

                for (int j = 0; j < cur.size() + 1; j++) {
                    cur.add(j, nums[i]);

                    List<Integer> tmp = new ArrayList<Integer>(cur);
                    oneRound.add(tmp);

                    cur.remove(j);
                }

            }
            res = new ArrayList<>(oneRound);
        }

        return res;
    }
```
