# 1304 Find N Unique Integers Sum up to Zero

Given an integer `n`, return **any** array containing `n` **unique** integers such that they add up to `0`.

**Example 1:**

<pre><code>Input: n = 5
<strong>Output: [-7,-1,1,3,4]
</strong><strong>Explanation: These arrays also are accepted [-5,-1,1,2,3] , [-3,-1,2,-2,4].
</strong></code></pre>

**Example 2:**

<pre><code>Input: n = 3
<strong>Output: [-1,0,1]
</strong></code></pre>

**Example 3:**

<pre><code>Input: n = 1
<strong>Output: [0]
</strong></code></pre>

&#x20;**Constraints:**

* `1 <= n <= 1000`

继续盲目找自信...这题不太难，嘛，简单题嘛。主要是loop结束条件要保证不会out of bound。



```java
public int[] sumZero(int n) {
    if (n < 1) {
        return null;
    }
    
    int[] result = new int[n];
    int pos = 1;
    int neg = -1;
    for (int i = 0; i + 1 < n; i = i + 2, pos++, neg--) {
        result[i] = pos;
        result[i + 1] = neg;
    }
    
    if (n % 2 != 0) {
        result[n - 1] = 0;
    }
}

java}    return resul
```
