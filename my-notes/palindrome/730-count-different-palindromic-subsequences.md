# 730 Count Different Palindromic Subsequences

Given a string S, find the number of different non-empty palindromic subsequences in S, and **return that number modulo**`10^9 + 7`**.**

A subsequence of a string S is obtained by deleting 0 or more characters from S.

A sequence is palindromic if it is equal to the sequence reversed.

Two sequences`A_1, A_2, ...`and`B_1, B_2, ...`are different if there is some`i`for which`A_i != B_i`.

**Example 1:**

```
Input:S = 'bccb'
Output: 6

Explanation: 
The 6 different non-empty palindromic subsequences are 'b', 'c', 'bb', 'cc', 'bcb', 'bccb'.
Note that 'bcb' is counted only once, even though it occurs twice.
```

**Example 2:**

```
Input:S = 'abcdabcdabcdabcdabcdabcdabcdabcddcbadcbadcbadcbadcbadcbadcbadcba'
Output: 104860361

Explanation: 
There are 3104860382 different non-empty palindromic subsequences, which is 104860361 modulo 10^9 + 7.
```

**Note:**

The length of`S`will be in the range`[1, 1000]`.

Each character`S[i]`will be in the set`{'a', 'b', 'c', 'd'}`.

这题真的炒鸡难，因为要去重，去重要分多种情况。这题写递推的话下标有点难控制，所以选择记记忆化搜索了。基本还是分扫到的字母相同与否，如果相同分3种情况；如果不同就1种情况。

```
相同：（中间的每一个都能在外边套一个bXXXb，本来的数目+加了套的数目，所以X2）
count("bccb") => count("cc") X 2 + 2 ；最后加上外层的b，bb
count("bcbcb") => count("cbc") X 2 + 1；中间会多加了一个b，所以只+1
count("bbccbb") => count("bccb") X 2 - count("cc")；这里因为“bccb”中已经把bccb这样的算了两边，所以要减去中间那些b跟cc组成的
不同：不同的情况下，要不取左边，要不取右边，所以加起来，但加起来以后中间部分会被重复计算，所以要减掉
count("abbc") => count("abb") + count("bbc") - count("bb")
```

```java
int k = 1000000000 + 7;
public int countPalindromicSubsequences(String S) {
    if (S == null || S.length() == 0) {
        return 0;
    }

    int n = S.length();
    int[][] memo = new int[n][n];        

    return count(S, 0, n - 1, memo);
}

private int count(String S, int s, int e, int[][] memo) {                
    if (s > e) {         
        return 0;
    }

    if (s == e) {
        memo[s][e] = 1;
        return 1;
    }

    if (memo[s][e] > 0) {
        return memo[s][e];
    }

    long res = 0;
    if (S.charAt(s) == S.charAt(e)) {
        res = 2 * count(S, s + 1, e - 1, memo);
        int l = s + 1;
        int r = e - 1;
        while (l <= r && S.charAt(l) != S.charAt(s)) {
            l++;
        }

        while (l <= r && S.charAt(r) != S.charAt(s)) {
            r--;
        }

        if (l > r) {
            res = res + 2;
        } else if (l == r) {
            res = res + 1;
        } else if (l < r) {
            res = res - count(S, l + 1, r - 1, memo);
        }


    } else {
        res = count(S, s + 1, e, memo) + count(S, s, e - 1, memo) - count(S, s + 1, e - 1, memo);
    }
    res = (res + k) % k;

    memo[s][e] = (int)res;
    return memo[s][e];
}
```

贴一个递推作为参考：原code出自花花酱

```cpp
// Author: Huahua
// Runtime: 79 ms
class Solution {
public:
    int countPalindromicSubsequences(const string& S) {
        int n = S.length();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = 0; i < n; ++i)
            dp[i][i] = 1;

        for (int len = 1; len <= n; ++len) {
            for (int i = 0; i < n - len; ++i) {
                const int j = i + len;                
                if (S[i] == S[j]) {
                    dp[i][j] = dp[i + 1][j - 1] * 2;                        
                    int l = i + 1;
                    int r = j - 1;
                    while (l <= r && S[l] != S[i]) ++l;
                    while (l <= r && S[r] != S[i]) --r;                    
                    if (l == r) dp[i][j] += 1;
                    else if (l > r) dp[i][j] += 2;
                    else dp[i][j] -= dp[l + 1][r - 1];
                } else {
                    dp[i][j] = dp[i][j - 1] + dp[i + 1][j] - dp[i + 1][j - 1]; 
                }

                dp[i][j] = (dp[i][j] + kMod) % kMod;
            }
        }

        return dp[0][n - 1];
    }
private:
    static constexpr long kMod = 1000000007;    
};
```
