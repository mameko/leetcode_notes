# L254 Drop Egg

There is a building of`n`floors. If an egg drops from the\_k\_th floor or above, it will break. If it's dropped from any floor below, it will not break.

You're given two eggs, Find\_k\_while minimize the number of drops for the worst case. Return the number of drops in the worst case.

**Clarification**

For n = 10, a naive way to find\_k\_is drop egg from 1st floor, 2nd floor ... kth floor. But in this worst case (k = 10), you have to drop 10 times.

Notice that you have two eggs, so you can drop at 4th, 7th & 9th floor, in the worst case (for example, k = 9) you have to drop 4 times.

**Example**

Given n =`10`, return`4`.\
Given n =`100`, return`14`.

这题是要求符合这个公式的第一个解，k \*（k + 1）> n

```java
public int dropEggs(int n) {
    if (n < 1) {
        return 1;
    }

    //find 1st k that k * (k + 1) > n
   long start = 1;
   long end = n;

   while (start + 1 < end) {
       long mid = start + (end - start) / 2;

       if (check(mid, n)) {
           start = mid;
       } else {
           end = mid;
       }
   }

    if (!check(start, n)) {
        return (int)start;
    }

    if (!check(end, n)) {
        return (int)end;
    }

    return 1;
}

private boolean check(long num, int n) {
    long mul = num * (num + 1) / 2;
    return mul < n;
}
```
