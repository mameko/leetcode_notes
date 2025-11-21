# 599 Minimum Index Sum of Two Lists

```
599 Minimum Index Sum of Two Lists
```

Suppose Andy and Doris want to choose a restaurant for dinner, and they both have a list of favorite restaurants represented by strings.

You need to help them find out their**common interest**with the**least list index sum**. If there is a choice tie between answers, output all of them with no order requirement. You could assume there always exists an answer.

**Example 1:**

```
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]

Output: ["Shogun"]

Explanation: The only restaurant they both like is "Shogun".
```

**Example 2:**

```
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["KFC", "Shogun", "Burger King"]

Output: ["Shogun"]

Explanation: The restaurant they both like and have the least index sum is "Shogun" with index sum 1 (0+1).
```

**Note:**

1. The length of both lists will be in the range of \[1, 1000].
2. The length of strings in both lists will be in the range of \[1, 30].
3. The index is starting from 0 to the list length minus 1.
4. No duplicates in both lists.

这题一开始脑残手瓢没写好。后来发现，这不就是[349 Intersection of two array](../two-pointers-other/349-intersection-of-two-arrays.md)嘛。所以就基本照抄了。T：O(m + n), S:max(m, n)

```java
public String[] findRestaurant(String[] list1, String[] list2) {
    if (list1 == null || list1.length == 0 || list2 == null || list2.length == 0) {
        return null;
    }

    // <resturant, loc>
    Map<String, Integer> hm1 = new HashMap<>();
    for (int i = 0; i < list1.length; i++) { 
        hm1.put(list1[i], i);
    }

    int min = Integer.MAX_VALUE;        
    Map<String, Integer> hm2 = new HashMap<>();
    for (int i = 0; i < list2.length; i++) {
        String cur = list2[i];
        if (hm1.containsKey(cur)) {                
            int sum = i + hm1.get(cur);
            min = Math.min(min, sum);
            hm2.put(cur, sum);
        } 
    }

    // find mins
    List<String> tmp = new ArrayList<>();
    for (Map.Entry<String, Integer> entry : hm2.entrySet()) {
        if (entry.getValue() == min) {
            tmp.add(entry.getKey());
        }
    }

    // collect result
    String[] res = new String[tmp.size()];
    for (int i = 0; i < tmp.size(); i++) {
        res[i] = tmp.get(i);
    }

    return res;
}
```

然而，正确的写法是这样的：T：O(m + n), S:O(l1 \* x), x 是字符串的平均长度。

```java
public String[] findRestaurant(String[] list1, String[] list2) {
    HashMap < String, Integer > map = new HashMap < String, Integer > ();
    // 把list1的放hashmap
    for (int i = 0; i < list1.length; i++)
        map.put(list1[i], i);

    List < String > res = new ArrayList < > ();
    int min_sum = Integer.MAX_VALUE, sum;

    // 只加list2的加到比现在min小
    for (int j = 0; j < list2.length && j <= min_sum; j++) {
        // 如果找到重复的，算一下sum
        if (map.containsKey(list2[j])) {
            sum = j + map.get(list2[j]);
            // 如果发现比现在的小，清一下结果，把新答案加进去，再更新一下min
            if (sum < min_sum) {
                res.clear();
                res.add(list2[j]);
                min_sum = sum;
            } else if (sum == min_sum) // 如果发现跟min相等，加到结果里
                res.add(list2[j]);
        }
    }
    // 最后原来还有这种操作
    return res.toArray(new String[res.size()]);
}
```

最后这题还有一个比较有用的想法是不用hashmap。因为list1跟list2所组成的sum是有限的，我们可以把所有的sum枚举出来。然后我们从list1\[0]\&list2\[sum\[0] - i]开始找。如果发现相等的话，我们加到答案里。这里因为sum是一直递增的，所以我们一旦找到某个sum有结果的话，那个结果必定是最小的。所以可以break掉直接返回了。T: O((l1 + l2) ^2 \* x), nested loop upto l1 + l2 & string comparison takes x, x is average length of the strings. S: O(r \* x), r is len of result

```java
public String[] findRestaurant(String[] list1, String[] list2) {
    List < String > res = new ArrayList < > ();
    for (int sum = 0; sum < list1.length + list2.length - 1; sum++) {
        for (int i = 0; i <= sum; i++) {
            if (i < list1.length && sum - i < list2.length && list1[i].equals(list2[sum - i]))
                res.add(list1[i]);
        }
        if (res.size() > 0)
            break;
    }
    return res.toArray(new String[res.size()]);
}
```
