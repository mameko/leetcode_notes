# 10 Common Complexities

**Algorithms**

| **Algorithm**                                   | **Time Complexity**      |
| ----------------------------------------------- | ------------------------ |
| Binary Search on array                          | O(logN)                  |
| Reversing a string                              | O(len)                   |
| Linear search                                   | O(N)                     |
| Compare Nth fibonacci number                    | O(N)                     |
| Compare two strings                             | O(min(Len1, Len2))       |
| Check if a string is palindrome                 | O(N)                     |
| Sorting (merge sort, quick sort avg, heap sort) | O(nlogn)                 |
| Sorting (bubble sort, quick sort worst etc.)    | O(n^2)                   |
| Two nested loop                                 | O(n^2)                   |
| Strstr                                          | O(n \* m) / KMP O(n + m) |
| Subsets                                         | O(2^n)                   |
| Permutations                                    | O(n!)                    |

**Data Structure**

|                       | TreeMap | Heap | PriorityQueue |
| --------------------- | ------- | ---- | ------------- |
| Insert                | logn    | logn | logn          |
| delete任意node          | logn    | logn | X             |
| pop (移除顶点）            | logn    | logn | logn          |
| find                  | long    | logn | X             |
| modify                | logn    | logn | X             |
| min/max (peek)        | logn    | O(1) | O(1)          |
| upperbound/lowerbound | logn    | X    | X             |

|                   | ArrayList | LinkedList |
| ----------------- | --------- | ---------- |
| set(index)        | O(1)      | O(n)       |
| add(E)            | O(n)      | O(1)       |
| add(E, index)     | O(n)      | O(n)       |
| remove(index)     | O(n)      | O(n)       |
| Iterator.remove() | O(n)      | O(1)       |
| Iterator.add(E)   | O(n)      | O(1)       |

**Power of 2 table**

| Power of 2 | Exac Value(X)     | Approximate value | X Bytes into MB, GB, etc |
| ---------- | ----------------- | ----------------- | ------------------------ |
| 7          | 128               |                   |                          |
| 8          | 256               |                   |                          |
| 10         | 1024              | 1 thousand        | 1KB                      |
| 16         | 65,535            |                   | 64KB                     |
| 20         | 1,048,576         | 1 million         | 1MB                      |
| 30         | 1,073,741,824     | 1 billion         | 1GB                      |
| 32         | 4,294,967,296     |                   | 4GB                      |
| 40         | 1,099,511,627,776 | 1 trillion        | 1TB                      |

![](../.gitbook/assets/screen-shot-2018-04-28-at-174531.png)
