# L438 copy books II

Given n books( the page number of each book is the same) and an array of integer with size k means k people to copy the book and the i th integer is the time i th person to copy one book). You must distribute the continuous id books to one people to copy. (You can give book A\[1],A\[2] to one people, but you cannot give book A\[1], A\[3] to one people, because book A\[1] and A\[3] is not continuous.) Return the number of smallest minutes need to copy all the books.

**Example**

Given n =`4`, array A =`[3,2,4]`, .

Return`4`( First person spends 3 minutes to copy book 1, Second person spends 4 minutes to copy book 2 and 3, Third person spends 4 minutes to copy book 4. )

方法一：2分答案。O(nlog(range))

```java
public int copyBooksII(int n, int[] times) {
    if (times == null || times.length == 0 || n < 1) {
        return 0;
    }

    // find upper & lower bounds
    // worst case : give all books to the slowest person to copy, 
    // (n * max in times)
    // best case : we can start with min in times, 
    // say if we only have one book and give it to the fastest person
    int fastest = Integer.MAX_VALUE;
    int slowest = Integer.MIN_VALUE;
    for (int i = 0; i < times.length; i++) {
        fastest = Math.min(fastest, times[i]);
        slowest = Math.max(slowest, times[i]);
    }

    // binary search, find the shortest amount of 
    // time/fist minutes to copy all books
    int left = fastest;
    int right = slowest * n;
    while (left + 1 < right) {
        int mid = left + (right - left) / 2;
        if (canFinishedIn(mid, times, n)) {
            right = mid;
        } else {
            left = mid;
        }
    }

    if (canFinishedIn(left, times, n)) {
        return left;
    } else {
        return right;
    }
}

/**
 * @param minutes : minutes allow
 * @param times: each person need to finish copy 1 book
 * @param n : number of book we need to copy
*/
private boolean canFinishedIn(int minutes, int[] times, int n) {
    int timeLeft = minutes;
    int bookAmt = 0;
    for (int i = 0; i < times.length; i++) {
        while (timeLeft >= times[i]) {
            timeLeft = timeLeft - times[i];
            bookAmt++;
        }
        timeLeft = minutes;
    }

    return bookAmt >= n ? true : false;
}
```

方法二：DP
