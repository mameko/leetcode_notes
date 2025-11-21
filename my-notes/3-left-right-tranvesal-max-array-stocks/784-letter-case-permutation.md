# 784 Letter Case Permutation

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.

Return _a list of all possible strings we could create_. You can return the output in **any order**.

**Example 1:**

```
Input: S = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]
```

**Example 2:**

```
Input: S = "3z4"
Output: ["3z4","3Z4"]
```

**Example 3:**

```
Input: S = "12345"
Output: ["12345"]
```

**Example 4:**

```
Input: S = "0"
Output: ["0"]
```

**Constraints:**

* `S` will be a string with length between `1` and `12`.
* `S` will consist only of letters or digits.

竟然想了一个早上，而且还不知道怎么去重，得用set。其实思路不难，就是套递归的模板。T:O(2^n \* N), S:O(2^n \* N)。应该会有更好的去重方法的。有空再补solution

```java
 public List<String> letterCasePermutation(String S) {
    List<String> res = new ArrayList<>();
    if (S == null || S.isEmpty()) {
        return res;
    }

    S = S.toLowerCase();
    StringBuilder tmp = new StringBuilder();
    Set<String> resTmp = new HashSet<>();
    helper(tmp, S, resTmp, 0);
    // 用set去重，因为数字也会生成重复的答案
    res.addAll(resTmp);
    return res;
}

private void helper(StringBuilder tmp, String S, Set<String> res, int start) {
    if (start == S.length()) {
        res.add(tmp.toString());
        return;
    }

    // 只能挑大写或小写，所以是2
    for (int i = 0; i < 2; i++) {
        char cur = S.charAt(start);

        if (i == 1) {
            cur = Character.toUpperCase(cur);
        }
        tmp.append(cur);
        helper(tmp, S, res, start + 1);
        tmp.deleteCharAt(tmp.length() - 1);
    }
}
```
