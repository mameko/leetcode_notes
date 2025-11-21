# L18 Subsets II

Given a list of numbers that may has duplicate numbers, return all possible subsets

## Notice

* Each element in a subset must be in _non-descending_ order.
* The ordering between two subsets is free.
* The solution set must not contain duplicate subsets.

**Example**

If S =`[1,2,2]`, a solution is:

```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

[**Challenge**](http://www.lintcode.com/en/problem/subsets-ii/#challenge)

Can you do it in both recursively and iteratively?

方法跟前面一样，去重跟[L16 Permutaion II](l16-permutation-ii.md) 一样

```java
public ArrayList<ArrayList<Integer>> subsetsWithDup(int[] nums) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }

        Arrays.sort(nums);
        ArrayList<Integer> tmp = new ArrayList<>();
        helper(nums, 0, tmp, res);

        return res;
    }

    private void helper(int[] nums, int start, ArrayList<Integer> tmp, 
                        ArrayList<ArrayList<Integer>> res) {
        res.add(new ArrayList<Integer>(tmp));

        for (int i = start; i < nums.length; i++) {
            //去重代码
            if (i != 0 && i != start && nums[i] == nums[i - 1]) {
                continue;
            }
            tmp.add(nums[i]);
            helper(nums, i + 1, tmp, res);
            tmp.remove(tmp.size() - 1);
        }

    }
```

非递归还是ctci，去重比较麻烦。要记住长度，只加到后面的子集。

例子：\[1, 2, 2]

当集合加到{\[], \[1], \[2], \[1, 2]} 的时候，用1的方法继续加的话会产生重复。{\[], \[1], \[2], \[1, 2], ~~\[2]~~, ~~\[1, 2]~~, \[2, 2], \[1, 2, 2]} 为了去掉中间连个，我们只能把新的数字加到上一回生曾的那些集合里。这里是\[2], \[1, 2]

```java
public ArrayList<ArrayList<Integer>> subsetsWithDup(int[] nums) {
        if (nums == null || nums.length == 0) {
            return null;
        }

        Arrays.sort(nums);

        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<Integer>());

        int lastsize = 1; 
        int last = nums[0];

        for (int j = 0; j < nums.length; j++) {
            int tmp = nums[j];
            if(tmp != last){
                lastsize = res.size();
                last = nums[j];
            }

            int newsize = res.size();
            ArrayList<ArrayList<Integer>> clone = new ArrayList<ArrayList<Integer>>();
            for (int i = newsize-lastsize;i < newsize; i++) {
                ArrayList<Integer> t = (ArrayList<Integer>) res.get(i);
                clone.add((ArrayList<Integer>) t.clone());
            }
            for (ArrayList l : clone) {
                l.add(tmp);
            }
            res.addAll(clone);
        }

        return res;
    }
```
