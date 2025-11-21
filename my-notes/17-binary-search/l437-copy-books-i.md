# L437 copy books I

Given\_n\_books and the\_i\_th book has`A[i]`pages. You are given\_k\_people to copy the\_n\_books.

the\_n\_books list in a row and each person can claim a continuous range of the\_n\_books. For example one copier can copy the books from\_i\_th to\_j\_th continously, but he can not copy the 1st book, 2nd book and 4th book (without 3rd book).

They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time?

**Example**

Given array A =`[3,2,4]`, k =`2`.

Return`5`( First person spends 5 minutes to copy book 1 and book 2 and second person spends 4 minutes to copy book 3. )

方法一：2分答案。O(nlog(range))

```java
public int copyBooks(int[] pages, int k) {
    if (pages == null || pages.length == 0 || k < 1) {
        return 0;    
    }

    int n = pages.length;
    // find lower bounds. 
    // because a person cannot copy partial book, 
    // so max pages of the book is lower bound & upper bounds.
    // worst case we only have 1 person to copy all books, 
    // so sum of pages of all book is upper bound
    int left = 0;
    int right = 0;
    for (int i = 0; i < n; i++) {
        left = Math.max(left, pages[i]);
        right += pages[i];
    }

    // do binary search on results, 
    // find smallest/1st mins that satisfy copying the books
    while (left + 1 < right) {
        int mid = left + (right - left) / 2;
        if (canFinish(mid, pages, k)) {
            right = mid;
        } else {
            left = mid;
        }
    }

    if (canFinish(left, pages, k)) {
        return left;
    } else {
        return right;
    }
}

private boolean canFinish(int minutes, int[] pages, int k) {
    int restTime = minutes;// rest time for preson ki
    int i = 0;
    int ki = 1;
    while (i < pages.length) {
        if (restTime >= pages[i]) {
            restTime = restTime - pages[i];
            i++;
        } else {
            restTime = minutes;
            ki++;
            if (ki > k) {
                return false;
            }
        }
    }

    return true;
}
```

方法二：DP
