# 2491 Divide Players Into Teams of Equal Skill

You are given a positive integer array `skill` of **even** length `n` where `skill[i]` denotes the skill of the `ith` player. Divide the players into `n / 2` teams of size `2` such that the total skill of each team is **equal**.

The **chemistry** of a team is equal to the **product** of the skills of the players on that team.

Return _the sum of the **chemistry** of all the teams, or return_ `-1` _if there is no way to divide the players into teams such that the total skill of each team is equal._

&#x20;

**Example 1:**

<pre><code><strong>Input: skill = [3,2,5,1,3,4]
</strong><strong>Output: 22
</strong><strong>Explanation: 
</strong>Divide the players into the following teams: (1, 5), (2, 4), (3, 3), where each team has a total skill of 6.
The sum of the chemistry of all the teams is: 1 * 5 + 2 * 4 + 3 * 3 = 5 + 8 + 9 = 22.
</code></pre>

**Example 2:**

<pre><code><strong>Input: skill = [3,4]
</strong><strong>Output: 12
</strong><strong>Explanation: 
</strong>The two players form a team with a total skill of 7.
The chemistry of the team is 3 * 4 = 12.
</code></pre>

**Example 3:**

<pre><code><strong>Input: skill = [1,1,2,3]
</strong><strong>Output: -1
</strong><strong>Explanation: 
</strong>There is no way to divide the players into teams such that the total skill of each team is equal.
</code></pre>

&#x20;

**Constraints:**

* `2 <= skill.length <= 105`
* `skill.length` is even.
* `1 <= skill[i] <= 1000`

这题是[1 Two Sum](1-two-sum.md)的马甲题，这里多了点判断。譬如，我们要判断team的数目，还有能不能整除。如果加起来的分数不能被team的数目整除，那么直接返回-1无解。这里找的target是整出后每个team的分数。然后，因为是double，所以纠结了很久到底能不能用hashmap，估计够呛。所以选择了排序，然后2ptr。判断的时候，因为是小数，所以用了一个epsilon来判断是否相等。最后返回的时候，要注意，如果分不成那么多个team，也是得返回-1。排了个序，所以T:O(nlogn)，S:O(1)。哪个判断是否整除要记记，得查Java api。

```java
public long dividePlayers(int[] skill) {
    if (skill == null || skill.length == 0) {
        return -1l;
    }

    int size = skill.length;
    if (size == 2) {
        return skill[0] * skill[1];
    }

    long sum = 0l;
    for (int ski : skill) {
        sum += ski;
    }
    
    int teamsCnt = size / 2;
    double target = sum / (teamsCnt * 1.0);
    if (target != Math.round(target)) {
        return -1;
    }

    double epi = 0.0000001;

    Arrays.sort(skill);
    int left = 0;
    int right = size - 1;
    long chemistry = 0l;
    while (left < right) {
        long twoSum = skill[left] + skill[right];
        if ((twoSum - target) < epi) {
            chemistry += skill[left] * skill[right];
            left++;
            right--;
            teamsCnt--;
        } else if (twoSum > target) {
            left++;
        } else {
            right--;
        }
    }

    return teamsCnt == 0 ? chemistry : -1;
}
```
