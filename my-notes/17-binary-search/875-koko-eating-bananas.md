# 875 Koko Eating Bananas



Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.

&#x20;

**Example 1:**

```
Input: piles = [3,6,7,11], h = 8
Output: 4
```

**Example 2:**

```
Input: piles = [30,11,23,4,20], h = 5
Output: 30
```

**Example 3:**

```
Input: piles = [30,11,23,4,20], h = 6
Output: 23
```

&#x20;

**Constraints:**

* `1 <= piles.length <= 104`
* `piles.length <= h <= 109`
* `1 <= piles[i] <= 109`

这题，做了好久，二分的思想基本一看就看出来。但是canEat()函数写了半天。一开始以为start是从min of piles开始，然后发现其实不是。譬如，piles = \[312884470] h = 968709470，每次取1就ok了。另外，一开始打算是loop piles，然后一个个减去，然后判断有没有负数。譬如，\[3, 6, 7, 11]，我们用4，一轮过后，\[-1, 2, 3, 7] -> \[-1, -2, -1, 3] -> \[-1, -2, -1, -1], 总共8次就可以把数组全减到0。但，问题是，loop的条件很难控制，而且因为pass reference，每次修改就得copy数组。后来想到，其实需要多少次减到0，可以用num/k然后round up。java google了半天才找到因该怎样写。S：O（1）因为没有用extra space。 T：O(n \* log Range), 每次判断要过一次piles，然后二分range

```java
public int minEatingSpeed(int[] piles, int h) {
    if (piles == null || piles.length == 0 || h < 1) {
        return 0;
    }
    
    int start = 1;
    int end = Integer.MIN_VALUE;
    
    for (int i = 0; i < piles.length; i++) {
        end = Math.max(piles[i], end);
    }
    
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (canEatAll(piles, h, mid)) {
            end = mid;
        } else {
            start = mid;
        }
    }
    
    if (canEatAll(piles, h, start)) {
        return start;
    } else {
        return end;
    }
}

private boolean canEatAll(int[] piles, int h, int k) {
    int requireTimes = 0;

    for (int j = 0; j < piles.length; j++) {
	requireTimes = requireTimes + (int) (Math.ceil(piles[j] / (k * 1.0)));
    }
    return h >= requireTimes;    
}
```
