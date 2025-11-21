# 150 Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are`+`,`-`,`*`,`/`. Each operand may be an integer or another expression.

Some examples:

```
  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
```

用栈很容易解决，这里假设输入的是合法的reverse polish expression

```java
public int evalRPN(String[] tokens) {
    if (tokens == null || tokens.length == 0) {
        return 0;
    } 
    // assume only +, -, *, / and numbers in the tokens
    Stack<Integer> stack = new Stack<>();
    for (int i = 0; i < tokens.length; i++) {
        String cur = tokens[i];
        if (isOperator(cur)) {
            int op2 = stack.pop();
            int op1 = stack.pop();
            int res = 0;
            switch(cur) {
                case "+" : 
                    res = op1 + op2;
                    break;
                case "-" : 
                    res = op1 - op2;
                    break;
                case "*" :
                    res = op1 * op2;
                    break;
                case "/" :
                    res = op1 / op2;
                    break;
            }
            stack.push(res);
        } else {
            stack.push(Integer.valueOf(cur));
        }
    }

    return stack.pop();
}

private boolean isOperator(String s) {
    if (s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/")) {
        return true;
    }

    return false;
}

// switch版
public int evalRPN(String[] tokens) {
    if (tokens == null || tokens.length == 0) {
        return 0;
    }

    Stack<Integer> stack = new Stack<>();
    for (String s : tokens) {
        if (s.equals("+")) {
            int op2 = Integer.valueOf(stack.pop());
            int op1 = Integer.valueOf(stack.pop());
            stack.push(op1 + op2);
        } else if (s.equals("-")) {
            int op2 = Integer.valueOf(stack.pop());
            int op1 = Integer.valueOf(stack.pop());
            stack.push(op1 - op2);
        } else if (s.equals("*")) {
            int op2 = Integer.valueOf(stack.pop());
            int op1 = Integer.valueOf(stack.pop());
            stack.push(op1 * op2);
        } else if (s.equals("/")) {
            int op2 = Integer.valueOf(stack.pop());
            int op1 = Integer.valueOf(stack.pop());
            stack.push(op1 / op2);
        } else {
            stack.push(Integer.valueOf(s));
        }
    }

    return stack.peek();
}
```
