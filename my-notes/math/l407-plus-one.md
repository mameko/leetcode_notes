# L407 Plus One

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

**Example**

Given`[1,2,3]`which represents 123, return`[1,2,4]`.

Given`[9,9,9]`which represents 999, return`[1,0,0,0]`.

这题下标真特么的难控制。

两年后再写的版本：

```java
public int[] plusOne(int[] digits) {        
    if (digits == null) {
        return new int[0];
    }

    int carry = 0;        
    int sum = digits[digits.length - 1] + 1;
    for (int i = digits.length - 1; i >= 0; i--) {                
        digits[i] = sum % 10;
        carry = sum / 10;
        int last = i == 0 ? 0 : digits[i - 1];
        sum = carry + last;
    }

    if (carry != 0) {
        int[] res;
        int ind = 0;
        res = new int[digits.length + 1];
        res[0] = carry;
        ind++;
        for (int dig : digits) {
            res[ind++] = dig;
        }                  
        return res;
    } else {
        return digits;
    }        

}
```

```java
public int[] plusOne(int[] digits) {
    if (digits == null || digits.length == 0) {
        return null;
    }

    int carry = 0;
    int sum = digits[digits.length - 1] + 1 + carry;
    carry = sum / 10;
    digits[digits.length - 1] = sum % 10;

    for (int i = digits.length - 2; i >= 0 && carry == 1; i--) {
        int curSum = digits[i] + carry;
        carry = curSum / 10;
        digits[i] = curSum % 10;
    }

    if (carry == 1) {
        int[] res = Arrays.copyOf(digits, digits.length + 1);
        for (int i = digits.length; i > 0; i--) {
            res[i] = res[i - 1];
        }
        res[0] = 1;
        return res;
    } else {
        return digits;
    }
}
```
