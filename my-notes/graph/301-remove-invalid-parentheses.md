# 301 Remove Invalid Parentheses

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses`(`and`)`.

**Examples:**

```
"()())()" -> ["()()()", "(())()"]
"(a)())()" -> ["(a)()()", "(a())()"]
")(" -> [""]
```

这题真的很难，不看discuss大神们答案都不知道怎么解。看了答案也就看懂了bfs，所以写了（调了）一遍。基本思路是，先把原来的s放进queue里，然后每次从队列里拿出来，看看是否是合法的，如果是的话，标记我们已经找到了。因为是bfs，所以找到了以后，我们可以保证是删掉最少数量的括号得到合法串。如果发现不合法，我们试图去掉一个括号，用一个for生成所有去掉括号以后的串，放进队列里，用visited来去掉重复串，因为删了括号以后可能会产生重复。然后我们在这一层找，如果再找不到，我们就进入下一层，再去一个括号。如此类推，知道最后队列空了，我们把所有括号去掉了，就返回。还有，这里用了个数括号的方法来避免用栈。这货复杂度很高，每次判断valid要O(n)然后生成括号可能需要2^(n - 1)，所以时间复杂度是：O(n \* 2 ^ (n - 1))

```java
public List<String> removeInvalidParentheses(String s) {
    List<String> res = new ArrayList<>();
    if (s == null) {
        return res;
    }

    HashSet<String> visited = new HashSet<>();
    Queue<String> q = new LinkedList<>();

    q.offer(s);
    visited.add(s);
    boolean found = false;

    while (!q.isEmpty()) {
        s = q.poll();

        if (isValid(s)) {
            res.add(s);
            found = true;
        }

        if (found) {
            continue;
        }

        for (int i = 0; i < s.length(); i++) {
                if (s.charAt(i) != '(' && s.charAt(i) != ')') {
                    continue;
                }

                String sub = s.substring(0, i) + s.substring(i + 1);
                if (!visited.contains(sub)) {
                    visited.add(sub);
                    q.offer(sub);
                }
            }
    }

    return res;
}

private boolean isValid(String s) {
    int cnt = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(') {
            cnt++;
        } 

        if (s.charAt(i) == ')') {
            cnt--;
            if (cnt < 0) {
                return false;
            }
        }
    }

    return cnt == 0;
}
```
