# 711 Number of Distinct Islands II

Given a non-empty 2D array `grid` of 0's and 1's, an **island** is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Count the number of **distinct** islands. An island is considered to be the same as another if they have the same shape, or have the same shape after **rotation** (90, 180, or 270 degrees only) or **reflection** (left/right direction or up/down direction).

**Example 1:**

```
11000
10000
00001
00011
```

Given the above grid map, return `1`.\
\
Notice that:

```
11
1
```

and

```
 1
11
```

are considered **same** island shapes. Because if we make a 180 degrees clockwise rotation on the first island, then two islands will have the same shapes.

**Example 2:**<br>

```
11100
10001
01001
01110
```

Given the above grid map, return `2`.\
\
Here are the two distinct islands:

```
111
1
```

and

```
1
1
```

\
Notice that:

```
111
1
```

and

```
1
111
```

are considered **same** island shapes. Because if we flip the first array in the up/down direction, then they have the same shapes.

**Note:** The length of each dimension in the given `grid` does not exceed 50.Accepted6,691Submissions13,589

这题的其实跟[694 Number of Distinct Islands](694-number-of-distinct-islands.md)思路一样。那么难题是怎么吧reflection和rotation的island编码呢？自己是想不出来了，看答案也看了半天。其实，rotation就是把现在的（x, y)做一下变换：x y, x -y, -x y, -x -y；reflection就是把现在的左边做以下变换：y x, y -x, -y x, -y -x。我们把组成island的每一个点都做一遍这样的变换，找出8种形态。每个形态，我们要loop一遍，算一下这个岛跟最左上一点的差值。然后排序，用所产生的数字最小值作为key。至于为什么这个编码过程是work的，就不太懂了。T:O(r \* clog(r \* c)),这个log是因为要给8种形态排序。S:O(r \* c)，递归深度

```java
class Solution {
	int[][] grid;
	boolean[][] seen;
	ArrayList<Integer> shape;

	public void explore(int r, int c) {
		if (0 <= r && r < grid.length && 0 <= c && c < grid[0].length && grid[r][c] == 1 && !seen[r][c]) {
			seen[r][c] = true;
			shape.add(r * grid[0].length + c);
			explore(r + 1, c);
			explore(r - 1, c);
			explore(r, c + 1);
			explore(r, c - 1);
		}
	}
	// x y, x -y, -x y, -x -y, --- rotation
	// y x, y -x, -y x, -y -x --- reflection 
	int[] xMul = {1, 1, -1, -1, 1, -1, 1, -1};
	int[] yMul = {1, -1, 1, -1, 1, 1, -1, -1};
	public String canonical(ArrayList<Integer> shape) {
		String ans = "";
		int lift = grid.length + grid[0].length;
		int[] out = new int[shape.size()];
		int[] xs = new int[shape.size()];
		int[] ys = new int[shape.size()];

		for (int c = 0; c < 8; ++c) {
			int t = 0;
			for (int z : shape) {
				int x = z / grid[0].length;
				int y = z % grid[0].length;

				if (c <= 3) {
					xs[t] = x * xMul[c];
					ys[t] = y * yMul[c];
				} else {
					xs[t] = y * xMul[c];
					ys[t] = x * yMul[c];
				}
				t++;
			}

			int mx = xs[0], my = ys[0];
			for (int x : xs)
				mx = Math.min(mx, x);
			for (int y : ys)
				my = Math.min(my, y);

			for (int j = 0; j < shape.size(); ++j) {
				out[j] = (xs[j] - mx) * lift + (ys[j] - my);
			}
			Arrays.sort(out);
			String candidate = Arrays.toString(out);
			if (ans.compareTo(candidate) < 0)
				ans = candidate;
		}
		return ans;
	}

	public int numDistinctIslands2(int[][] grid) {
		this.grid = grid;
		seen = new boolean[grid.length][grid[0].length];
		Set shapes = new HashSet<String>();

		for (int r = 0; r < grid.length; ++r) {
			for (int c = 0; c < grid[0].length; ++c) {
				shape = new ArrayList();
				explore(r, c);
				if (!shape.isEmpty()) {
					shapes.add(canonical(shape));
				}
			}
		}

		return shapes.size();
	}
}
```
