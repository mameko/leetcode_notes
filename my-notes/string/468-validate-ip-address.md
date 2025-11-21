# 468 Validate IP Address



Given a string `IP`, return `"IPv4"` if IP is a valid IPv4 address, `"IPv6"` if IP is a valid IPv6 address or `"Neither"` if IP is not a correct IP of any type.

**A valid IPv4** address is an IP in the form `"x1.x2.x3.x4"` where `0 <= xi <= 255` and `xi` **cannot contain** leading zeros. For example, `"192.168.1.1"` and `"192.168.1.0"` are valid IPv4 addresses but `"192.168.01.1"`, while `"192.168.1.00"` and `"192.168@1.1"` are invalid IPv4 addresses.

**A valid IPv6** address is an IP in the form `"x1:x2:x3:x4:x5:x6:x7:x8"` where:

* `1 <= xi.length <= 4`
* `xi` is a **hexadecimal string** which may contain digits, lower-case English letter (`'a'` to `'f'`) and upper-case English letters (`'A'` to `'F'`).
* Leading zeros are allowed in `xi`.

For example, "`2001:0db8:85a3:0000:0000:8a2e:0370:7334"` and "`2001:db8:85a3:0:0:8A2E:0370:7334"` are valid IPv6 addresses, while "`2001:0db8:85a3::8A2E:037j:7334"` and "`02001:0db8:85a3:0000:0000:8a2e:0370:7334"` are invalid IPv6 addresses.

**Example 1:**

```
Input: IP = "172.16.254.1"
Output: "IPv4"
Explanation: This is a valid IPv4 address, return "IPv4".
```

**Example 2:**

```
Input: IP = "2001:0db8:85a3:0:0:8A2E:0370:7334"
Output: "IPv6"
Explanation: This is a valid IPv6 address, return "IPv6".
```

**Example 3:**

```
Input: IP = "256.256.256.256"
Output: "Neither"
Explanation: This is neither a IPv4 address nor a IPv6 address.
```

**Example 4:**

```
Input: IP = "2001:0db8:85a3:0:0:8A2E:0370:7334:"
Output: "Neither"
```

**Example 5:**

```
Input: IP = "1e1.4.5.6"
Output: "Neither"
```

**Constraints:**

* `IP` consists only of English letters, digits and the characters `'.'` and `':'`.

这题其实算法上不难，难度在于各种边界条件的判断。就算题目给了条件，也很难写对。嘛，除掉regex解法不说，很直接的就是一段一段判断。

第一个陷阱，split(".")要escape。

第二个陷阱，原来split要用第二个数来指定长度，如果不指定的话，trailing空串会被吃掉，但前面的不会，wtf? 所以“x.x.x.x."会出问题。

第三个陷阱，判断leading zero的时候，记得'0'是合法的，但‘01'是不合法的。

第四个陷阱，char数字转换要用Character.getNumericValue()，如果用Integer.valueOf()你会得到ASCII码。

第五个陷阱，如果每一节是空的，是不valid的，需要加判断。“2:::::::2"

不难，但不能bug free的题

```java
public String validIPAddress(String IP) {
    if (IP == null || IP.isEmpty()) {
        return "Neither";
    }

    String[] ipv4Seg = IP.split("\\.", 5);
    String[] ipv6Seg = IP.split(":", 9);

    if (ipv4Seg.length == 4) {
        return validateV4(ipv4Seg);
    } else if (ipv6Seg.length == 8) {
        return validateV6(ipv6Seg);
    } else {
        return "Neither";
    }
}

private String validateV4(String[] ipv4Seg) {
    for (String seg : ipv4Seg) {
        if (seg.isEmpty()) {
            return "Neither";
        }
        char[] chars = seg.toCharArray();
        if (chars[0] == '0' && chars.length > 1) {// leading zero is not allow
            return "Neither";
        }

        int value = 0;
        for (char c : chars) {
            if (!Character.isDigit(c)) { // non digit is not allow
                return "Neither";
            }
            value = value * 10 + Character.getNumericValue(c);
        }

        if (value < 0 || value > 255) { // value has to be 1 to 255
             return "Neither";
        }
    }

    return "IPv4";
}

private String validateV6(String[] ipv6Seg) {        
    Set<Character> validV6Parts = new HashSet<>(Arrays.asList('a', 'b', 'c', 'd', 'e', 'f', 'A', 'B', 'C', 'D','E','F','0', '1', '2', '3', '4', '5', '6', '7', '8', '9'));        
    for (String seg : ipv6Seg) {
        if (seg.isEmpty()) {
            return "Neither";
        }
        char[] chars = seg.toCharArray();
        if (chars.length > 4 || chars.length < 1) { // has to have 1 to 4 
             return "Neither";
        }

        for (char c : chars) {
            if (!validV6Parts.contains(c)) {
                return "Neither";
            }
        }            
    }
    if (ipv6Seg[0].charAt(0) == '0') {
        return "Neither";
    }

    return "IPv6";
}
```
