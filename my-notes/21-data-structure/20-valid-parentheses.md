# 20 Valid Parentheses

Given a string containing just the characters`'('`,`')'`,`'{'`,`'}'`,`'['`and`']'`, determine if the input string is valid.

The brackets must close in the correct order,`"()"`and`"()[]{}"`are all valid but`"(]"`and`"([)]"`are not.

做法不难，就是用一个栈，看到左括号就push看到右括号就pop。每次pop的时候看看栈顶，如果不是左括号的话返回false。然后最后把字符串处理完后还得检查栈是否为空，不为空表明不合法，有多余的右括号。

```java
//加一个简洁点的写法？
public boolean isValid(String s) {
    if (s == null || s.length() == 0) {
        return true;
    }

    Stack<Character> stack = new Stack<>();
    for (int i = 0; i < s.length(); i++) {
        char cur = s.charAt(i);
        if (cur == '(' || cur == '[' || cur == '{') {
            stack.push(cur);
        } else {
            if (stack.isEmpty()) {
                return false;
            } 
            if (cur == ')' && stack.peek() != '(') {
                return false;
            }

            if (cur == ']' && stack.peek() != '[') {
                return false;
            }

            if (cur == '}' && stack.peek() != '{') {
                return false;
            }
            stack.pop();
        }
    }

    return stack.isEmpty();
}

// if else
public boolean isValid(String s) {
    if (s == null || s.length() == 0) {
        return false;
    }

    Stack<Character> stack = new Stack<>();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (c == ')') {
            if (!stack.isEmpty() && stack.peek() == '(') {
                stack.pop();
            } else {
                return false;
            }
        } else if (c == ']') {
            if (!stack.isEmpty() && stack.peek() == '[') {
                stack.pop();
            } else {
                return false;
            }
        } else if (c == '}') {
            if (!stack.isEmpty() && stack.peek() == '{') {
                stack.pop();
            } else {
                return false;
            }
        } else {
            stack.push(c);
        }
    }
return stack.isEmpty();

// 加一个switch的写法：
public boolean isValid(String s) {
    if (s == null) {
        return false;
    }

    Stack<Character> stack = new Stack<>();
    for (int i = 0; i < s.length(); i++) {
        char cur = s.charAt(i);
        switch (cur) {
            case '(':
                stack.push(cur);
                break;
            case '[':
                stack.push(cur);
                break;
            case '{':
                stack.push(cur);
                break;
            case ')':
                if (stack.isEmpty() || stack.peek() != '(') {
                    return false;
                } else {
                    stack.pop();
                }
                break;
            case ']':
                if (stack.isEmpty() || stack.peek() != '[') {
                    return false;
                } else {
                    stack.pop();
                }
                break;
            case '}':
                if (stack.isEmpty() || stack.peek() != '{') {
                    return false;
                } else {
                    stack.pop();
                }
                break;
        }
    }
    return stack.isEmpty();
}
```
