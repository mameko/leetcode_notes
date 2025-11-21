# 267 Palindrome Permutaion II

Given a string`s`, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

For example:

Given`s = "aabb"`, return`["abba", "baab"]`.

Given`s = "abc"`, return`[]`.

**Hint:**

1. If a palindromic permutation exists, we just need to generate the first half of the string.
2.  To generate all distinct permutations of a (half of) string, use a similar approach from:

    [Permutations II](https://leetcode.com/problems/permutations-ii) or [Next Permutation](https://leetcode.com/problems/next-permutation)

这题是把[266 Palindrome Permuation](../palindrome/266-palindrome-permuation.md)跟permutation结合了。没什么特别难的地方。因为题目提示说可以生成前半段，所以得找出前半段的chars。递归结束条件是生成了前半段，然后把中间字符加上，最后把生成的前半段reverse一下，三个加起来组成palindrome。记得在reverse之后要reverse回来，不然的话，递归出去以后，tmp.remove会删错字符。

```java
public List<String> generatePalindromes(String s) {
    List<String> res = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return res;
    }

    // 1. check if we can form a palindrom
    HashMap<Character, Integer> hm = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (hm.containsKey(c)) {
            hm.put(c, hm.get(c) + 1);
        } else {
            hm.put(c, 1);
        }
    }

    String single = "";
    StringBuilder sb = new StringBuilder();
    for (Map.Entry<Character,Integer> e : hm.entrySet()) {
        int num = e.getValue();
        if (num % 2 != 0) {
            if (!single.equals("")) {// 已经有一个落单的数字了，再看见一个
                return res;// 不能组成parlindme，返回空
            } else {
                single = String.valueOf(e.getKey());
            }
        } 
        // 拿出要加到halfchars里（candidates）的字符
        num = num / 2; // 因为只算一半，所以线除以二
        while (num > 0) {
            sb.append(e.getKey());
            num--;
        }
    }

    // 2. recursive search for them
    String halfchars = sb.toString();
    char[] cs = halfchars.toCharArray();
    Arrays.sort(cs);

    boolean[] visited = new boolean[cs.length];
    StringBuilder tmp = new StringBuilder();
    dfsHelper(tmp, res, single, 0, cs, visited);

    return res;
}

private void dfsHelper(StringBuilder tmp, List<String> res, String single, 
                       int start, char[] cs, boolean[] visited) {
    if (tmp.length() == cs.length) {
        //生成前半段后，加上中间字符（odd的那个）+reverse前半段
        String result = tmp.toString() + single;
        res.add(result + tmp.reverse().toString());
        tmp.reverse();//记得reverse回原来样子，不然下面会删错char
        return;
    }

    for (int i = 0; i < cs.length; i++) {
        if (i != 0 && !visited[i - 1] && cs[i] == cs[i - 1]) {
            continue;
        }

        if (!visited[i]) {
            tmp.append(cs[i]);
            visited[i] = true;
            dfsHelper(tmp, res, single, start + 1, cs, visited);
            visited[i] = false;
            tmp.deleteCharAt(tmp.length() - 1);//下面指的是这里。
        }
    }
}
```
