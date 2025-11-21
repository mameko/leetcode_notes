# 487 Max Consecutive Ones II

Given a binary array `nums`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most one_ `0`.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [1,0,1,1,0]
</strong><strong>Output: 4
</strong><strong>Explanation: 
</strong>- If we flip the first zero, nums becomes [1,1,1,1,0] and we have 4 consecutive ones.
- If we flip the second zero, nums becomes [1,0,1,1,1] and we have 3 consecutive ones.
The max number of consecutive ones is 4.
</code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [1,0,1,1,0,1]
</strong><strong>Output: 4
</strong><strong>Explanation: 
</strong>- If we flip the first zero, nums becomes [1,1,1,1,0,1] and we have 4 consecutive ones.
- If we flip the second zero, nums becomes [1,0,1,1,1,1] and we have 4 consecutive ones.
The max number of consecutive ones is 4.
</code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 105`
* `nums[i]` is either `0` or `1`.

&#x20;

**Follow up:** What if the input numbers come in one by one as an infinite stream? In other words, you can't store all numbers coming from the stream as it's too large to hold in memory. Could you solve it efficiently?

这题是真的得slide了。做法是，用一个boolean标注flip过了没有，这最多可以flip一次，所以没flip过的，就继续cnt++，同时记录一下flip的位置。flip过了，就移动left指向下一位，同时计算max。最后出了循环还得算一次max，因为最后一个没算进去。right已经跳出了。T:O(n), S:O(1)

```java
public int findMaxConsecutiveOnes(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int indFlipped = -1;
    int left = 0;
    int max = -1;        
    int cnt = 0;
    int right = 0;
    for (;right < nums.length; right++) {
        int cur = nums[right];
        if (cur == 1) {
            cnt++;
        } else {
            if (indFlipped == -1) {
                indFlipped = right;
                cnt++;
            } else {
                max = Math.max(max, right - left);
                left = indFlipped + 1;
                indFlipped = right;
            }
        }
    }

    max = Math.max(max, right - left);
    return max;
}
```
