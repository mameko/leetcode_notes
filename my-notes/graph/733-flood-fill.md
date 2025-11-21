# 733 Flood Fill

An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.

You are also given three integers `sr`, `sc`, and `color`. You should perform a **flood fill** on the image starting from the pixel `image[sr][sc]`.

To perform a **flood fill**, consider the starting pixel, plus any pixels connected **4-directionally** to the starting pixel of the same color as the starting pixel, plus any pixels connected **4-directionally** to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with `color`.

Return _the modified image after performing the flood fill_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg)

<pre><code><strong>Input: image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2
</strong><strong>Output: [[2,2,2],[2,2,0],[2,0,1]]
</strong><strong>Explanation: From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.
</strong>Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.
</code></pre>

**Example 2:**

<pre><code><strong>Input: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0
</strong><strong>Output: [[0,0,0],[0,0,0]]
</strong><strong>Explanation: The starting pixel is already colored 0, so no changes are made to the image.
</strong></code></pre>

&#x20;

**Constraints:**

* `m == image.length`
* `n == image[i].length`
* `1 <= m, n <= 50`
* `0 <= image[i][j], color < 216`
* `0 <= sr < m`
* `0 <= sc < n`

经典bfs求最短路径，用了之前pramp里越南哥的方法存queue。注意，一定要开一个visited，不然很容易出错！TEL。T:O(n \* m), S:O(n \* m), visited用了

```java
public int[][] floodFill(int[][] image, int sr, int sc, int color) {
	if (image == null || image.length == 0 || image[0].length == 0) {
		return image;
	}

	int[] x = { 0, 0, 1, -1 };
	int[] y = { 1, -1, 0, 0 };

	int rowLen = image.length;
	int colLen = image[0].length;
        boolean[][] visited = new boolean[rowLen][colLen];

	Queue<int[]> queue = new LinkedList<>();
	queue.offer(new int[] { sr, sc });
	int oldColor = image[sr][sc];
	image[sr][sc] = color;
        visited[sr][sc] = true;

	while (!queue.isEmpty()) {
		int[] cur = queue.poll();

		for (int k = 0; k < 4; k++) {
			int nexti = cur[0] + x[k];
			int nextj = cur[1] + y[k];                

			if (inbound(nexti, nextj, rowLen, colLen) && !visited[nexti][nextj] && image[nexti][nextj] == oldColor) {
				queue.offer(new int[] { nexti, nextj });
				image[nexti][nextj] = color;
                    visited[nexti][nextj] = true;
			}
		}
	}

	return image;
}

private boolean inbound(int i, int j, int r, int c) {
	if (i < 0 || j < 0 || i >= r || j >= c) {
		return false;
	}

	return true;
}
```

