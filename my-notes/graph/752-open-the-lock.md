# 752 Open the Lock

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots:`'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example we can turn`'9'`to be`'0'`, or`'0'`to be`'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at`'0000'`, a string representing the state of the 4 wheels.

You are given a list of`deadends`dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a`target`representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

**Example 1:**

```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6

Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```

**Example 2:**

```
Input: deadends = ["8888"], target = "0009"
Output: 1

Explanation:
We can turn the last wheel in reverse to move from "0000" -> "0009".
```

**Example 3:**

```
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1

Explanation:
We can't reach the target without getting stuck.
```

**Example 4:**

```
Input: deadends = ["0000"], target = "8888"
Output: -1
```

**Note:**

1. The length of`deadends`will be in the range`[1, 500]`.
2. `target`will not be in the list`deadends`.
3. Every string in`deadends`and the string`target`will be a string of 4 digits from the 10,000 possibilities`'0000'`to`'99`

这题跟之前127 Word Ladder一样的解法，BFS，而且靠的是枚举邻居。T: O(N^2∗A^N+D)， S:O(A^N+D), N是长度，每次找邻居我们都找N^2个，例如，有4个数字，我们找到16个可能的邻居，每个数字能去两种，前一个/后一个，所以4^2。然后A是字母的个数？然后A^N来自make string ？D是那个loop把不能用的词放hashset里。

```java
public int openLock(String[] deadends, String target) {
    if (deadends == null || target == null || target.length() == 0) {
        return -1;
    }

    Set<String> set = new HashSet<>();
    for (String num : deadends) {
        set.add(num);
    }

    if (set.contains("0000")) { // 判断一下，输入有可能不合法
        return -1;
    }

    Set<String> visited = new HashSet<>();
    Queue<String> queue = new LinkedList<>();
    queue.offer("0000");
    visited.add("0000");
    int step = 0;

    while(!queue.isEmpty()) {
        step++;
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            String cur = queue.poll();

            if (cur.equals(target)) {
                return step - 1; // 因为我们连开始的点也数了一层， 所以减一
            }

            List<String> neis = findNei(set, cur);
            for (String nei : neis) {
                if (visited.contains(nei)) {
                    continue;
                }

                queue.offer(nei);
                visited.add(nei);
            }
        }
    }

    return -1;
}

private List<String> findNei(Set<String> set, String node) {
    List<String> res = new ArrayList<>();                
    char[] cs = node.toCharArray();

    // 枚举相邻的数字
    for (int i = 0; i < cs.length; i++) {                
        char ori = cs[i];
        int prev = (cs[i] - '0' + 9) % 10;
        int next = (cs[i] - '0' + 1) % 10;

        cs[i] = (char) (prev + '0');
        String prevStr = new String(cs);
        if (!set.contains(prevStr)) {
            res.add(prevStr);
        }
        cs[i] = (char) (next + '0');;
        String postStr = new String(cs);
        if (!set.contains(postStr)) {
            res.add(postStr);
        }
        cs[i] = ori;
    }

    return res;
}
```
