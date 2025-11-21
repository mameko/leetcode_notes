# 881 Boats to Save People

The `i`-th person has weight `people[i]`, and each boat can carry a maximum weight of `limit`.

Each boat carries at most 2 people at the same time, provided the sum of the weight of those people is at most `limit`.

Return the minimum number of boats to carry every given person.  (It is guaranteed each person can be carried by a boat.)

**Example 1:**

```
Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)
```

**Example 2:**

```
Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)
```

**Example 3:**

```
Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
```

**Note**:

* `1 <= people.length <= 50000`
* `1 <= people[i] <= limit <= 30000`

这一题，我看着题目想了很久，到底是背包还是二分答案呢？后来还是用了二分答案。solution说是贪心。因为我们最多只需要N条船。然后判断K艘船够不够用的地方，我用了2ptr来解。因为2ptr要排序，所以T:O(NlogN)

```java
public int numRescueBoats(int[] people, int limit) {
    if (people == null || people.length == 0 || limit <= 0) {
        return 0;
    }

    Arrays.sort(people);

    int start = 0;
    int end = people.length;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (canSaveWith(mid, people, limit)) {
            end = mid;
        } else {
            start = mid;
        }
    }

    if (canSaveWith(start, people, limit)) {
        return start;
    } else {
        return end;
    }
}

private boolean canSaveWith(int boatCnt, int[] people, int limit) {
    int i = 0; 
    int j = people.length - 1;

    while (i <= j && boatCnt > 0) {
        if (people[i] + people[j] > limit) {                
            j--;
        } else {
            j--;
            i++;
        }
        boatCnt--;
    }

    return i > j;
}
```

贴一个solution的贪心，感觉很像...

```java
public int numRescueBoats(int[] people, int limit) {
    Arrays.sort(people);
    int i = 0, j = people.length - 1;
    int ans = 0;

    while (i <= j) {
        ans++;
        if (people[i] + people[j] <= limit)
            i++;
        j--;
    }

    return ans;
}
```
