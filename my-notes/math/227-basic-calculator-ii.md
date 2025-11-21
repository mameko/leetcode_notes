# 227 Basic Calculator II

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only**non-negative**integers,`+`,`-`,`*`,`/`operators and empty spaces. The integer division should truncate toward zero.

You may assume that the given expression is always valid.

Some examples:

```
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5
```

这题的忠告是，小心符号。一开始时分了两个栈，一个符号，另一个放数字。后来发现可以把加减号跟数字结合起来，如果加号的话，把+res放进栈里，如果是减号的话，把-res放进栈里。不过记得每次是算完前面再push，push完以后再把下一个的sign设好正负然后继续。

```java
public int calculate(String s) {
    if (s == null || s.isEmpty()) {
        return 0;
    }

    int n = s.length();
    int num = 0;
    int res = 0;
    char op = '+';
    Stack<Integer> nStack = new Stack<>();
    int sign = 1;

    for (int i = 0; i < n; i++) {
        char cur = s.charAt(i);

        if (Character.isDigit(cur)) {
            num = num * 10 + Character.getNumericValue(cur);
        } else if (cur == '+' || cur == '-') {
            res = calc(res, op, num);                
            op = cur;
            num = 0;
        } else if (cur == '*' || cur == '/') {
            if (op == '*' || op == '/') {
                res = calc(res, op, num);
            } else if (op == '+' || op == '-') {                     
                nStack.push(res);
                sign = op == '+' ? 1 : -1;
                res = num * sign;
            }
            op = cur;
            num = 0;
        }
    }

    res = calc(res, op, num);

    while (!nStack.isEmpty()) {
        num = nStack.pop();
        res = calc(num, '+', res);
    }

    return res;
}

private int calc(int n1, char op, int n2) {
    switch (op) {
    case '+':
        return n1 + n2;
    case '-':
        return n1 - n2;
    case '*':
        return n1 * n2;
    case '/':
        return n1 / n2;
    }

    return 0;
}
```
