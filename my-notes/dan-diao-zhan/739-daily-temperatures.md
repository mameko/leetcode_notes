# 739 Daily Temperatures

Given a list of daily temperatures`T`, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put`0`instead.

For example, given the list of temperatures`T = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be`[1, 1, 4, 2, 1, 1, 0, 0]`.

**Note:**&#x54;he length of`temperatures`will be in the range`[1, 30000]`. Each temperature will be an integer in the range`[30, 100]`.

这题是单调栈。每次存下标方便计算。因为要找的是比现在这个数大的数，所以每次进栈前比较一下当前数和栈里的数。把所有比自己小的踢出来，踢的时候顺便算一下下标的差距（这就是要隔几天）。因为数组里元素本身inti为0，最后的没填的那些空格（没被提出来的；以后没有遇到比这更高温度的日子）自动就是0。T:O(N) S:O(N)

```java
public int[] dailyTemperatures(int[] T) {
    if (T == null || T.length == 0) {
        return new int[0];
    }

    int len = T.length;
    int[] res = new int[len];
    Stack<Integer> stack = new Stack<>();
    for (int i = 0; i < len; i++) {
        int cur = T[i];
        while (!stack.isEmpty() && cur > T[stack.peek()]) {
            int ind = stack.pop();
            res[ind] = i - ind;
        }
        stack.push(i);
    }

    return res;
}
```
