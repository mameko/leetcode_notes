# L79 Longest Common SubString

Given two strings, find the longest common substring.

Return the length of it.

## Notice

The characters in **substring** should occur continuously in original string. This is different with **subsequence**.

**Example**

Given A =`"ABCD"`, B =`"CBCE"`, return`2`.

[**Challenge**](http://www.lintcode.com/en/problem/longest-common-substring/#challenge)

O(n x m) time and memory.

跟上一题L77的做法基本一样，只是这里要求substring，连续的，当我们发现两个串现在的char不一样时，我们把dp设为0.而且还得用一个max边找边记录。因为结果不一定在最后。

```java
public int longestCommonSubstring(String A, String B) {

    if(A == null || B == null){
        return 0;
    }

    if(A.length() == 0 || B.length() == 0){
        return 0;
    }

    int[][] dp = new int[A.length()+1][B.length()+1];
    int max = 0;

    for(int i = 1; i <= A.length(); i++){
        for (int j = 1; j<= B.length(); j++){
            if(A.charAt(i-1) == B.charAt(j-1)){
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                dp[i][j] = 0;
            }
            max = Math.max(max, dp[i][j]);
        }
    }

    return max;
}
```
