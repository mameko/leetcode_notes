# 760 Find Anagram Mappings



You are given two integer arrays `nums1` and `nums2` where `nums2` is **an anagram** of `nums1`. Both arrays may contain duplicates.

Return _an index mapping array_ `mapping` _from_ `nums1` _to_ `nums2` _where_ `mapping[i] = j` _means the_ `ith` _element in_ `nums1` _appears in_ `nums2` _at index_ `j`. If there are multiple answers, return **any of them**.

An array `a` is **an anagram** of an array `b` means `b` is made by randomizing the order of the elements in `a`.

&#x20;

**Example 1:**

<pre><code>Input: nums1 = [12,28,46,32,50], nums2 = [50,12,32,46,28]
<strong>Output: [1,4,3,2,0]
</strong><strong>Explanation:
</strong> As mapping[0] = 1 because the 0th element of nums1 appears at nums2[1], and mapping[1] = 4 because the 1st element of nums1 appears at nums2[4], and so on.
</code></pre>

**Example 2:**

<pre><code>Input: nums1 = [84,46], nums2 = [84,46]
<strong>Output: [0,1]
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums1.length <= 100`
* `nums2.length == nums1.length`
* `0 <= nums1[i], nums2[i] <= 105`
* `nums2` is an anagram of `nums1`.

还是一如既往的简单题，查java doc比想算法的时间还长。因为有dup，所以map的位置用了list来存。花了S:O(N),T:O(N)

```java
public int[] anagramMappings(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null || nums1.length != nums2.length) {
        return null;
    }
    
    int n = nums1.length;
    int[] result = new int[n];
    
    // <val, loc list>
    Map<Integer, List<Integer>> locMap = new HashMap<>();
    for (int i = 0; i < n; i++) {
        if (locMap.containsKey(nums2[i])) {
            locMap.get(nums2[i]).add(i);
        } else {
            List<Integer> tmp = new LinkedList<>();
            tmp.add(i);
            locMap.put(nums2[i], tmp);
        }
    }
    
    for (int i = 0; i < n; i++) {
        List<Integer> tmp = locMap.get(nums1[i]);
        result[i] = tmp.remove(0);
    }
    return result;
}
}j
```

