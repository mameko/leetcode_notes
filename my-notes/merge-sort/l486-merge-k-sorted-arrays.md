# L486 Merge K sorted Arrays

Given\_k\_sorted integer arrays, merge them into one sorted array.

**Example**

Given 3 sorted arrays:

```
[
  [1, 3, 5, 7],
  [2, 4, 6],
  [0, 8, 9, 10, 11]
]
```

return`[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`.

[**Challenge**](http://www.lintcode.com/en/problem/merge-k-sorted-arrays/#challenge)

Do it in O(N log k).

* \_N \_is the total number of integers.
* \_k \_is the number of arrays.

基本上跟前面些题没什么区别，但要注意处理现在loop到哪条，和那条的下标。

```java
// 抄一个九章的
class Element {
    public int row, col, val;
    Element(int row, int col, int val) {
        this.row = row;
        this.col = col;
        this.val = val;
    }
}

public class Solution {
    private Comparator<Element> ElementComparator = new Comparator<Element>() {
        public int compare(Element left, Element right) {
            return left.val - right.val;
        }
    };
    
    /**
     * @param arrays k sorted integer arrays
     * @return a sorted array
     */
    public int[] mergekSortedArrays(int[][] arrays) {
        if (arrays == null) {
            return new int[0];
        }
        
        int total_size = 0;
        Queue<Element> Q = new PriorityQueue<Element>(
            arrays.length, ElementComparator);
            
        for (int i = 0; i < arrays.length; i++) {
            if (arrays[i].length > 0) {
                Element elem = new Element(i, 0, arrays[i][0]);
                Q.add(elem);
                total_size += arrays[i].length;
            }
        }
        
        int[] result = new int[total_size];
        int index = 0;
        while (!Q.isEmpty()) {
            Element elem = Q.poll();
            result[index++] = elem.val;
            if (elem.col + 1 < arrays[elem.row].length) {
                elem.col += 1;
                elem.val = arrays[elem.row][elem.col];
                Q.add(elem);
            }
        }
        
        return result;
    }
}

// 我的
public List<Integer> mergekSortedArrays(int[][] arrays) {
    List<Integer> res = new ArrayList<>();
    if (arrays == null || arrays.length == 0) {
        return res;
    }

    // use to store index of each array [1][i]
    // and the array len [0][i]
    int[][] cnt = new int[2][arrays.length];
    PriorityQueue<Pair> pq = new PriorityQueue<>();
    for (int i = 0; i < arrays.length; i++) {
        cnt[0][i] = arrays[i].length;
        if (arrays[i].length > 0) {
            int num = arrays[i][0];
            pq.offer(new Pair(num, i));
            cnt[1][i]++;
        }
    }         

       while (!pq.isEmpty()) {
            Pair tmp = pq.poll();
            res.add(tmp.val);
            // still have elem left to sort in that array
            if (cnt[0][tmp.loc] > cnt[1][tmp.loc]) {
                // add next elem to pq
                int num = arrays[tmp.loc][cnt[1][tmp.loc]];
                pq.offer(new Pair(num, tmp.loc));
                cnt[1][tmp.loc]++;
            }
        }

    return res;
}
}

class Pair implements Comparable<Pair> {
    Integer val;
    Integer loc;

    public Pair(Integer v, Integer l) {
        val = v;
        loc = l;
    }

    @Override
    public int compareTo(Pair o) {
        return val.compareTo(o.val);
    }
}
```
