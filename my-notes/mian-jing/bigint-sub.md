# BigInt sub

```java
public String subtract(String num1, String num2) {
    boolean positive = true;
    if (compare(num1, num2) == 0) {
        return "0";
    } else if (compare(num1, num2) < 0) {
        String tmp = num1;
        num1 = num2;
        num2 = tmp;
        positive = false;
    }

    int n1 = num1.length();
    int n2 = num2.length();
    int i = n1 - 1;
    int j = n2 - 1;
    int borrow = 0;
    StringBuilder sb = new StringBuilder();

    while (i >= 0 || j >= 0) {
        int dig1 = i >= 0 ? Character.getNumericValue(num1.charAt(i)) : 0;
        int dig2 = j >= 0 ? Character.getNumericValue(num2.charAt(j)) : 0;

        int diff = dig1 - dig2 - borrow;
        if (diff < 0) {
            diff += 10;
            borrow = 1;
        }
        sb.append(diff % 10);

        i--;
        j--;
    }    

    // remove preceding 0s
    int start = sb.length() - 1;
    while(sb.charAt(start) == '0') {
        sb.deleteCharAt(start--);
    }        

    sb.reverse();
    return positive ? sb.toString() : "-" + sb.toString();

}

private int compare(String num1, String num2) {
    int n1 = num1.length();
    int n2 = num2.length();

    if (n1 > n2) {
        return 1;
    } else if (n1 < n2) {
        return -1;
    } else {
        for (int i = 0; i < n1; i++) {
            if (num1.charAt(i) > num2.charAt(i)) {
                return 1;
            } else if (num1.charAt(i) < num2.charAt(i)) {
                return -1;
            }
        }
    }
    return 0;
}
```
