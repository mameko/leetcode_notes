# 415 Add Strings

Given two non-negative integers`num1`and`num2`represented as string, return the sum of`num1`and`num2`.

**Note:**

1. The length of both`num1`and`num2`is < 5100.
2. Both`num1`and`num2`contains only digits`0-9`.
3. Both`num1`and`num2`does not contain any leading zero.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

```java
public String addStrings(String num1, String num2) {
    if (num1 == null || num2 == null || num1.isEmpty() || num2.isEmpty()) {
        return "0";
    }

    int n1 = num1.length();
    int n2 = num2.length();
    int i = n1 - 1;
    int j = n2 - 1;
    int carry = 0;
    StringBuilder res = new StringBuilder();

    while (i >= 0 || j >= 0) {
        int dig1 = i >= 0 ? Character.getNumericValue(num1.charAt(i)) : 0;
        int dig2 = j >= 0 ? Character.getNumericValue(num2.charAt(j)) : 0;

        int sum = dig1 + dig2 + carry;
        res.append(sum % 10);
        carry = sum / 10;
        i--;
        j--;
    }

    if (carry == 1) {
        res.append(1);
    }

    return res.reverse().toString();
}
```
