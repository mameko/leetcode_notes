# 1249 Minimum Remove to Make Valid Parentheses

Given a string s of `'('` , `')'` and lowercase English characters.&#x20;

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting _parentheses string_ is valid and return **any** valid string.

Formally, a _parentheses string_ is valid if and only if:

* It is the empty string, contains only lowercase characters, or
* It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
* It can be written as `(A)`, where `A` is a valid string.

**Example 1:**

```
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
```

**Example 2:**

```
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

**Example 3:**

```
Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.
```

**Example 4:**

```
Input: s = "(a(b(c)d)"
Output: "a(b(c)d)"
```

**Constraints:**

* `1 <= s.length <= 10^5`
* `s[i]` is one of  `'('` , `')'` and lowercase English letters`.`

一看，还以为是[301 Remove Invalid Parentheses](../graph/301-remove-invalid-parentheses.md)呢。其实这我就用[20 Valid Parentheses](20-valid-parentheses.md)那题的办法。把要remove的括号放到deque里。一开始是用栈的，因为我们要返回remove了之后的字符串。所以用deque方便点。T:O(n), S:O(n)

```java
public String minRemoveToMakeValid(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }

    // parenthesis's location
    Deque<Integer> deque = new LinkedList<>();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (c == ')' && !deque.isEmpty() && s.charAt(deque.peekLast()) == '(') {
            deque.removeLast();
        } else if (c == '(' || c == ')') {
            deque.addLast(i);
        }
    }

    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {            
        if (!deque.isEmpty() && i == deque.getFirst()) {
            deque.removeFirst();
            continue;
        }
        sb.append(s.charAt(i));
    }

    return sb.toString();
}
```
