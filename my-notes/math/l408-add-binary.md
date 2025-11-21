# L408 Add Binary

Given two binary strings, return their sum (also a binary string).

**Example**

a =`11`

b =`1`

Return`100`

```java
public String addBinary(String a, String b) {
    if (a == null || b == null) {
        return null;
    } else if (a.length() == 0 && b.length() == 0) {
        return "";
    } else if (a.length() != 0 && b.length() == 0) {
        return a;
    } else if (a.length() == 0 && b.length() != 0) {
        return b;
    }

    int i = a.length() - 1;
    int j = b.length() - 1;
    int carry = 0;
    StringBuilder res = new StringBuilder();

    while (i >= 0 || j >= 0) {
        int diga = (i >= 0 ? (a.charAt(i) - '0') : 0);
        int digb = (j >= 0 ? (b.charAt(j) - '0') : 0);

        int sum = carry + diga + digb;
        carry = sum / 2;
        sum = sum % 2;
        res.append(sum);
        i--;
        j--;
    }

    if (carry != 0) {
        res.append(carry);
    }

    return res.reverse().toString();
}
```
