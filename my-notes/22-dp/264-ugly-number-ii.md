# 264 Ugly Number II

Write a program to find the`n`-th ugly number.

Ugly numbers are positive numbers whose prime factors only include`2, 3, 5`. For example,`1, 2, 3, 4, 5, 6, 8, 9, 10, 12`is the sequence of the first`10`ugly numbers.

Note that`1`is typically treated as an ugly number, andn**does not exceed 1690**.

**Hint:**

1. The naive approach is to call`isUgly`for every number until you reach the nth one. Most numbers are not ugly. Try to focus your effort on generating only the ugly ones.
2. An ugly number must be multiplied by either 2, 3, or 5 from a smaller ugly number.
3. The key is how to maintain the order of the ugly numbers. Try a similar approach of merging from three sorted lists: L1, L2, and L3.
4. Assume you have Uk, the kth ugly number. Then Uk+1must be Min(L1\* 2, L2\* 3, L3\* 5).

这题有两个解法，一个是网上抄的，用一个array来存所有的ugly数字。我们用3个不同的index来指出现在2，3和5的ugly数字到哪里了。首先我们把res\[0]设为1，第一个udly number是1.然后，我们把3个下标都设成从0开始。每次我们算3个数，然后找最小。每次把生成的这3个数里最小的那个放到第i位ugly number的地方。最后返回res\[i - 1]。

```java
 public int nthUglyNumber(int n) {
    if (n < 1) {
        return 0;
    }

    int[] res = new int[n];
    res[0] = 1;
    int i = 1;
    int i2 = 0;
    int i3 = 0;
    int i5 = 0;
    while (i < n) {
        int min2 = res[i2] * 2;
        int min3 = res[i3] * 3;
        int min5 = res[i5] * 5;
        int min = Math.min(Math.min(min2, min3), min5);
        res[i] = min;
        if (min == min2) {
            i2++;
        } 
        if (min == min3) {
            i3++;
        } 
        if (min == min5) {
            i5++;
        }            
        i++;
    }

    return res[n - 1];
}
```

另一种做法是cc189里的第17.9.用的是队列，每次也是把最小的乘积找出来，放到适当的队伍里。这里要注意，如果用int的话，在i = 1545的时候会overflow，所以要改用long。

```java
// when i = 1545, int val overflow, so need to use long
public int nthUglyNumber(int n) {
    if (n < 0) {
        return 0;
    }

    long val = 0;
    Queue<Long> q2 = new LinkedList<>();
    Queue<Long> q3 = new LinkedList<>();
    Queue<Long> q5 = new LinkedList<>();
    q2.offer(1L);

    for (int i = 0; i < n; i++) {
        long v2 = q2.size() > 0 ? q2.peek() : Long.MAX_VALUE;
        long v3 = q3.size() > 0 ? q3.peek() : Long.MAX_VALUE;
        long v5 = q5.size() > 0 ? q5.peek() : Long.MAX_VALUE;
        val = Math.min(v2, Math.min(v3, v5));
        if (val == v2) {
            q2.poll();
            q2.offer(2 * val);
            q3.offer(3 * val);
            q5.offer(5 * val);
        } else if (val == v3) {
            q3.poll();
            q3.offer(3 * val);
            q5.offer(5 * val);
        } else if (val == v5) {
            q5.poll();
            q5.offer(5 * val);
        }
    }

    return (int)val;
}
```
