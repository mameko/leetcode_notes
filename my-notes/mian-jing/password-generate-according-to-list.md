# password generate according to list

给一个字符串和一堆可替换的字母，输出所有可能的组合。例子：str = “password”， hm = \[\[a, {@}], \[o, {0, +}]] 输出：\[p@ssw0rd, p@ssw+rd]

这题做法比较像[sudoku solver](../28-search/37-sudoku-solver.md)不太像[combination sum](../3-left-right-tranvesal-max-array-stocks/39-combination-sum-l135.md)或[subset](../3-left-right-tranvesal-max-array-stocks/l17-subsets.md)那一类，其实也有点像[pattern 2](../28-search/291-word-pttern-ii.md)

```java
public List<String> generate(Map<Character, List<Character>> hm, String pw) {
    List<String> res = new ArrayList<>();
    if (hm == null || hm.isEmpty() || pw == null || pw.isEmpty()) {
        return res;
    }

    char[] pwChar = pw.toCharArray();
    StringBuilder tmp = new StringBuilder();
    dfs(hm, res, tmp, 0, pwChar);
    return res;
}

private void dfs(Map<Character, List<Character>> hm, List<String> res, StringBuilder tmp, int start,
        char[] pwChar) {
    if (start >= pwChar.length) {
        res.add(tmp.toString());
        return;
    }

    char cur = pwChar[start];
    if (!hm.containsKey(cur)) {
        tmp.append(cur);
        dfs(hm, res, tmp, start + 1, pwChar);
    } else {
        List<Character> options = hm.get(cur);
        for (Character option : options) {
            tmp.append(option);
            dfs(hm, res, tmp, start + 1, pwChar);
            tmp.setLength(start);
        }
    }
}

public static void main(String[] args) {
    PassWordGenerate sol = new PassWordGenerate();
    String pw = "password";
    Map<Character, List<Character>> hm = new HashMap<>();
    hm.put('a', new ArrayList<>(Arrays.asList('@')));
    hm.put('o', new ArrayList<>(Arrays.asList('0', '+')));

    System.out.println(sol.generate(hm, pw));
}
```
