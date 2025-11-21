# 439 Ternary Expression Parser

Given a string representing arbitrarily nested ternary expressions, calculate the result of the expression. You can always assume that the given expression is valid and only consists of digits`0-9`,`?`,`:`,`T`and`F`(`T`and`F`represent True and False respectively).

**Note:**

1. The length of the given string is ≤ 10000.
2. Each number will contain only one digit.
3. The conditional expressions group right-to-left (as usual in most languages).
4. The condition will always be either`T`or`F`. That is, the condition will never be a digit.
5. The result of the expression will always evaluate to either a digit`0-9`,`T`or`F`.

**Example 1:**

```
Input: "T?2:3"
Output: "2"

Explanation: If true, then result is 2; otherwise result is 3.
```

**Example 2:**

```
Input: "F?1:T?4:5"
Output: "4"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(F ? 1 : (T ? 4 : 5))"                   "(F ? 1 : (T ? 4 : 5))"
          -> "(F ? 1 : 4)"                 or       -> "(T ? 4 : 5)"
          -> "4"                                    -> "4"
```

**Example 3:**

```
Input: "T?T?F:5:3"
Output: "F"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(T ? (T ? F : 5) : 3)"                   "(T ? (T ? F : 5) : 3)"
          -> "(T ? F : 3)"                 or       -> "(T ? F : 5)"
          -> "F"                                    -> "F"
```

这题原来是用stack，用string concatenation虽然行，不过很慢，没预处理的话会TEL。看了discuss里大神的解释才知道是用stack。

```java
public String parseTernary(String expression) {
    if (expression == null || expression.isEmpty()) {
        return "";
    }

    StringBuilder sb = new StringBuilder(expression);

    while (sb.length() > 1) {
        int n = sb.length();
        int i = n - 5;
        for (i = n - 5; i >= 0; i--) {
            // skip those definitely not valid or you will TEL
            if (sb.charAt(i) != 'T' && sb.charAt(i) != 'F') {
                continue;
            }
            if (matchPattern(sb.substring(i, i + 5))) {
                break;
            }
        }

        StringBuilder tmp = new StringBuilder();

        tmp.append(sb.substring(0, i));
        tmp.append(eval(sb.substring(i, i + 5)));
        if (i < n - 5) {
            tmp.append(sb.substring(i + 5));
        }
        sb = tmp;
    }

    return sb.toString();
}

private char eval(String sb) {
    if (sb.charAt(0) == 'T') {
        return sb.charAt(2);
    } else {
        return sb.charAt(4);
    }
}

private boolean matchPattern(String sb) {
    if (sb.charAt(0) != 'T' && sb.charAt(0) != 'F') {
        return false;
    }

    if (sb.charAt(1) != '?') {
        return false;
    }

    if (sb.charAt(3) != ':') {
        return false;
    }

    if (!Character.isDigit(sb.charAt(2)) && sb.charAt(2) != 'T' && sb.charAt(2) != 'F') {
        return false;
    }

    if (!Character.isDigit(sb.charAt(4)) && sb.charAt(4) != 'T' && sb.charAt(4) != 'F') {
        return false;
    }

    return true;
}
```

大神手稿：（还是有点慢，不过好理解多了）

```java
public String parseTernary(String expression) {
    if (expression == null || expression.isEmpty()) {
        return "";
    }

    int n = expression.length();
    Stack<Character> stack = new Stack<>();
    for (int i = n - 1; i >= 0; i--) {
        char cur = expression.charAt(i);
        if (!stack.isEmpty() && stack.peek() == '?') {
            stack.pop();// pop '?'
            char n1 = stack.pop();
            stack.pop();// pop ':'
            char n2 = stack.pop();

            if (cur == 'T') {
                stack.push(n1);
            } else {
                stack.push(n2);
            }
        } else {
            stack.push(cur);
        }
    }

    return Character.toString(stack.pop());
}
```
