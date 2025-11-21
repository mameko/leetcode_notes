# L17 Subsets

Given a set of distinct integers, return all possible subsets.

## Notice

* Elements in a subset must be in _non-descending_ order.
* The solution set must not contain duplicate subsets.

**Example**

If S =`[1,2,3]`, a solution is:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

[**Challenge**](http://www.lintcode.com/en/problem/subsets/#challenge)

Can you do it in both recursively and iteratively?

这题也是很模板的递归。先找以1开头的，然后以2开头顺的，以此类推。 顺序是：{**\[]**} -> {**\[1]**} -> {**\[1, 2]**} -> {**\[1, 2, 3]**} 去掉3, {\[1, 2]}； 去掉2 {\[1]}； 加3，{**\[1, 3]**}； 去掉3，{\[1]} ；去掉1 {\[]}； 加2 {**\[2]**}； 加3{**\[2, 3]**}；然后去3{\[2]}；去2，{\[]}； 加3{**\[3]**}；去3。T:O(n \* 2^n)因为有这么多个子集，求每一个用了O(n)的时间。S:O(n)递归栈的深度。

```java
public ArrayList<ArrayList<Integer>> subsets(int[] nums) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        // sort to add elements in non-descending order这只是题目要求
        Arrays.sort(nums);

        ArrayList<Integer> tmp = new ArrayList<>();
        helper(nums, 0, tmp, res);

        return res;
    }

    private void helper(int[] nums, int start, ArrayList<Integer> tmp, 
                        ArrayList<ArrayList<Integer>> res) {
        // everytime in add tmp to res
        res.add(new ArrayList<Integer>(tmp));

        // loop to add other element into the set
        // loop from start, not like permutation, 
        // we don't need to pick every number
        for (int i = start; i < nums.length; i++) {// 递归结束条件是loop完所有数字一遍
            tmp.add(nums[i]);
            helper(nums, i + 1, tmp, res);//下一层递归加下一个数字
            tmp.remove(tmp.size() - 1);
        }
    }
```

非递归解法还是用ctci的一条链loop着插。{\[]} -> {\[],\[**1**]} -> {\[], \[1], \[**2**], \[1, **2**]} -> {\[], \[1], \[2], \[1, 2], \[**3**], \[1, **3**], \[2, **3**], \[1, 2, **3**]}

```java
public ArrayList<ArrayList<Integer>> subsets(int[] nums) {
     if(nums==null||nums.length==0){
         return null;
     }
     Arrays.sort(nums);
     ArrayList<ArrayList<Integer>> res = new ArrayList<>();
     res.add(new ArrayList<Integer>());
     for (int j = 0; j < nums.length; j++) {
            int tmp = nums[j];
            ArrayList<ArrayList<Integer>> clone = new ArrayList<ArrayList<Integer>>();
            for (ArrayList<Integer> item : res) {
                clone.add((ArrayList<Integer>) item.clone());
            }
            for (int i = 0; i < clone.size(); i++) {
                clone.get(i).add(tmp);
            }
            res.addAll(clone);
        }

     return res;
 }
```
