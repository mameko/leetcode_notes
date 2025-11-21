# 350 Intersection of Two Arrays II

Given two arrays, write a function to compute their intersection.

**Example:**\
Given nums1=`[1, 2, 2, 1]`,nums2=`[2, 2]`, return`[2, 2]`.

**Note:**

* Each element in the result should appear as many times as it shows in both arrays.
* The result can be in any order.

**Follow up:**

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

这题跟前面一题349 Intersection of Two Arrays很像，只是要把set换成map而已。还是有3种方法。

```java
public int[] intersect(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null || nums1.length == 0 || nums2.length == 0) {
        return new int[0];
    }

    // <val, count>
    HashMap<Integer, Integer> hm = new HashMap<>();
    for (int i = 0; i < nums1.length; i++) {
        int cur = nums1[i];
        if (hm.containsKey(cur)) {
            hm.put(cur, hm.get(cur) + 1);
        } else {
            hm.put(cur, 1);
        }
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    for (int i = 0; i < nums2.length; i++) {
        int cur = nums2[i];
        if (hm.containsKey(cur) && hm.get(cur) > 0) {
            tmp.add(cur);
            hm.put(cur, hm.get(cur) - 1);
        }
    }

    int[] res = new int[tmp.size()];
    int j = 0;
    for (Integer elem : tmp) {
        res[j++] = elem;
    }

    return res;
}
```

解法二：sort + merge，跟前一题的唯一不同是这次用的是arraylist作为中间结果容器，上一题因为要去重，所以用了set。

```java
public int[] intersection(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null) {
        return new int[0];
    }

    Arrays.sort(nums1);
    Arrays.sort(nums2);

    ArrayList<Integer> tmp = new ArrayList<>();
    int i = 0;
    int j = 0;
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] == nums2[j]) {
            tmp.add(nums1[i]);
            i++;
            j++;
        } else if (nums1[i] < nums2[j]) {
            i++;
        } else {
            j++;
        }
    }

    int[] res = new int[tmp.size()];
    int k = 0;
    for (Integer in : tmp) {
        res[k++] = in;
    }

    return res;

}
```
