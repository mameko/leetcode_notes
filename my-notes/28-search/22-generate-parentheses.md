# 22 Generate Parentheses

Givennpairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, givenn= 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

这题做法是找括号生成的规律，一开始自己做的时候找错了，后来看到cc189的做法。每个左括号的后面加一对，最后再在前面加一对，这样就能生成所有结果了，因为有重复，所以要用hashset去重。其实这个方法有点慢，因为会产生很多重复的结果。（好像说是catlan number那么多个）后来看了递归的方法，也写下来。这个快点，只用了O(2^n)时间和O(n)空间。每次取"("或")"两种选择，取n次，所以O(2^n)，递归深度n层。

```java
public ArrayList<String> generateParenthesis(int n) {
    ArrayList<String> res = new ArrayList<>();
    if (n < 1) {
        return res;
    }

    StringBuilder tmp = new StringBuilder();
    dfsHelper(res, tmp, n, n);
    return res;
}

private void dfsHelper(ArrayList<String> res, StringBuilder sb, int left, int right) {

    if (left == 0 && right == 0) {
        res.add(sb.toString());
        return;
    }

    if (left > 0) {
        sb.append('(');
        dfsHelper(res, sb, left - 1, right);
        sb.deleteCharAt(sb.length() - 1);
    }

    if (right > left) {
        sb.append(')');
        dfsHelper(res, sb, left, right - 1);
        sb.deleteCharAt(sb.length() - 1);
    }

}
```

```java
public List<String> generateParenthesis(int n) {
    List<String> res = new ArrayList<>();
    if (n < 1) {
        return res;
    }

    res.add("");
    for (int ind = 0; ind < n; ind++) {
        HashSet<String> cur = new HashSet<>();
        for (String item : res) {
            for (int i = 0; i < item.length(); i++) {
                if (item.charAt(i) == '(') {
                    String tmp = item.substring(0, i + 1) + "()" + item.substring(i + 1);
                    cur.add(tmp);
                }
            }

            cur.add("()" + item);
        }
        res = new ArrayList<>(cur);
    }

    return res;
}
```
