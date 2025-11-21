# 273 Integer to English Words

Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 231- 1.

For example,

```
123 -> "One Hundred Twenty Three"
12345 -> "Twelve Thousand Three Hundred Forty Five"
1234567 -> "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```

这题是很久以前在写cc189的时候就写了的，代码有点长。有处理负数的情况。然后再discuss里看到一种比较简洁的写法，没有处理负数的情况。贴上来作为参考。因为英文数字都是3位3位为一个单位重复，所以用helper函数生成这部分。每隔3位call一次

```java
// 3刷，改了一下用StringBuilder
// 这里注意要把“Ten”也放进来，不然一mod位置会不对，第一个全用“ ”，其他的全部后边加个空格方便打印
String[] LESS_THAN_20 = {"", "One ", "Two ", "Three ", "Four ", "Five ", "Six ", "Seven ", "Eight ", "Nine ", "Ten ", "Eleven ", "Twelve ", "Thirteen ", "Fourteen ", "Fifteen ", "Sixteen ", "Seventeen ", "Eighteen ", "Nineteen "};
String[] TENS = {"", "Ten ", "Twenty ", "Thirty ", "Forty ", "Fifty ", "Sixty ", "Seventy ", "Eighty ", "Ninety "};
String[] THOUSANDS = {"", "Thousand ", "Million ", "Billion "};

public String numberToWords(int num) {   
    // 特判一下0
    if (num == 0) {
        return "Zero";
    }

    StringBuilder res = new StringBuilder();
    int i = 0; // Thousands数组的下标，每次除1000
    while (num > 0) {
        if (num % 1000 != 0) { // 这里特判是for “One Million”这种情况，没有这个的话，后面会多加了Thousand
            res.insert(0, THOUSANDS[i]);        // 每次从头插进去            
            res.insert(0, helper(num % 1000));  // 然后调用helper函数生成3位
        }
        num = num / 1000;
        i++;
    }        

    return res.toString().trim(); // 怕最后会有多余的空格，所以trim（）一下
}

private String helper(int num) { // 生成3位数
    if (num < 20) {
        return LESS_THAN_20[num];
    } else if (num < 100) {
        return TENS[num / 10] + helper(num % 10);
    } else {
        return LESS_THAN_20[num / 100] + "Hundred " + helper(num % 100);
    }
}
```

```java
private final String[] LESS_THAN_20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
private final String[] TENS = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
private final String[] THOUSANDS = {"", "Thousand", "Million", "Billion"};

public String numberToWords(int num) {
    if (num == 0) return "Zero";

    int i = 0;
    String words = "";

    while (num > 0) {
        if (num % 1000 != 0)
            words = helper(num % 1000) +THOUSANDS[i] + " " + words;
        num /= 1000;
        i++;
    }

    return words.trim();
}

private String helper(int num) {
    if (num == 0)
        return "";
    else if (num < 20)
        return LESS_THAN_20[num] + " ";
    else if (num < 100)
        return TENS[num / 10] + " " + helper(num % 10);
    else
        return LESS_THAN_20[num / 100] + " Hundred " + helper(num % 100);
}
```

```java
public String numberToWords(int n) {
        init();
    StringBuilder res = new StringBuilder();

    if (n == 0) {
        return res.append(map.get(0)).deleteCharAt(res.length() - 1).toString();
    }

    int minus = 1;
    if (n < 0) {
        res.append(map.get(-1));
        minus = minus * -1;
    }

    int mill = 1000000000 * minus;
    int bill = 1000000 * minus;
    int thou = 1000 * minus;

    if (n / mill > 0) {
        int num = n / mill;
        n = n % mill;

        res.append(map.get(num));
        res.append(map.get(minus * mill));
    }

    if (n / bill > 0) {
        int num = n / bill;
        n = n % bill;

        String s = getString(num);
        res.append(s);
        res.append(map.get(minus * bill));
    }

    if (n / thou > 0) {
        int num = n / thou;
        n = n % thou;

        String s = getString(num);
        res.append(s);
        res.append(map.get(minus * thou));
    }

    if (n / minus > 0) {
        String s = getString(n / minus);
        res.append(s);
    }


    return res.deleteCharAt(res.length() - 1).toString();
}

private String getString(int num) {
    StringBuilder sb = new StringBuilder();

    int n1 = num / 100;
    if (n1 > 0) {
        sb.append(map.get(n1));
        sb.append(map.get(100));
//             if (num % 100 != 0) {
//                 sb.append("and ");
//             }
    }
    num = num % 100;

    if (num < 20 && num > 0) {
        sb.append(map.get(num));
    } else if (num >= 20 && num < 100) {

        int n2 = num / 10;
        num = num % 10;
        sb.append(map.get(n2 * 10));
        if (num != 0) {
            sb.append(map.get(num));
        }
    }

    return sb.toString();
}

HashMap<Integer, String> map = new HashMap<>();

private void init() {
    map.put(0, "Zero ");
    map.put(1, "One ");
    map.put(2, "Two ");
    map.put(3, "Three ");
    map.put(4, "Four ");
    map.put(5, "Five ");
    map.put(6, "Six ");
    map.put(7, "Seven ");
    map.put(8, "Eight ");
    map.put(9, "Nine ");
    map.put(10, "Ten ");
    map.put(11, "Eleven ");
    map.put(12, "Twelve ");
    map.put(13, "Thirteen ");
    map.put(14, "Fourteen ");
    map.put(15, "Fifteen ");
    map.put(16, "Sixteen ");
    map.put(17, "Seventeen ");
    map.put(18, "Eighteen ");
    map.put(19, "Nineteen ");
    map.put(20, "Twenty ");
    map.put(30, "Thirty ");
    map.put(40, "Forty ");
    map.put(50, "Fifty ");
    map.put(60, "Sixty ");
    map.put(70, "Seventy ");
    map.put(80, "Eighty ");
    map.put(90, "Ninety ");
    map.put(100, "Hundred ");
    map.put(1000, "Thousand ");
    map.put(1000000, "Million ");
    map.put(1000000000, "Billion ");
    map.put(-1, "Minus ");
}
```
