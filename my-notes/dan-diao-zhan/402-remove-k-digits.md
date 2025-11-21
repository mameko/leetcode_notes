# 402 Remove K Digits

Given a non-negative integernumrepresented as a string, removekdigits from the number so that the new number is the smallest possible.

**Note:**

* The length of num is less than 10002 and will be ≥ k.
* The given num does not contain any leading zero.

**Example 1:**

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```

**Example 2:**

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```

**Example 3:**

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```

这题还有其他做法，这里就看懂了这个，先抄了，赶着面试。算法主要是如果入栈的数字比前面的小，就把前面大的那些pop出来，基本上感觉就是维护一个递增序列。（因为要找最小的数，所以小的digit在前最后得到的结果会比较小）然后因为我们要删除k个，素以最后只用保留前len - k个就好了。（因为后面的会比前面的大，这样就可以把大的删了）。然后因为像0200这样的答案不合法，所以最后要loop一下remove leading zeros。

```java
public String removeKdigits(String num, int k) {
    if (num == null || num.isEmpty()) {
        return "";
    }

    if (k == num.length()) {
        return "0";
    }

    int keep = num.length() - k;

    Stack<Character> stack = new Stack<>();
    char[] cs = num.toCharArray();

    for (char c : cs) {

        while (!stack.isEmpty() && k > 0 && c < stack.peek()) {
            stack.pop();
            k--;
        }

        stack.push(c);
    }

    // remove leading 0s
    StringBuilder sb = new StringBuilder();
    while (!stack.isEmpty()) {
        sb.insert(0, stack.pop());
    }

    sb.setLength(keep);

    int start = 0;
    for (start = 0; start < sb.length(); start++) {
        if (sb.charAt(start) != '0') {
            break;
        }
    }

    return start == sb.length() ? "0" : sb.substring(start);
}
```
