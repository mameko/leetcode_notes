# 282 Expression Add Operators

Given a string that contains only digits`0-9`and a target value, return all possibilities to add**binary**operators (not unary)`+`,`-`, or`*`between the digits so they evaluate to the target value.

Examples:

```
"123", 6 -> ["1+2+3", "1*2*3"] 
"232", 8 -> ["2*3+2", "2+3*2"]
"105", 5 -> ["1*0+5","10-5"]
"00", 0 -> ["0+0", "0-0", "0*0"]
"3456237490", 9191 -> []
```

这题又得看答案，一时想不出怎么做，自己想着先permute那个符号然后再算表达式的值。其实这题和那些combination sum很像，可以边permute边把算到现在的值带上。这题要注意的是，因为要算乘法，所以要把前一个数也带上，然后碰到×号的时候，要先减去再加上，例子：2 + 3 × 2，这里，传进去的时候是sumSofar = 5, pre = 3, 然后要× 2，正确的表达式是（5 - 3）+ 3 \* 2.另外还得注意跳过00-0的情况。还有，要用long因为数字太大会越界。一开始还以为每次只取一个数字，原来可以多个取。例如，123，可以解释成：1， 2， 3或者12，3之类的。

```java
public List<String> addOperators(String num, int target) {
    List<String> res = new ArrayList<>();
    if (num == null || num.isEmpty()) {
        return res;
    }

    dfsHelper(num, 0, "", 0, 0, target, res);

    return res;
}

private void dfsHelper(String num, int start, String strSofar, long sumSofar, long pre, int target, List<String> res) {
    if (start == num.length()) {
        if (sumSofar == target) {
            res.add(strSofar);
        }
        return;
    }

    for (int i = start; i < num.length(); i++) {
        String sub = num.substring(start, i + 1);
        if (i > start && sub.charAt(0) == '0') {// skip 00 + 0 etc
            break;
        }

        long now = Long.valueOf(sub);

        if (strSofar.isEmpty()) {// first one
            dfsHelper(num, i + 1, String.valueOf(now), now, now, target, res);        
        } else {
            dfsHelper(num, i + 1, strSofar + "+" + now, sumSofar + now, now, target, res);
            dfsHelper(num, i + 1, strSofar + "-" + now, sumSofar - now, -now, target, res);
            dfsHelper(num, i + 1, strSofar + "*" + now, sumSofar - pre + pre * now, pre * now, target, res);
        }
    }
}
```
