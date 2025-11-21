# 165 Compare Version Numbers

Compare two version numbersversion1andversion2.\
Ifversion1>version2return 1, ifversion1\<version2return -1, otherwise return 0.

You may assume that the version strings are non-empty and contain only digits and the`.`character.\
The`.`character does not represent a decimal point and is used to separate number sequences.\
For instance,`2.5`is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

Here is an example of version numbers ordering:

```
0.1 < 1.1 < 1.2 < 13.37
```

虽然这题貌似简单，可以是也有需要注意的地方。首先split函数里，"."是得escape的，不然切不开。另外，输入虽然只包含数字和点，但别天真的以为数字一定合法。测试用例里有01，00这样的输入，所以比较的时候不能直接比较字符串要换成数字再比较。

```java
public int compareVersion(String version1, String version2) {
    if (version1 == null && version2 == null) {
        return 0;
    } else if (version1 == null && version2 != null) {
        return -1;
    } else if (version1 != null && version2 == null) {
        return 1;
    }

    String[] v1 = version1.split("\\.");
    String[] v2 = version2.split("\\.");

    int i = 0;
    int j = 0;
    int len1 = v1.length;
    int len2 = v2.length;

    while (i < len1 || j < len2) {
        String s1 = i < len1 ? v1[i] : "0";
        String s2 = j < len2 ? v2[j] : "0";
        Integer one = Integer.valueOf(s1);
        Integer two = Integer.valueOf(s2);            

        if (one == two) {
            i++;
            j++;
        } else {
            return one.compareTo(two);
        }
    }

    return 0;
}
```
