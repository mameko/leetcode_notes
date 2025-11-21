# 224 Basic Calculator

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open`(`and closing parentheses`)`, the plus`+`or minus sign`-`,**non-negative**integers and empty spaces.

You may assume that the given expression is always valid.

Some examples:

```
"1 + 1" = 2
" 2-1 + 2 " = 3
"(1+(4+5+2)-3)+(6+8)" = 23
```

这题主要注意在弹出栈后怎么把值赋到适合的变量里。还得注意num可能是多位数，最后记得把最后一位加/减上。

```java
public int calculate(String s) {
    if (s == null || s.isEmpty()) {
        return 0;
    }

    int sign = 1;
    int n = s.length();
    int num = 0;
    int res = 0;
    Stack<Integer> stack = new Stack<>();

    for (int i = 0; i < n; i++) {
        char cur = s.charAt(i);

        if (Character.isDigit(cur)) {
            num = num * 10 + Character.getNumericValue(cur);
        } else if (cur == '+' || cur == '-') {
            res += num * sign;
            num = 0;

            sign = cur == '+' ? 1 : -1;
        } else if (cur == '(') {
            stack.push(res);
            stack.push(sign);

            res = 0;
            sign = 1;
        } else if (cur == ')') {
            res += num * sign;
            sign = stack.pop();
            num = res;
            res = stack.pop();
        }
    }

    res += num * sign;

    return res;
}
```
