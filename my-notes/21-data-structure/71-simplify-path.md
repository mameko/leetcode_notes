# 71 Simplify Path

Given an absolute path for a file (Unix-style), simplify it.

For example,\
**path**=`"/home/"`, =>`"/home"`\
**path**=`"/a/./b/../../c/"`, =>`"/c"`

**Corner Cases:**

*   Did you consider the case where

    **path**=`"/../"`?&#x20;

    In this case, you should return`"/"`

    .
*   Another corner case is the path might contain multiple slashes`'/'`together, such as`"/home//foo/"`.

    In this case, you should ignore redundant slashes and return`"/home/foo"`.

这题不是特别难，主要是注意corner case怎么跳过。

```java
public String simplifyPath(String path) {
    if (path == null || path.isEmpty()) {
        return "";
    }

    String[] parts = path.split("/");
    Stack<String> s = new Stack<>();
    for (String part : parts) {
        if (part.equals(".") || part.isEmpty()) {
            continue;
        } else if (part.equals("..")) {
            if (!s.isEmpty()) {
                s.pop();
            }    
        } else {
            s.push(part);
        }
    }

    StringBuilder result = new StringBuilder();
    while (!s.isEmpty()) {
        result.insert(0, s.pop());
        result.insert(0, "/");
    }

    return result.length() == 0 ? "/" : result.toString();
}
```
