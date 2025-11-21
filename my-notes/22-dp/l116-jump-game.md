# L116 Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

## Notice

This problem have two method which is`Greedy`and`Dynamic Programming`.

The time complexity of`Greedy`method is`O(n)`.

The time complexity of`Dynamic`Programming method is`O(n^2)`.

We manually set the small data set to allow you pass the test in both ways. This is just to let you learn how to use this problem in dynamic programming ways. If you finish it in dynamic programming ways, you can try greedy method to make it accept again.

**Example**

A =`[2,3,1,1,4]`, return`true`.

A =`[3,2,1,0,4]`, return`false`.

其实感觉这里的dp有点像LIS，work break也是，palindrome min cut也是。把距离设成是MAX表示一开始不可达。然后每次遍历i前面所有的j，如果有能够达到这个i而且更少的步数，更新现在的步数。其实可以用一个boolean的dp，只要找到一个能跳的就ok了，这里用了jump game2的dp。最后返回值就看能否达到最后一格。

```java
public boolean canJump(int[] A) {
    if (A == null || A.length == 0) {
        return false;
    }

    int[] dp = new int[A.length];
    for (int i = 0; i < A.length; i++) {
        dp[i] = Integer.MAX_VALUE;
    }

    dp[0] = 0;
    for (int i = 1; i < A.length; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] != Integer.MAX_VALUE && (A[j] + j) >= i) {
                dp[i] = Math.min(dp[j] + 1, dp[i]);
            }
        }

    }
    return dp[A.length - 1] != Integer.MAX_VALUE;
}
```

boolean DP：

```java
public boolean canJump(int[] nums) {
    if (nums == null || nums.length == 0) {
        return false;
    }

    int n = nums.length;
    boolean[] dp = new boolean[n];
    dp[0] = true;
    for (int i = 1; i < n; i++) {
        for (int j = i - 1; j >= 0; j--) {
            if (dp[j] && nums[j] + j >= i) {
                dp[i] = true;
                break;
            }
        }
    }

    return dp[n - 1];
}
```

这里还能用另外一种O(n)的dp，把能跳的步数放进dp里，然后一边往后扫一边减，看会不会出现负数。如果出现了，证明挑不到最后。

```java
public boolean canJump(int[] A) {
    if (A == null || A.length == 0) {
        return false;
    }

    int[] dp = new int[A.length];
    dp[0] = A[0];
    for (int i = 1; i < A.length; i++) {
        dp[i] = Math.max(A[i - 1], dp[i - 1]) - 1;
        if (dp[i] < 0) {
            return false;
        }
    }

    return dp[A.length - 1] >= 0;
}
```

最后再来一个贪心的O(n)：

```java
public boolean canJump(int[] A) {        
    if(A==null||A.length==0){
        return false;
    }

    int cur=0;
    while(cur<A.length ){
      if((cur+A[cur]) >= A.length-1){
            return true;
        }else{
            int j=1;
            int i=A[cur];
            int max = 0;
            int maxLoc = j;
            while(j<=i){
                int tmp = A[cur+j]+cur+j;
                if(tmp >= max){
                    max = tmp;
                    maxLoc = j;
                }
                j++;
            }
            if (max==0) {
                return false;
            }
            cur=cur+maxLoc;
        }
    }

    return false;
}
```
