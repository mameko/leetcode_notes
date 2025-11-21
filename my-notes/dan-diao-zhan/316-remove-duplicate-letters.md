# 316 Remove Duplicate Letters

Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

**Example:**

Given`"bcabc"`\
Return`"abc"`

Given`"cbacdcbc"`\
Return`"acdb"`

这题感觉看懂都有点难度，也有几种做法，这里用了栈。具体是，删除重复的，但最终结果要保持lexical order。例如第二个例子里，虽然c在前面有后面也有，我们要留中间那个（a后面d前面的那个c）。具体做法看注释。

```java
public String removeDuplicateLetters(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }

    char[] cs = s.toCharArray();
    Stack<Character> stack = new Stack<>();
    Set<Character> visited = new HashSet<>();
    Map<Character, Integer> count = new HashMap<>();

    // get the counts for each char
    for (char c : cs) {
        if (count.containsKey(c)) {
            count.put(c, count.get(c) + 1);
        } else {
            count.put(c, 1);
        }
    }

    for (int i = 0; i < cs.length; i++) {
        char c = cs[i];
        count.put(c, count.get(c) - 1);

        // if stack contains the char already, we skip
        if (visited.contains(c)) {
            continue;
        }

        // remove chars that are dup but has larger lexicalgraphic order in stack
        // count.get(c) > 0 means there are dup still to come. 
        // eg stack = bc, remaining abc, a can pop b,c because there are b, c after a
        while (!stack.isEmpty() && c < stack.peek() && count.get(stack.peek()) > 0) {
            visited.remove(stack.pop());
        }

        stack.push(c);
        visited.add(c);
    }

    StringBuilder sb = new StringBuilder();
    while (!stack.isEmpty()) {
        sb.insert(0, stack.pop());
    }

    return sb.toString();
}
```
