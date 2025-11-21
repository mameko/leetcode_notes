# 1641Count Sorted Vowel Strings

Given an integer `n`, return _the number of strings of length_ `n` _that consist only of vowels (_`a`_,_ `e`_,_ `i`_,_ `o`_,_ `u`_) and are **lexicographically sorted**._

A string `s` is **lexicographically sorted** if for all valid `i`, `s[i]` is the same as or comes before `s[i+1]` in the alphabet.

**Example 1:**

```
Input: n = 1
Output: 5
Explanation: The 5 sorted strings that consist of vowels only are ["a","e","i","o","u"].
```

**Example 2:**

```
Input: n = 2
Output: 15
Explanation: The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.
```

**Example 3:**

```
Input: n = 33
Output: 66045
```

**Constraints:**

* `1 <= n <= 50`&#x20;

这题一看，第一反应是枚举，所以很快就写了brute force的方案。然后看了答案，发现这题水很深。而且自己的实现不够高效。其实这题有很多解法，因为求方案数，所以其实是可以dp的。

解法一：递归brute force，T:O(n5)因为只有5个元音字母。S:O(n)递归深度等于要找string的长度

```java
// 我的版本
int count;
public int countVowelStrings(int n) {
    if (n < 1) {
        return 0;
    }

    count = 0;
    char[] vowels = {'a', 'e', 'i', 'o', 'u'};
    recurHelper(n, 0, vowels, 0);
    return count;
}

private void recurHelper(int n, int start, char[] vowels, int charLoc) {
    if (start == n) {
        count++;
        return;
    }

    for (int i = charLoc; i < vowels.length; i++) {
        recurHelper(n, start + 1, vowels, i);
    }
}

// Solution的版本
   public int countVowelStrings(int n) {
        return countVowelStringUtil(n, 1);
    }

    int countVowelStringUtil(int n, int vowels) {
        if (n == 0)
            return 1;
        int result = 0;
        for (int i = vowels; i <= 5; i++) {
            result += countVowelStringUtil(n - 1, i);
        }
        return result;
    }
```

解法二：利用递推关系来recur，虽然空间和时间复杂度没有改变，但可以引导出下面memorization的解法。这里的递推关系是，countValueString(n, vowels) = countValueString(n - 1, vowels)  + countValueString(n, vowels - 1) 。

![](<../../.gitbook/assets/image (7).png>)

base case有，n = 1，返回元音字母数；vowels = 1，返回1种做法

```java
public int countVowelStrings(int n) {
    return countVowelStringUtil(n, 5);
}

int countVowelStringUtil(int n, int vowels) {
    if (n == 1)
        return vowels;
    if (vowels == 1)
        return 1;
    return countVowelStringUtil(n - 1, vowels) +
            countVowelStringUtil(n, vowels - 1);
}
```

解法三：加上memorization。把递归树上重复的计算用memo记录。T:O(N), 每个组合我们只计算一次，然后递归树节点数是n\*5；S:O(N)，递归深度是n，memo大小是5n，所以总体复杂度是O(n)

![](<../../.gitbook/assets/image (22).png>)

因为有两个变量，n和vowel数目，所以memo是一个二维数组

```java
public int countVowelStrings(int n) {
    int memo[][] = new int[n + 1][6];
    return countVowelStringUtil(n, 5, memo);
}

int countVowelStringUtil(int n, int vowels, int memo[][]) {
    if (n == 1)
        return vowels;
    if (vowels == 1)
        return 1;
    if (memo[n][vowels] != 0)
        return memo[n][vowels];
    int res = countVowelStringUtil(n - 1, vowels, memo) +
            countVowelStringUtil(n, vowels - 1, memo);
    memo[n][vowels] = res;
    return res;
}
```

解法四：把memorization变成递推版本。递推公式是

```
dp[n][vowels] = dp[n - 1][vowels] + dp[n][vowels - 1]
```

我们建立一个n\*vowel的dp数组，下标加1，容易点理解。然后，初始化所有n == 1的情况为vowels数目，vowels == 1的情况数目为1.中间就递推式搞定。复杂度跟上面memorization一样

```java
public int countVowelStrings(int n) {
    int[][] dp = new int[n + 1][6];
    for (int vowels = 1; vowels <= 5; vowels++)
        dp[1][vowels] = vowels;
    for (int nValue = 2; nValue <= n; nValue++) {
        dp[nValue][1] = 1;
        for (int vowels = 2; vowels <= 5; vowels++) {
            dp[nValue][vowels] = dp[nValue][vowels - 1] + dp[nValue - 1][vowels];
        }
    }
    return dp[n][5];
}
```

解法五：数学解，嘛，具体得看solution，反正数学太差没懂。O(1)的解法

```java
public int countVowelStrings(int n) {
    return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
}
```

