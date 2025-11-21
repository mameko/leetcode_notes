# 43 Multiply Strings

Given two non-negative integers`num1`and`num2`represented as strings, return the product of`num1`and`num2`.

**Note:**

1. The length of both`num1`and`num2`is < 110.
2. Both`num1`and`num2`contains only digits`0-9`.
3. Both`num1`and`num2`does not contain any leading zero.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

这个看解释。首先得注意的是，我们从左到右process string，所以乘积的array是反过来的。然后记住array的长度是两个数的长度之和，例如9 × 9 = 81， 结果长度是num1的长度+num2的长度。然后处理时注意把前缀0去掉，不然0 \* 0 = 00这就不对了。最后记得把答案翻转过来。ans\[i + j] += a\[i] \* b\[j]，然后一次性进位。

容易记点的解法：

```java
public String multiply(String num1, String num2) {
    if (num1 == null || num2 == null || num1.length() == 0 || num2.length() == 0) {
        return "";
    }

    int n = num1.length();
    int m = num2.length();
    int len = m + n; // n × m位数最后到一个m+n位数

    int[] res = new int[len];

    // 记住num1和num2从后面开始取
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) { 
            res[i + j] += (num1.charAt(n - 1 - i) - '0') * (num2.charAt(m - 1 - j) - '0');
        }
    }

    // 得到一串反过来的答案
    StringBuilder sb = new StringBuilder();
    int carry = 0;
    for (int i = 0; i < len; i++) {
        int tmp = res[i] + carry;
        res[i] = tmp % 10;
        carry = tmp / 10;
        sb.append(res[i]);
    }

    // 把高位的前置0去掉
    int ind = len - 1;
    while (ind > 0 && res[ind] == 0) {
        sb.deleteCharAt(sb.length() - 1);
        ind--;
    }

    // 最后反过来
    return sb.reverse().toString();
}
```

```java
public String multiply(String num1, String num2) {
    if (num1 == null || num2 == null) {
        return "0";
    }

    int n1 = num1.length();
    int n2 = num2.length();
    int k = n1 + n2 - 2;
    // eg. 21 * 9
    int[] parts = new int[n1 + n2];
    for (int i = 0; i < n1; i++) {
        for (int j = 0; j < n2; j++) {
            parts[k - i - j] += Character.getNumericValue(num1.charAt(i)) * Character.getNumericValue(num2.charAt(j));
        }
    }
    // parts[9, 8, 1]
    int carry = 0;
    for (int cur = 0; cur < parts.length; cur++) {
        int tmp = parts[cur] + carry;
        parts[cur] = tmp % 10;
        carry = tmp / 10;
    }

    // remove "preceeding 0s", right side is the front
    int ind = parts.length - 1;
    while (ind > 0 && parts[ind] == 0) {
        ind--;
    }

    // construct a number string reversely
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i <= ind; i++) {
        sb.insert(0, parts[i]);
    }

    String res = sb.toString();
    return res.isEmpty() ? "0" : res;
}
```
