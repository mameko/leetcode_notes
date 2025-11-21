# 755 Pour Water

You are given an elevation map represents as an integer array `heights` where `heights[i]` representing the height of the terrain at index `i`. The width at each index is `1`. You are also given two integers `volume` and `k`. `volume` units of water will fall at index `k`.

Water first drops at the index `k` and rests on top of the highest terrain or water at that index. Then, it flows according to the following rules:

* If the droplet would eventually fall by moving left, then move left.
* Otherwise, if the droplet would eventually fall by moving right, then move right.
* Otherwise, rise to its current position.

Here, **"eventually fall"** means that the droplet will eventually be at a lower level if it moves in that direction. Also, level means the height of the terrain plus any water in that column.

We can assume there is infinitely high terrain on the two sides out of bounds of the array. Also, there could not be partial water being spread out evenly on more than one grid block, and each unit of water has to be in exactly one block.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/12/pour11-grid.jpg)

<pre><code><strong>Input: heights = [2,1,1,2,1,2,2], volume = 4, k = 3
</strong><strong>Output: [2,2,2,3,2,2,2]
</strong><strong>Explanation:
</strong>The first drop of water lands at index k = 3. When moving left or right, the water can only move to the same level or a lower level. (By level, we mean the total height of the terrain plus any water in that column.)
Since moving left will eventually make it fall, it moves left. (A droplet "made to fall" means go to a lower height than it was at previously.) Since moving left will not make it fall, it stays in place.

The next droplet falls at index k = 3. Since the new droplet moving left will eventually make it fall, it moves left. Notice that the droplet still preferred to move left, even though it could move right (and moving right makes it fall quicker.)

The third droplet falls at index k = 3. Since moving left would not eventually make it fall, it tries to move right. Since moving right would eventually make it fall, it moves right.

Finally, the fourth droplet falls at index k = 3. Since moving left would not eventually make it fall, it tries to move right. Since moving right would not eventually make it fall, it stays in place.

</code></pre>

![](<../../.gitbook/assets/image (26).png>)![](<../../.gitbook/assets/image (27).png>)

![](<../../.gitbook/assets/image (25).png>)![](<../../.gitbook/assets/image (28).png>)



**Example 2:**

<pre><code><strong>Input: heights = [1,2,3,4], volume = 2, k = 2
</strong><strong>Output: [2,3,3,4]
</strong><strong>Explanation: The last droplet settles at index 1, since moving further left would not cause it to eventually fall to a lower height.
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: heights = [3,1,3], volume = 5, k = 1
</strong><strong>Output: [4,4,4]
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= heights.length <= 100`
* `0 <= heights[i] <= 99`
* `0 <= volume <= 2000`
* `0 <= k < heights.length`

这题，看了半天没想出个所以然，brute force就是volume \* k。然后想想，是不是能单调栈来减低复杂度。然后没想通。后来看了答案。原来好像solution就是这个复杂度的。后面还有大神想出了O(v + k)的，虽然但是用了栈。但太复杂了，就决定背这个brute force的。（说实在，自己写了半天没写对，组后还是抄了人家答案）太多edge case，处理不好

基本上，就是先看左边，找最低的坑，但是如果遇到比现在位置高的地方，就break掉，去右边找。中间那个continue是用于从最左边的地方开始drop water的情况。第一个loop会跳过。然后，最后哪里不用if是因为，那个index会指到右边最低的地方，或者指到k如果两边都找不到比k位置更矮的洼地。

<pre class="language-java"><code class="lang-java"><strong>public int[] pourWater(int[] heights, int volume, int k) {
</strong>    if (heights == null || heights.length == 0 || volume &#x3C; 1 || k &#x3C; 0) {
        return heights;
    }

    int len = heights.length;        
    while (volume > 0) {
        int index = k;            
        // pour water to left
        // find water landing place, index pointing to lowest location
        for (int toLeft = k - 1; toLeft >= 0; toLeft--) {
            if (heights[toLeft] > heights[index]) {
                break;
            }
            if (heights[toLeft] &#x3C; heights[index]) {
               index = toLeft;
            }
        }         

        // because if k = 0, won't enter first loop
        if (index != k) {
            heights[index]++;
            volume--;
            continue;
        }

        // pour water to right
        // find water landing place, index pointing to lowest location
        for (int toRight = k + 1; toRight &#x3C; len; toRight++) {
            if (heights[toRight] > heights[index]) {
                break;
            }
            if (heights[toRight] &#x3C; heights[index]) {
                index = toRight;
            }
        }
        
        // index will be k if can't find anything on left or right, pour at current location
        // or index will point to right lowest
        heights[index]++;
        volume--;                
    }

    return heights;
}
</code></pre>
