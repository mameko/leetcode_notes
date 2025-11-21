# 517 Super Washing Machines

You have**n**super washing machines on a line. Initially, each washing machine has some dresses or is empty.

For each**move**, you could choose**any m**(1 ≤ m ≤ n) washing machines, and pass**one dress**of each washing machine to one of its adjacent washing machines**at the same time**.

Given an integer array representing the number of dresses in each washing machine from left to right on the line, you should find the**minimum number of moves**to make all the washing machines have the same number of dresses. If it is not possible to do it, return -1.

**Example1**

```
Input: [1,0,5]
Output: 3

Explanation: 
1st move:    1     0 <-- 5    =>    1     1     4
2nd move:    1 <-- 1 <-- 4    =>    2     1     3    
3rd move:    2     1 <-- 3    =>    2     2     2
```

**Example2**

```
Input: [0,3,0]
Output: 2

Explanation:
1st move:    0 <-- 3     0    =>    1     2     0    
2nd move:    1     2 --> 0    =>    1     1     1
```

**Example3**

```
Input: [0,2,0]
Output: -1

Explanation:
It's impossible to make all the three washing machines have the same number of dresses.
```

**Note:**

1. The range of n is \[1, 10000].
2. The range of dresses number in a super washing machine is \[0, 1e5].

这又是一题不看答案完全不知道要怎么做的题。只知道首先得判断不能平分的情况，然后就想不到然后了。看了discuss里才发现原来不用在意哪个machine向哪边分衣服了。我们只用关系哪边差多少件衣服。我们用L和R表示左边和右边。每一边都可以计算出还需要多少件衣服。（左边应有衣服数 - 现在所有左边machine里有的总数，右边同理）如果L>0 && R>0，表示我们要从这个machine向两边分衣服，因为一次只能移一边所以移动的总量为|L| + |R|。然后如果L<0 && R<0,表示这个machine要从左或右边得到衣服，因为可以同时从两边得到，所以移动数目为max（|L|, |R|）。还有一种情况就是L>0 R<0 || L<0 R>0，这种情况衣服会从大那边分到小的那边去，所以移动数目同样是max（|L|, |R|）。这题用了下标和前缀和来快速算出左边和右边应有衣服件数从而减低复杂度。

例子：最后分完的时候avg是每个洗衣机有2件。

| machine | L             | R              | Answer （求max） |
| ------- | ------------- | -------------- | ------------- |
| 1       | 0 × 2 - 0 = 0 | 2 × 2 - 5 = -1 | 1             |
| 0       | 1 × 2 - 1 = 1 | 1 × 2 - 5 = -3 | 3             |
| 5       | 2 × 2 - 1 = 3 | 0 × 2 - 0 = 0  | 3             |

所以答案是3. 解释第二行：因为1机器左边有1部machine，所以分完之后应该有1 × 2这么多件衣服，但左边意见都没有，所以-0；同理，右边也应该有一台机器，所以分完后也是应该有1×2这么多件衣服，但现在有5件，所以有3件需要移动。

```java
public int findMinMoves(int[] machines) {
    if (machines == null || machines.length == 0) {
        return 0;
    }

    int n = machines.length;
    int[] preSum = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        preSum[i] = preSum[i - 1] + machines[i - 1];
    }

    if (preSum[n] % n != 0) {
        return -1;
    }

    int avg = preSum[n] / n;
    int max = 0;

    for (int i = 0; i < n; i++) {
        int L = i * avg - preSum[i];
        int R = (n - i - 1) * avg - (preSum[n] - preSum[i] - machines[i]);

        if (L > 0 && R > 0) {
            max = Math.max(max, Math.abs(L) + Math.abs(R));
        } else {
            max = Math.max(max, Math.max(Math.abs(L), Math.abs(R)));
        }
    }
    return max;
}
```
