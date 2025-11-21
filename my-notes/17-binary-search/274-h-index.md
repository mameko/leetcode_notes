# 274 H-Index

Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index h if h of his/her N papers have **at least** h citations each, and the otherN − hpapers have **no more than** hcitations each."

For example, given`citations = [3, 0, 6, 1, 5]`, which means the researcher has`5`papers in total and each of them had received`3, 0, 6, 1, 5`citations respectively. Since the researcher has`3`papers with **at least**`3`citations each and the remaining two with **no more than**`3`citations each, his h-index is`3`.

**Note**: If there are several possible values for`h`, the maximum one is taken as the h-index.

这题，不太懂要搞毛，一开始以为排个序，找值大于下标的位置。然后发现不对，看了答案一号才知道是反着排序，然后找第一个下标比值大的位置。后来看了solution再一次发现，还能用桶排，达到O(n)的境界。

答案一号：因为要排序所以O(nlogn)

```java
public int hIndex(int[] citations) {
    if (citations == null || citations.length == 0) {
        return 0;
    }

    int n = citations.length;
    Arrays.sort(citations);
    for (int i = n - 1; i >= 0; i--) {
        if (citations[i] <= n - i - 1) {
            return n - i - 1;
        }
    }

    return n;
}
```

答案二号：

```java
public int hIndex(int[] citations) {
    if (citations == null || citations.length == 0) {
        return 0;
    }

    int n = citations.length;
    // define buckets
    int[] papers = new int[n + 1];
    for (Integer in : citations) {
        papers[Math.min(n, in)]++;
        // eg. [1, 3, 2, 3, 100] 
        // it dosen't matter we have value 100 or 5 at the last spot, 
        // we count 1 to indicate we have 1 paper that get cited at least 5 times
    }

    int k = n;
    // add them up from the back, suffix sum
    for (int i = papers[n]; k > i; i += papers[k]) {
        k--;
    }
    // k   = [0, 1, 2, 3, 4, 5]
    // i   = [5, 5, 4, 3, 1, 1]
    //                this

    return k;
}
```
