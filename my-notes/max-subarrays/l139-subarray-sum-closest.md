# L139 Subarray Sum Closest

Given an integer array, find a subarray with sum closest to zero. Return the indexes of the first number and last number.

**Example**

Given`[-3, 1, 1, -3, 5]`, return`[0, 2]`,`[1, 3]`,`[1, 1]`,`[2, 2]`or`[0, 4]`.

[**Challenge**](http://www.lintcode.com/en/problem/subarray-sum-closest/#challenge)

O(nlogn) time

这题是把prefix sum排序，排序以后，closest的会被排在一齐。然后我们把相邻的都减一遍，找diff最小的，那个就是答案了。这里prefix sum的维度还是+1，所以先塞一个（0，0）进去。返回时记得-1。而且根据题目要求还得把下标排个序。

```java
     public int[] subarraySumClosest(int[] nums) {
        int[] res = new int[2];
        if (nums == null || nums.length == 0) {
            return res;
        } 

        int len = nums.length;
        if(len == 1) {
            res[0] = res[1] = 0;
            return res;
        }
        Pair[] sums = new Pair[len+1];
        int prev = 0;
        sums[0] = new Pair(0, 0);
        for (int i = 1; i <= len; i++) {
            sums[i] = new Pair(prev + nums[i-1], i);
            prev = sums[i].sum;
        }
        Arrays.sort(sums, new Comparator<Pair>() {
           public int compare(Pair a, Pair b) {
               return a.sum - b.sum;
           } 
        });
        int ans = Integer.MAX_VALUE;
        for (int i = 1; i <= len; i++) {

            if (ans > sums[i].sum - sums[i-1].sum) {
                ans = sums[i].sum - sums[i-1].sum;
                int[] temp = new int[]{sums[i].index - 1, sums[i - 1].index - 1};
                Arrays.sort(temp);
                res[0] = temp[0] + 1;
                res[1] = temp[1];
            }
        }

        return res;
    }


class Pair {
    int sum;
    int index;
    public Pair(int s, int i) {
        sum = s;
        index = i;
    }
}
```
