# 403 Frog Jump

A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump waskunits, then its next jump must be eitherk- 1,k, ork+ 1 units. Note that the frog can only jump in the forward direction.

**Note:**

* The number of stones is ≥ 2 and is < 1,100.
* Each stone's position will be a non-negative integer < 2^31.
* The first stone's position is always 0.

**Example 1:**

```
[0,1,3,5,6,8,12,17]


There are a total of 8 stones.
The first stone at the 0th unit, second stone at the 1st unit,
third stone at the 3rd unit, and so on...
The last stone at the 17th unit.


Return true
. The frog can jump to the last stone by jumping 
1 unit to the 2nd stone, then 2 units to the 3rd stone, then 
2 units to the 4th stone, then 3 units to the 6th stone, 
4 units to the 7th stone, and 5 units to the 8th stone.
```

**Example 2:**

```
[0,1,2,3,4,8,9,11]
Return false
. There is no way to jump to the last stone as 
the gap between the 5th and 6th stone is too large.
```

这题感觉是坐标型。就是从哪里跳到哪里。一开始往backpack方面想，所以用了很多空间，开了一个dp\[0到最大stone值]\[所有可能的k值]。然后发现有些k值会重复，所以就用了set。然后发现不能这样new数组。然后就用hashmap吧。一开始还每个值给一个set，后来发现只要给有石头的位置set就行了。空的地方可以不用管。另外一个需要注意的地方是，算步数放set的时候要注意别把<=0的放进去，因为青蛙只能向前跳。听说这解法是T:O(n^2) S:O(n^2)

```java
public boolean canCross(int[] stones) {
    if (stones == null || stones.length == 0) {
        return false;
    }

    int maxStone = stones[stones.length - 1];
    Map<Integer, HashSet<Integer>> dp = new HashMap<>();

    for (Integer ind : stones) {
        dp.put(ind, new HashSet<>());
    }

    dp.put(0, new HashSet<Integer>());
    dp.get(0).add(1);//石头0只能跳一步


    for (Integer ind : stones) {
        HashSet<Integer> curSet = dp.get(ind);
        for (Integer step : curSet) {
            int curLen = ind + step;
            if (curLen <= maxStone && dp.containsKey(curLen)) {// 如果不越界，而且有石头，我们把可能的步数加上
                if (step - 1 > 0) {//把小于<=0的去掉
                    dp.get(curLen).add(step - 1);
                }
                dp.get(curLen).add(step);
                dp.get(curLen).add(step + 1);
            }
        }
    }

    return dp.get(maxStone).size() > 0;// 判断最后的石头是否有再跳的步数
}
```
