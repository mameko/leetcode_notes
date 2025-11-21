# csv parser

给一个string，有换行和双引号，然后被逗号隔开。要求返回list of list。返回要求是如果\n在双引号里的时候不算换行，把string按行切开返回。例如：input ：［a,b\nc,d,"e,f\ng"\n,h,i］output: \[\[a, b], \[c,d,"e,f\ng"],\[h,i]]

感觉跟228 Summary Range，163 Missing Range和168 Reverse words in string II差不多，都是得用两个指针循环着找。这里因为要escape麻烦，所以我把\n改成-n，把双引号改成单引号了。

```java
public List<String> parse(String str) {
    List<String> res = new ArrayList<>();
    if (str == null || str.isEmpty()) {
        return res;
    }

    int s = 0;
    int len = str.length();
    boolean quoteStarted = false;
    int e = s;
    while (s < len && e < len) {
        char cur = str.charAt(e);
        if (cur == '\'') {
            quoteStarted = quoteStarted ^ true;
        } else if (cur == '-' && (e + 1 < len) && str.charAt(e + 1) == 'n') {
            if (quoteStarted) {
                e++;
                continue;
            } else {
                res.add(str.substring(s, e));
                s = e + 2;
                e++;
            }
        }
        e++;
    }

    if (e > s) {
        res.add(str.substring(s, len));
    }

    return res;
}

public static void main(String[] args) {
    CSVParser sol = new CSVParser();
    String str1 = "a,b-nc,d,'e,f-ng'-nh,i";
    String str2 = " ";
    String str3 = "ab-ncd";
    String str4 = "'ab-n'";
    String str5 = "'ab-nc'd";
    // for (String string : sol.parse(str1)) {
    // for (String string : sol.parse(str2)) {
    // for (String string : sol.parse(str3)) {
    // for (String string : sol.parse(str4)) {
    for (String string : sol.parse(str5)) {
        System.out.println(string);
    }
}
```
