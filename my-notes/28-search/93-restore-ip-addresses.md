# 93 Restore IP Addresses

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:\
Given`"25525511135"`,

return`["255.255.11.135", "255.255.111.35"]`. (Order does not matter)

这题感觉很经典的backtrack。因为ip地址分成四段，所以递归时最多也就4层。然后把字符串长度也传进递归函数中来快速去掉不合条件的分法，例如例子里，第一层去一个2.然后第二层就会有5525511135，这层长度最多也只能是9，所以可以跳过2.这个取法，去尝试25.之类的。另外，因为ip地址每一段最多只有3个字符，所以for loop3次都ok了。最后要注意的是，判断是否合法ip地址的函数，要判断截取出来的字符串是否以0开头，例如01，这就不是合法的ip地址段，要返回false。递归结束条件是递归到最后一层，没有点了，我们就把答案加上“.”放到result里返回。

```java
public List<String> restoreIpAddresses(String s) {
    List<String> res = new ArrayList<>();
    if (s == null || s.isEmpty()) {
        return res;
    }

    List<String> tmp = new ArrayList<>();
    dfsHelper(4, s, 12, tmp, res);

    return res;
}

private void dfsHelper(int dotLeft, String str, int len, List<String> tmp, List<String> res) {
    if (str.length() > len) {
        return;
    }

    if (dotLeft == 0) {
        StringBuilder sb = new StringBuilder();
        for (String seg : tmp) {
            sb.append(seg);
            sb.append(".");
        }
        sb.deleteCharAt(sb.length() - 1);
        res.add(sb.toString());
        return;
    }

    for (int i = 1; i <= 3 && i <= str.length(); i++) {
        String sub = str.substring(0, i);
        if (!valid(sub)) {
            continue;
        }
        tmp.add(sub);
        dfsHelper(dotLeft - 1, str.substring(i), len - 3, tmp, res);
        tmp.remove(tmp.size() - 1);
    }
}

private boolean valid(String s) {
    if (s.charAt(0) == '0' && s.length() > 1) {
        return false;
    }
    int num = Integer.valueOf(s);
    if (num < 0 || num > 255) {
        return false;
    } else {
        return true;
    }
}
```
