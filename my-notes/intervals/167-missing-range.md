# 163 Missing Range

Given a sorted integer array where**the range of elements are in the inclusive range \[lower,upper]**, return its missing ranges.

For example, given`[0, 1, 3, 50, 75]`,lower= 0 andupper= 99, return`["2", "4->49", "51->74", "76->99"].`

这题只能够用蛋疼来形容，要用long来pass那些range是MAX或MIN的test case。基本上跟上一题差不多。如果相差为1个数，加一个数进答案；如果相差多于1个数，加两个数进答案。

```java
public List<String> findMissingRanges(int[] nums, int lower, int upper) {
    List<String> res = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        if (lower == upper) {
            res.add(String.valueOf(lower));
        } else {
            res.add(String.valueOf(lower) + "->" + String.valueOf(upper));
        }
        return res;
    }

    long startVal = (long)lower - 1;
    for (int i = 0; i <= nums.length; i++) {
        long endVal = i == nums.length ? (long)upper + 1 : nums[i];
        if (startVal + 2 == endVal) {
            res.add(String.valueOf(startVal + 1));
        } else if (startVal + 2 < endVal) {
            res.add(String.valueOf(startVal + 1) + "->" + String.valueOf(endVal - 1));
        }
        startVal = endVal;
    }

    return res;
}
```

九章有个好点的解法（容易记点的），另开一个函数来构造range。传进去的是构造range的那个数。同样得用long来pass那些MAX，MIN。

```java
public List<String> findMissingRanges(int[] nums, int lower, int upper) {
    // write your code here
    List<String> res = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        addRange(res, lower, upper);
        return res;
    }

    addRange(res, lower, (long)nums[0] - 1);
    for (int i = 1; i < nums.length; i++) {
    // 这3行可以省，addRange的第一个判断会跳过，所以不用另外判断
    //    if (nums[i] == nums[i - 1] + 1) {
    //        continue;
    //    }
        addRange(res, (long)nums[i - 1] + 1, (long)nums[i] - 1);
    }
    addRange(res, (long)nums[nums.length - 1] + 1, upper);

    return res;
}

private void addRange(List<String> res, long start, long end) {
    if (start > end) {
        return;
    }

    if (start == end) {
        res.add(String.valueOf(start));// 可以直接res.add(st + "");
    }

    if (start < end) {
        res.add(String.valueOf(start) + "->" + String.valueOf(end));
        // 可以res.add(st + "->" + ed); 而且不用判断小于，因为就只剩下小于的情况了。
    }
}
```
