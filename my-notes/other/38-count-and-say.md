# 38 Count and Say

The count-and-say sequence is the sequence of integers beginning as follows:\
`1, 11, 21, 1211, 111221, ...`

`1`is read off as`"one 1"`or`11`.\
`11`is read off as`"two 1s"`or`21`.\
`21`is read off as`"one 2`, then`one 1"`or`1211`.

Given an integern, generate thenthsequence.

Note: The sequence of integers will be represented as a string.

这题虽然是easy，但一开始没想到做法。上网看了人家怎么做。首先把“1”放进结果里，然后循环着数这个res里每个数字的个数。其实有想过，如果n很大的话，我们要不要看两位呢？但在eclipse上调试过（把中间结果打出来）知道n=35为止都只有1，2，3出现。然后n=50就跑不动了。 除了1,2,3之外，没有其他数字，除非初始的种子使用了其他数字，或者初始种子包含连续三个以上的相同数字，具体请参照维基百科[Look-and-say sequence](https://en.wikipedia.org/wiki/Look-and-say_sequence)。

```java
public String countAndSay(int n) {
    if (n < 1) {
        return "";
    }

    String res = "1";
    int i = 1;
    while (i < n) {
        int count = 1;
        StringBuilder sb = new StringBuilder();
        for (int j = 1; j < res.length(); j++) {
            if (res.charAt(j) == res.charAt(j - 1)) {
                count++;
            } else {
                sb.append(count);
                sb.append(res.charAt(j - 1));
                count = 1;
            }
        }

        sb.append(count);
        sb.append(res.charAt(res.length() - 1));
        res = sb.toString();
        i++;
    }
    return res;
}
```
