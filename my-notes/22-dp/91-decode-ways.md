# 91 Decode Ways

A message containing letters from`A-Z`is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given an encoded message containing digits, determine the total number of ways to decode it.

**Example**

Given encoded message`12`, it could be decoded as`AB`(1 2) or`L`(12).\
The number of ways decoding`12`is 2.

dp数组表示的是，到目前为止，能有多少种decode的方法。因为字母只有26个，所以能有多少种decode的方法，要么把现在这个char看成一个来decode，要么跟前面那个char连在一起作为一个两位数decode。dp\[0]表示把0到2这一段的decdoe方法，有1种。（其实这里感觉解释有点牵强，就是这样初始化dp才能得到正确结果，不知为毛）这里要注意dp\[1]的初始化，一开始我把dp\[1]初始化为1，这是不对的，应该根据第一个char来判定，因为如果是‘0’的话，这个string是无法decode（0必须跟前一个连在一起decode，这里没有前一个，所以不能decode）。所以要初始化为0（0表示不能decode）。for里面判断时还得注意，如果遇到的是0的话，我们只能跟前面连在一起decode。然后算两位数时，判断是否合法的两位数，（10 到 26），如果是91的话，我们不能把这两个连在一起算。这时要看dp\[i - 1]的值。T:O(n)，S:O(n)

```java
public int numDecodings(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int[] nums = new int[s.length() + 1];
    nums[0] = 1;
    nums[1] = s.charAt(0) != '0' ? 1 : 0;
    for (int i = 2; i <= s.length(); i++) {
        if (s.charAt(i - 1) != '0') {
            nums[i] = nums[i - 1];
        }

        int twoDigits = (s.charAt(i - 2) - '0') * 10 + s.charAt(i - 1) - '0';
        if (twoDigits >= 10 && twoDigits <= 26) {
            nums[i] += nums[i - 2];
        }
    }
    return nums[s.length()];
}
```
