# 373 Find K Pairs with Smallest Sums

You are given two integer arrays **nums1**and **nums2** sorted in ascending order and an integer **k**.

Define a pai&#x72;**(u,v)**&#x77;hich consists of one element from the first array and one element from the second array.

Find the k pair&#x73;**(u1,v1),(u2,v2) ...(uk,vk)**&#x77;ith the smallest sums.

**Example 1:**

```
Given nums1 = [1,7,11], nums2 = [2,4,6],  k = 3

Return: [1,2],[1,4],[1,6]

The first 3 pairs are returned from the sequence:
[1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

**Example 2:**

```
Given nums1 = [1,1,2], nums2 = [1,2,3],  k = 2

Return: [1,1],[1,1]

The first 2 pairs are returned from the sequence:
[1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

**Example 3:**

```
Given nums1 = [1,2], nums2 = [3],  k = 3 

Return: [1,3],[2,3]

All possible pairs are returned from the sequence:
[1,3],[2,3]
```

这两个数组是递增的，所以它们的和会是一个按行和按列递增的矩阵。这就和前面的那条[L465 Kth Smallest Sum In Two Sorted Arrays](l465-kth-smallest-sum-in-two-sorted-arrays.md)很相似了。这里主要注意非法输入的判断和while loop的结束条件。因为如果k比所有可能解的数目都大的话，我们在输出所有可能解以后跳出循环。T:O(klog(对角线长度))，进堆log对角线长度，S:O(m\*n)，visited数组的大小。

```java
public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
    List<int[]> res = new ArrayList<>();
    if (nums1 == null || nums2 == null) {
        return res;
    } else if (nums1.length == 0 || nums2.length == 0) { 
        return res;
    } else if (k < 0) {
        return res;
    } 

    int n = nums1.length;
    int m = nums2.length;

    PriorityQueue<Node> pq = new PriorityQueue<>(k, new Comparator<Node> (){
        public int compare(Node n1, Node n2) {
            return n1.sum - n2.sum;
        }
    });
    boolean[][] visited = new boolean[n][m];

    pq.offer(new Node(0, 0, nums1[0] + nums2[0]));
    visited[0][0] = true;

    while (res.size() < k && res.size() < n * m) {
        Node cur = pq.poll();
        int i = cur.r;
        int j = cur.c;

        int[] tmp = new int[2];
        tmp[0] = nums1[i];
        tmp[1] = nums2[j];
        res.add(tmp);

        if (i + 1 < n && !visited[i + 1][j]) {
            pq.offer(new Node(i + 1, j, nums1[i + 1] + nums2[j]));
            visited[i + 1][j] = true;
        }

        if (j + 1 < m && !visited[i][j + 1]) {
            pq.offer(new Node(i, j + 1, nums1[i] + nums2[j + 1]));
            visited[i][j + 1] = true;
        }
    }

    return res;
}


class Node {
    int r;
    int c;
    int sum;

    public Node(int i, int j, int s) {
        r = i;
        c = j;
        sum = s;
    }
}
```
