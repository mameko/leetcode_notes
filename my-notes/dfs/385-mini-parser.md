# 385 Mini Parser

Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Note:**&#x59;ou may assume that the string is well-formed:

* String is non-empty.
* String does not contain white spaces.
* String contains only digits`0-9`,`[`,`-,`,`]`.

**Example 1:**

```
Given s = "324",

You should return a NestedInteger object which contains a single integer 324.
```

**Example 2:**

```
Given s = "[123,[456,[789]]]",

Return a NestedInteger object containing a nested list with 2 elements:

1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789.
```

这题实现的时候得非常小心，一开始想的时候，本来打算像calculator那两题那样一圈一圈算数字，后来发现可以截一段string出来直接用Integer来转换。第二个需要注意的点是，可以把‘，’与‘]’放在一起处理。然后还得注意最后如果‘]'这种情况怎么pop。还有注意start的位置，以免截取错了。

```java
public NestedInteger deserialize(String s) {
    if (s == null || s.isEmpty()) {
        return new NestedInteger();
    } 

    if (s.charAt(0) != '[') {
        return new NestedInteger(Integer.valueOf(s));
    }

    NestedInteger res = new NestedInteger();
    Stack<NestedInteger> stack = new Stack<>();
    int start = 1;

    for (int end = 0; end < s.length(); end++) {
        char cur = s.charAt(end);
        if (cur == '[') {
            stack.push(new NestedInteger());
            start = end + 1;// increase start position
        } else if (cur == ',' || cur == ']') {
            if (start < end) {// prevent "[]"
                NestedInteger num = new NestedInteger(Integer.valueOf(s.substring(start, end)));
                stack.peek().add(num);
            }

            if (cur == ']') {
                if (stack.size() > 1) {// add upper ones to lower ones
                    NestedInteger last = stack.pop();
                    stack.peek().add(last);
                }
            }
            start = end + 1;// increase start position
        }
    }

    return stack.peek();
}
```
