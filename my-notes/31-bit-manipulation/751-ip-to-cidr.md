# 751 IP to CIDR

An **IP address** is a formatted 32-bit unsigned integer where each group of 8 bits is printed as a decimal number and the dot character `'.'` splits the groups.

* For example, the binary number `00001111 10001000 11111111 01101011` (spaces added for clarity) formatted as an IP address would be `"15.136.255.107"`.

A **CIDR block** is a format used to denote a specific set of IP addresses. It is a string consisting of a base IP address, followed by a slash, followed by a prefix length `k`. The addresses it covers are all the IPs whose **first `k` bits** are the same as the base IP address.

* For example, `"123.45.67.89/20"` is a CIDR block with a prefix length of `20`. Any IP address whose binary representation matches `01111011 00101101 0100xxxx xxxxxxxx`, where `x` can be either `0` or `1`, is in the set covered by the CIDR block.

You are given a start IP address `ip` and the number of IP addresses we need to cover `n`. Your goal is to use **as few CIDR blocks as possible** to cover all the IP addresses in the **inclusive** range `[ip, ip + n - 1]` **exactly**. No other IP addresses outside of the range should be covered.

Return _the **shortest** list of **CIDR blocks** that covers the range of IP addresses. If there are multiple answers, return **any** of them_.

&#x20;

**Example 1:**

<pre><code><strong>Input: ip = "255.0.0.7", n = 10
</strong><strong>Output: ["255.0.0.7/32","255.0.0.8/29","255.0.0.16/32"]
</strong><strong>Explanation:
</strong>The IP addresses that need to be covered are:
- 255.0.0.7  -> 11111111 00000000 00000000 00000111
- 255.0.0.8  -> 11111111 00000000 00000000 00001000
- 255.0.0.9  -> 11111111 00000000 00000000 00001001
- 255.0.0.10 -> 11111111 00000000 00000000 00001010
- 255.0.0.11 -> 11111111 00000000 00000000 00001011
- 255.0.0.12 -> 11111111 00000000 00000000 00001100
- 255.0.0.13 -> 11111111 00000000 00000000 00001101
- 255.0.0.14 -> 11111111 00000000 00000000 00001110
- 255.0.0.15 -> 11111111 00000000 00000000 00001111
- 255.0.0.16 -> 11111111 00000000 00000000 00010000
The CIDR block "255.0.0.7/32" covers the first address.
The CIDR block "255.0.0.8/29" covers the middle 8 addresses (binary format of 11111111 00000000 00000000 00001xxx).
The CIDR block "255.0.0.16/32" covers the last address.
Note that while the CIDR block "255.0.0.0/28" does cover all the addresses, it also includes addresses outside of the range, so we cannot use it.
</code></pre>

**Example 2:**

<pre><code><strong>Input: ip = "117.145.102.62", n = 8
</strong><strong>Output: ["117.145.102.62/31","117.145.102.64/30","117.145.102.68/31"]
</strong></code></pre>

&#x20;

**Constraints:**

* `7 <= ip.length <= 15`
* `ip` is a valid **IPv4** on the form `"a.b.c.d"` where `a`, `b`, `c`, and `d` are integers in the range `[0, 255]`.
* `1 <= n <= 1000`
* Every implied address `ip + x` (for `x < n`) will be a valid IPv4 address.

Airbnb的题目怎么都这么难，发现这题太久以前就有，已经没有人更新写法了。目前市面上的写法都过不了新的test case ”0.0.0.0“。因为现有的写法都是根据 num & ·num求最后一个1的位置，然后确定这一节能包含多少个ip地址。0.0.0.0根本没有1，所以会死循环。

不过总比没有好，这里先贴上来了。首先，要把这个ip地址变成一个数字，进制转换就ok了。然后呢，我们要用 X & -X来算出最后一个1是在哪里。譬如，如果最后几位是1000，那么我们知道这个000是3个位置，2^3，能代表8个地址。我们一边loop一边把CIDR的地址放到结果里。因为我们要刚刚好，所以如果这个子网太大了，我们要不断除以2来缩小到小于n的范围。最后，把num变回4节的ip地址。每次取8位，然后放到block里。count是用来算子网数字的，每一位就是一个2的次方。譬如，32是1个子网，31是两个子网，所以这里我们不断除以2，看是多少位的子网。

```java
public List<String> ipToCIDR(String ip, int n) {
    if (ip == null || ip.isEmpty() || n < 0) {
        return null;
    }

    long num = convertIpStrToNum(ip.split("\\."));
    List<String> result = new ArrayList<>();
    while (n > 0) {
        // 取最后一个1的位置，last1表示的是，所包含的子网数目
        long last1 = num & -num;
        // 每一段子网都要小于n的范围，所以不能大于n
        while (last1 > n) { 
            last1 = last1 / 2;
        }
        // 找到之后，变成CIDR的样子
        result.add(convertNumToCIDRFormat(num, last1));
        // 我们找完一段，就把num加上已经加了的子网
        num = num + last1;
        // n要减去加了的子网
        n -= last1;
    }

    return result;
}

private String convertNumToCIDRFormat(long num, long last1) {
    int[] block = new int[4];

    block[0] = (int)(num & 255);
    num = num >> 8;
    block[1] = (int)(num & 255);
    num = num >> 8;
    block[2] = (int)(num & 255);
    num = num >> 8;
    block[3] = (int)(num & 255);
    num = num >> 8;

    int count = 0;
    while (last1 > 0) {
        count++;
        last1 = last1 / 2;
    }

    // 记得放过来assemble那个ip
    StringBuilder sb = new StringBuilder();
    sb.append(block[3]);
    sb.append(".");
    sb.append(block[2]);
    sb.append(".");
    sb.append(block[1]);
    sb.append(".");
    sb.append(block[0]);
    sb.append("/");
    sb.append(33 - count);

    return sb.toString();
}


private long convertIpStrToNum(String[] ipSec) {
    long total = 0;
    for (int i = 0; i < ipSec.length; i++) {
        total = total * 256 + Integer.parseInt(ipSec[i]);
    }

    return total;
}
```
