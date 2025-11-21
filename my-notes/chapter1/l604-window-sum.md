# L604 Window Sum

Given an array of n integer, and a moving window(size k), move the window at each iteration from the start of the array, find the`sum`of the element inside the window at each moving.

**Example**

For array`[1,2,7,8,5]`, moving window size k =`3`.\
1 + 2 + 7 = 10\
2 + 7 + 8 = 17\
7 + 8 + 5 = 20\
return`[10,17,20]`

感觉挺像[209](209-minimum-size-subarray-sum.md)的。

```java
public int[] winSum(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return new int[0];
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        if (i >= k) {
            tmp.add(sum);
            sum = sum - nums[i - k];
        }

        sum += nums[i];
    }

    tmp.add(sum);

    int j = 0;
    int[] res = new int[tmp.size()];
    for (Integer elem : tmp) {
        res[j++] = elem;
    }

    return res;
}
```
