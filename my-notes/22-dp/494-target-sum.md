# 494 Target Sum

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols`+`and`-`. For each integer, you should choose one from`+`and`-`as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

**Example 1:**

```
Input:
 nums is [1, 1, 1, 1, 1], S is 3. 

Output:
 5

Explanation:


-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

**Note:**

1. The length of the given array is positive and will not exceed 20.
2. The sum of elements in the given array will not exceed 1000.
3. Your output answer is guaranteed to be fitted in a 32-bit integer.

这题得看答案才把dp做正确。一层一层地把这层所能得到的值算出来。计算频率是要注意，算的是这层（tmp）的加上一层（hm）的freq。一开始没搞清楚所以一直算不对。时间是O(range of sum value \* size of nums), 空间是O（range of sum value）

```java
public int findTargetSumWays(int[] nums, int S) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    // <sum, freq>
    HashMap<Integer, Integer> hm = new HashMap<>();
    hm.put(0, 1);// set 0 to 1 so that the 1st num will be added to map with correct freq
    for (Integer num : nums) {
        HashMap<Integer, Integer> tmp = new HashMap<>();
        for (Map.Entry<Integer, Integer> entry : hm.entrySet()) {
            int val = entry.getKey();
            int freq = entry.getValue();// there are freq many ways to get to prev step
            int newValplus = val + num;
            int newValminus = val - num;
            if (tmp.containsKey(newValplus)) {
                // we need to add the freq ways to other ways that from another number
                tmp.put(newValplus, tmp.get(newValplus) + freq);
            } else {
                tmp.put(newValplus, freq);// so we have freq many ways to get to this new value
            }

            if (tmp.containsKey(newValminus)) {// do same thing for minus value
                tmp.put(newValminus, tmp.get(newValminus) + freq);
            } else {
                tmp.put(newValminus, freq);
            }
        }
        hm = tmp;
    }

    return hm.get(S) == null ? 0 : hm.get(S);
}
```

这题的brute force是dfs，补上来做参考：

```java
int sum = 0;
public int findTargetSumWays(int[] nums, int S) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    dfs(nums, S, 0);
    return sum;
}

private void dfs(int[] nums, int target, int start) {
    if (start == nums.length) {
        if (target == 0) {
            sum++;    
        }
        return;
    }

    dfs(nums, target - nums[start], start + 1);
    dfs(nums, target + nums[start], start + 1);        
}
```
