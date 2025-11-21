# 1257 Smallest Common Region

You are given some lists of `regions` where the first region of each list includes all other regions in that list.

Naturally, if a region `x` contains another region `y` then `x` is bigger than `y`. Also, by definition, a region `x` contains itself.

Given two regions: `region1` and `region2`, return _the smallest region that contains both of them_.

If you are given regions `r1`, `r2`, and `r3` such that `r1` includes `r3`, it is guaranteed there is no `r2` such that `r2` includes `r3`.

It is guaranteed the smallest region exists.

&#x20;

**Example 1:**

<pre><code><strong>Input:
</strong>regions = [["Earth","North America","South America"],
["North America","United States","Canada"],
["United States","New York","Boston"],
["Canada","Ontario","Quebec"],
["South America","Brazil"]],
region1 = "Quebec",
region2 = "New York"
<strong>Output: "North America"
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: regions = [["Earth", "North America", "South America"],["North America", "United States", "Canada"],["United States", "New York", "Boston"],["Canada", "Ontario", "Quebec"],["South America", "Brazil"]], region1 = "Canada", region2 = "South America"
</strong><strong>Output: "Earth"
</strong></code></pre>

&#x20;

**Constraints:**

* `2 <= regions.length <= 104`
* `2 <= regions[i].length <= 20`
* `1 <= regions[i][j].length, region1.length, region2.length <= 20`
* `region1 != region2`
* `regions[i][j]`, `region1`, and `region2` consist of English letters.

这题，看上去很像有parent指针的LCA。但想了半天都没想出来怎么build那个tree。后来看了答案，原来不需要build tree，只需要把每个点与TA parent的关系放到map里方便查询就ok了。先建立mapping，然后找region1的root to path。这里root的parent会是null，所以找到null为止 （这题因为两个点都在map里）。结果放到Set里方便找region2的时候查询。不然就得弄两个list来intersection了。从region2一直往上找，找到的第一个同样在region1的parent list（set）里的，就是我们的LCA。T：O(map size)，找parent只是“树”的高度，建立”树“mapping的时候要整个map都过一遍，复杂度最高。S：O(map size),同样，path to root也只是“树”高，“树”所占的空间才是大头。

```java
public String findSmallestRegion(List<List<String>> regions, String region1, String region2) {
   if (regions == null || regions.get(0) == null || region1 == null || region2 == null) {
       return null;
   }

   // 1. create parent mapping
   Map<String, String> regionToParent = createMapping(regions);

   // 2. find path to root, region1
   Set<String> parents = new HashSet<>();
   while (region1 != null) {
       parents.add(region1);
       region1 = regionToParent.get(region1);
   }
   
    // 3. find path to root, region2, and check whether we found lca
    while (!parents.contains(region2)) {
        region2 = regionToParent.get(region2);
    }

    return region2;
}

private Map<String, String> createMapping(List<List<String>> regions) {
    Map<String, String> result = new HashMap<>();
    
    for (List<String> region : regions) {
        for (int i = 1; i < region.size(); i++) {
            // child -> parent
            result.put(region.get(i), region.get(0));
        }
    }

    return result;
}
```



