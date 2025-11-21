# 228 Summary Ranges

Given a sorted integer array without duplicates, return the summary of its ranges.

For example, given`[0,1,2,4,5,7]`, return`["0->2","4->5","7"].`

这题得分情况处理，非常麻烦，所以我多加一个数在后面，但不是真正地加。只是想像成多一个数。

例如：\[0,1,2,4,5,7] =》 \[0,1,2,4,5,7, MIN]

然后每次都把end指针向后移，边移边找区间。找到后加入结果然后更新start指针。

end指针会指着区间的下一个数。所以加进结果时要-1.

简洁版：

```java
public List<String> summaryRanges(int[] nums) {
    List<String> result = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return result;
    }

    int start = 0;// starting point of the range
    int n = nums.length;

    // end is next number after the end of a range
    for (int end = start + 1; end <= n; end++) {
        if (end == n || nums[end] != nums[end - 1] + 1) {
            if (start == end - 1) {// this range only consist of 1 number
                result.add(String.valueOf(nums[end - 1]));
            } else {// this range has more than 1 number
                result.add(nums[start] + "->" + nums[end - 1]);
            }
            start = end;// after we add a range to result, we update our starting point to
        } 
    }

    return result;
}
```

详细版：其实就是除了第二个case以外都可以加结果

```java
/*
      start : starting point of the range
      end: 1 slot after range

      need to seperate different cases.
      detail version
*/
public List<String> summaryRanges(int[] nums) {
    List<String> result = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return result;
    }

    int start = 0;
    int n = nums.length;

    for (int end = start + 1; end <= n; end++) {
        if (end == n) {
            if (start == end - 1) {
                result.add(String.valueOf(nums[end - 1]));
            } else {
                result.add(nums[start] + "->" + nums[end - 1]);
            }
        } else if (nums[end] == nums[end - 1] + 1) {
            continue;
        } else {
            if (start == end - 1) {
                result.add(String.valueOf(nums[end - 1]));
            } else {
                result.add(nums[start] + "->" + nums[end - 1]);
            }
        }
        start = end;
    }

    return result;
}
```
