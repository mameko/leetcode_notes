# 349 Intersection of Two Arrays

Given two arrays, write a function to compute their intersection.

**Example:**\
Givennums1=`[1, 2, 2, 1]`,nums2=`[2, 2]`, return`[2]`.

**Note:**

* Each element in the result must be unique.
* The result can be in any order.

这题其实有3中解法，第一是用set（当两个array都能存memory时）T: O (n + m) S: O(min(n, m));第二是先排序，然后merge 2 array时找intersection T:O(nlogn + mlogm) S:O(1);第三是如果有一个array特别大，这样的话，就把小的排序，再用大的进去2分找，T：O(nlogn + mlogn) S:O(1)。因为return的是int\[] array所以中间用了tmp，所以也不是strictly O(1).但如果返回值不是int\[]，可以做到O(1)

方法1：

```java
public int[] intersection(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null || nums1.length == 0 || nums2.length == 0) {
        return new int[0];
    }

    // method 1, use two hashset
    HashSet<Integer> hs1 = new HashSet<>();
    for (int i = 0; i < nums1.length; i++) {
        hs1.add(nums1[i]);
    }

    HashSet<Integer> hs2 = new HashSet<>();
    for (int i = 0; i < nums2.length; i++) {
        int cur = nums2[i];
        if (hs1.contains(cur)) {
            hs2.add(cur);
        }
    }

    int[] res = new int[hs2.size()];
    int i = 0;
    for (Integer elem : hs2) {
        res[i++] = elem;
    }

    return res;        
}
```

方法2：

```java
 public int[] intersection(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null || nums1.length == 0 || nums2.length == 0) {
        return new int[0];
    }
    // method 2, sort both array and merge
    Arrays.sort(nums1);
    Arrays.sort(nums2);

    HashSet<Integer> hs = new HashSet<>();
    int i = 0;
    int j = 0;
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] == nums2[j]) {
            hs.add(nums1[i]);
            i++;
            j++;
        } else if (nums1[i] < nums2[j]) {
            i++;
        } else {
            j++;
        }
    }

    int[] res = new int[hs.size()];
    int k = 0;
    for (Integer elem : hs) {
        res[k++] = elem;
    }

    return res;              
}
```

方法3：

```java
public int[] intersection(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null || nums1.length == 0 || nums2.length == 0) {
        return new int[0];
    }
    // method 3，sort smaller array, then do binary search in it for every elem in large array
    Arrays.sort(nums1);// assum nums1 is shorter
    HashSet<Integer> hs = new HashSet<>();   

    for (int i = 0; i < nums2.length; i++) {
        int cur = nums2[i];
        if(!hs.contains(cur) && Arrays.binarySearch(nums1, cur) > -1) {
            hs.add(cur);
        }
    }

    int[] res = new int[hs.size()];
    int k = 0;
    for (Integer elem : hs) {
        res[k++] = elem;
    }

    return res;  

}
```
