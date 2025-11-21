# 128 Longest Consecutive Sequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

**Clarification**

Your algorithm should run in O(n) complexity.

**Example**

Given`[100, 4, 200, 1, 3, 2]`,\
The longest consecutive elements sequence is`[1, 2, 3, 4]`. Return its length:`4`.

不明觉厉，自己是想不出来的了。抄下来慢慢研究。经过老师讲解，发现还是挺好理解的。最原始的方法是O(nlogn)排个序。但是我们发现每个数字在判断它的最长连续时只会被访问到常数项次。例如，100，你只会看我有没有101和99.没有就下一个。所以这题可以推断出能有O(n)的解法。我们用一个set来表示没有访问过的，那些数字。每次访问了，就移除这个集合。（因为4的longest consecutive和1，2，3的是一样的。所以检查一个去掉一个。）我们先向上找找，再向下找找，最后算一下这个区间。例如，100，我们找不到101，找不到99，101 - 99 = 2，但我们知道100的longest consecutive是1，所以最后要多减个1.

```java
public int longestConsecutive(int[] num) {
    // write your code here
    if (num == null || num.length == 0) {
        return 0;
    }

    int max = 0;
    HashSet<Integer> set = new HashSet<>();
    for (int n : num) {
        set.add(n);
    }

    for (int i = 0; i < num.length; i++) {
        int cur = num[i];
        if (set.contains(cur)) {
            set.remove(cur);
        }

        int down = cur - 1;
        while (set.contains(down)) {
            set.remove(down--);
        }

        int up = cur + 1;
        while (set.contains(up)) {
            set.remove(up++);
        }

        max = Math.max(max, up - down - 1);
    }

    return max;
}
```
