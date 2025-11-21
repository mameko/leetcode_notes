# 1461 Check If a String Contains All Binary Codes of Size K

Given a binary string `s` and an integer `k`.

Return _True_ if every binary code of length `k` is a substring of `s`. Otherwise, return _False_.

**Example 1:**

```
Input: s = "00110110", k = 2
Output: true
Explanation: The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indicies 0, 1, 3 and 2 respectively.
```

**Example 2:**

```
Input: s = "00110", k = 2
Output: true
```

**Example 3:**

```
Input: s = "0110", k = 1
Output: true
Explanation: The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring. 
```

**Example 4:**

```
Input: s = "0110", k = 2
Output: false
Explanation: The binary code "00" is of length 2 and doesn't exist in the array.
```

**Example 5:**

```
Input: s = "0000000001011100", k = 4
Output: false
```

其实这一题觉得主要考的是trade off。因为生成substring的时间和空间复杂度是O(nk)，生成二进制，用rolling hash的话，可以T:O(n)，但S是O(2^k)。hash的解法也写下来参考。

rollinghash 思路：譬如我们k=3，s=“11010110”，那么第一个hash是110，然后第二个是101。那么怎么快速从110的hash算出101呢？首先我们吧110左移1位变成1100，然后&上111（k的长度个的1），100。然后加上新进来的1，101。所以公式是：ne&#x77;_&#x68;ash = ((ol&#x64;_&#x68;ash << 1) & allone) | las&#x74;_&#x64;igit_ of new hash

```java
public boolean hasAllCodes(String s, int k) {
    if (s == null || s.length() < 1 || k < 1) {
        return false;
    }

    int numOfBinShouldBe = 1 << k;        
    Set<String> subStrings = new HashSet<>();
    for (int i = k; i <= s.length(); i++) {
        String subStr = s.substring(i - k, i);
        if (!subStrings.contains(subStr)) {
            subStrings.add(subStr);
            numOfBinShouldBe--;
            if (numOfBinShouldBe == 0) {
                return true;
            }
        }            
    }

    return false;
}

// 解法二：hash
public static boolean hasAllCodes(String s, int k) {
    int need = 1 << k;
    boolean[] got = new boolean[need];
    int allOne = need - 1;
    int hashVal = 0;

    for (int i = 0; i < s.length(); i++) {
        // calculate hash for s.substr(i-k+1,i+1)
        hashVal = ((hashVal << 1) & allOne) | (s.charAt(i) - '0');
        // hash only available when i-k+1 > 0
        if (i >= k - 1 && !got[hashVal]) {
            got[hashVal] = true;
            need--;
            if (need == 0) {
                return true;
            }
        }
    }
    return false;
}
```

