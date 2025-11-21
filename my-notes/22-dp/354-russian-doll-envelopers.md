# 354 Russian Doll Envelopers

You have a number of envelopes with widths and heights given as a pair of integers`(w, h)`. One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? (put one inside other)

**Example**

Given envelopes =`[[5,4],[6,4],[6,7],[2,3]]`,\
the maximum number of envelopes you can Russian doll is 3 (\[2,3] => \[5,4] => \[6,7]).

这题如果把信封按照width排序之后就只要判断height好了。题目就会转变成[L76](397-longest-increasing-continuous-subsequence.md)了。还是用一维dp来做。这里跟L76一样也有一个nlogn的方法。不过排序时得把height也排起来，而且要降序排。因为信封得严格大于时才能套进去，\[3, 3] \[3, 4]是套不进的，所以把\[3, 4]排在前面，避免发生误套的情况。

```java
public int maxEnvelopes(int[][] envelopes) {
    int res = 0;
    if (envelopes == null || envelopes.length == 0 || envelopes[0].length == 0) {
        return res;
    }

    Arrays.sort(envelopes, new Comparator<int[]>(){
        public int compare(int[] e1, int[] e2) {
            if (e1[0] == e2[0]) {
                return e1[1] - e2[1];
            }
            return e1[0] - e2[0];
        }
    });

    int n = envelopes.length;
    int[] dp = new int[n];

    Arrays.fill(dp, 1);

    for (int i = 0; i < n; i++) {
        int[] curEnv = envelopes[i];
        for (int j = 0; j < i; j++) {
            int[] tmpEnv = envelopes[j];
            if (curEnv[0] > tmpEnv[0] && curEnv[1] > tmpEnv[1]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }

    int max = 0;
    for (int i = 0; i < n; i++) {
        max = Math.max(max, dp[i]);
    }

    return max;
}
```

下面是nlogn的方法：

```java
public int maxEnvelopes(int[][] envelopes) {
    int res = 0;
    if (envelopes == null || envelopes.length == 0 || envelopes[0].length == 0) {
        return res;
    }

    Arrays.sort(envelopes, new Comparator<int[]>(){
        public int compare(int[] e1, int[] e2) {
            if (e1[0] == e2[0]) {
                return e2[1] - e1[1];
            }
            return e1[0] - e2[0];
        }
    });

    int n = envelopes.length;
    int[] dp = new int[n];
    int max = 0;
    for (int[] env : envelopes) {
        int index = Arrays.binarySearch(dp, 0, max, env[1]);
        if (index < 0) {
            index = -index - 1;
        }
        dp[index] = env[1];

        if (index == max) {
            max++;
        }
    }

    return max;
}
```
