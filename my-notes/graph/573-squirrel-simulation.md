# 573 Squirrel Simulation

There's a tree, a squirrel, and several nuts. Positions are represented by the cells in a 2D grid. Your goal is to find the **minimal** distance for the squirrel to collect all the nuts and put them under the tree one by one. The squirrel can only take at most **one nut** at one time and can move in four directions - up, down, left and right, to the adjacent cell. The distance is represented by the number of moves.

**Example 1:**

```
Input: 
Height : 5
Width : 7
Tree position : [2,2]
Squirrel : [4,4]
Nuts : [[3,0], [2,5]]
Output: 12
Explanation:
​​​​​
```

**Note:**

1. All given positions won't overlap.
2. The squirrel can take at most one nut at one time.
3. The given positions of nuts have no order.
4. Height and width are positive integers. 3 <= height \* width <= 10,000.
5. The given positions contain at least one nut, only one tree and one squirrel.

这题，一看就觉得是曼哈顿距离的。但是呢，推断的时候错了。以为只有最后一颗nut是得加双倍距离。其实，所有nuts到Tree的距离都得X2，因为都得来回跑。然后只有第一颗是从松鼠到nut到树的。那么怎么找从哪一颗nuts开始呢？其实是在loop的时候，顺便找dis(nut, tree) - dis(nut, squirrel)的max。因为我们先无脑地加了两倍某nut到tree的距离。dis(nut, tree) + dis(nut, tree)。但其实，我们只要加dis(nut, tree) + dis(nut, squirrel)就好了，所以差值就是dis(nut, tree) + dis(nut, tree) - dis(nut, tree) - dis(nut, squirrel) = dis(nut, tree) - dis(nut, squirrel)。T：O(N), S:O(1)

```java
public int minDistance(int height, int width, int[] tree, int[] squirrel, int[][] nuts) {
    if (tree == null || tree.length == 0 || squirrel == null || squirrel.length == 0 || nuts == null || nuts.length == 0) {
        return Integer.MAX_VALUE;        
    }

    int distance = 0;
    int maxSaving = Integer.MIN_VALUE;
    //All NUTs distance to Tree needs to be added
    for (int i = 0; i < nuts.length; i++) {
        distance += 2 * findDistance(nuts[i], tree);
        maxSaving = Math.max(maxSaving, findDistance(nuts[i], tree) - findDistance(nuts[i], squirrel)) ;           
    }

    return distance - maxSaving;
}

private int findDistance(int[] nuts, int[] treeOrSquirrel) {
    return Math.abs(nuts[0] - treeOrSquirrel[0]) + Math.abs(nuts[1] - treeOrSquirrel[1]);
}
```

