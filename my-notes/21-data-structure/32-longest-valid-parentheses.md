# 32 Longest Valid Parentheses

Given a string containing just the characters`'('`and`')'`, find the length of the longest valid (well-formed) parentheses substring.

For`"(()"`, the longest valid parentheses substring is`"()"`, which has length = 2.

Another example is`")()())"`, where the longest valid parentheses substring is`"()()"`, which has length = 4.

看了答案才知道怎么做，真心有点刷不动...

解法一：用栈，首先要用一个start变量记录合法的起始位置。然后操作就像判断合法括号那样，左括号入栈，右括号出栈。这里要注意的是，每当我们有过多的右括号的时候，我们把其实位置start重置到下一个字符。然后，每次找到合法串时算一下长度。最后取长度的max。这里算长度的时候要注意，如果栈不为空的话，我们可以看看栈里最后一个没被匹配的左括号的位置。如果栈为空，我们减去start + 1.

```java
public int longestValidParentheses(String s) {
    if (s == null || s.isEmpty()) {
        return 0;
    }

    int max = 0;
    int start = 0;
    Stack<Integer> stack = new Stack<>();
    for (int i = 0; i < s.length(); i++) {
        char cur = s.charAt(i);
        if (cur == '(') {
            stack.push(i);
        } else {
            if (stack.isEmpty()) {
                start = i + 1;
            } else {
                stack.pop();
                int len = stack.isEmpty() ? i - start + 1 : i - stack.peek();
                max = Math.max(max, len);
            }
        }
    }

    return max;
}
```

看了tutorial发现还有另外一种stack的写法，比较简洁。不用记录start位置，一开始push一个-1进去解决栈空时+1的问题。然后每次pop完以后如果栈空，表示有多余的右括号，这时，把右括号的下标push进stack里，下次减的时候刚好是正确的位置。

```java
public class Solution {

    public int longestValidParentheses(String s) {
        int maxans = 0;
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.empty()) {
                    stack.push(i);
                } else {
                    maxans = Math.max(maxans, i - stack.peek());
                }
            }
        }
        return maxans;
    }
}
```

解法二：DP看tutorial看得似懂非懂...先抄下来参考

```java
public class Solution {
    public int longestValidParentheses(String s) {
        int maxans = 0;
        int dp[] = new int[s.length()];
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == ')') {
                if (s.charAt(i - 1) == '(') {
                    dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;// ...()
                } else if (i - dp[i - 1] > 0 && s.charAt(i - dp[i - 1] - 1) == '(') {// ...))
                    dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
                }
                maxans = Math.max(maxans, dp[i]);
            }
        }
        return maxans;
    }
}
```

还有解法三：O(1)空间，左右扫面数括号...好高深...

```java
public class Solution {
    public int longestValidParentheses(String s) {
        int left = 0, right = 0, maxlength = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxlength = Math.max(maxlength, 2 * right);
            } else if (right >= left) {
                left = right = 0;
            }
        }
        left = right = 0;
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxlength = Math.max(maxlength, 2 * left);
            } else if (left >= right) {
                left = right = 0;
            }
        }
        return maxlength;
    }
}
```
