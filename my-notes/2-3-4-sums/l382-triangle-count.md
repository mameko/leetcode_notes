# L382 Triangle Count

Given an array of integers, how many three numbers can be found in the array, so that we can build an triangle whose three edges length is the three numbers that we find?

Have you met this question in a real interview?

Yes

**Example**

Given array S =`[3,4,6,7]`, return`3`. They are:

```
[3,4,6]
[3,6,7]
[4,6,7]
```

Given array S =`[4,4,4,4]`, return`4`. They are:

```
[4(1),4(2),4(3)]
[4(1),4(2),4(4)]
[4(1),4(3),4(4)]
[4(2),4(3),4(4)]
```

三角形成立的条件是：A + B > C && A + C > B && B + C > A。假设A < B < C（题目要求的输出），那么后两个条件已经成立。要找的就只剩下令A + B > C 成立的3个元素。就是求2 sum > target。这里target跟能挑的数都在一个数组里。其实跟[259 3 Sum Smaller](259-3-sum-smaller.md)挺象。这题是3 sum bigger。

几年后，再去九章上课，老师提出了一个有趣的follow up，如果我们要求的是有没有解，那么我们排完序之后，直接一个loop找相邻的3个数是不是满足，A + B > C就ok了。譬如，\[2, 2, 7, 10, 20, 2]，我们排完序，同样符合了一开始那个条件，所以只要找剩下的那个就好了。虽然也是O(n)，但不用2ptr那么复杂。

另外如果要求的是具体方案，那么要O(n ^ 3)

```java
 public int triangleCount(int S[]) {
    if (S == null || S.length < 3) {
        return Integer.MAX_VALUE;
    }

    int res = 0;
    Arrays.sort(S);
    for (int i = 2; i < S.length; i++) {
        int left = 0;
        int right = i - 1;
        while (left < right) {
            int sum = S[left] + S[right];
            if (sum > S[i]) {
                res += right - left;
                right--;
            } else {
                left++;
            }
        }
    }
    return res;
}

// leetcode 版
public int triangleNumber(int[] nums) {
    if (nums == null || nums.length < 3) {
        return 0;
    }

    int count = 0;
    Arrays.sort(nums);

    for (int i = 2; i < nums.length; i++) {
        int left = 0;
        int right = i - 1;
        while (left < right) {
            int sum = nums[left] + nums[right];
            if (sum > nums[i]) {
                count += right - left;
                right--;
            } else {
                left++;
            }
        }
    }
    return count;
```
