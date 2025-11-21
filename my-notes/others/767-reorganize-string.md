# 767 Reorganize String

Given a string`S`, check if the letters can be rearranged so that two characters that are adjacent to each other are not the same.

If possible, output any possible result. If not possible, return the empty string.

**Example 1:**

```
Input: S = "aab"
Output: "aba"
```

**Example 2:**

```
Input: S = "aaab"
Output: ""
```

**Note:**

* `S`will consist of lowercase letters and have length in range`[1, 500]`.

这题呢，做了两次还是没掌握精髓。知道是用pq，大概知道超过一半不行。然而，操作起来失误。例如大于一般不行应该用(len + 1) / 2, 而我写了len / 2 + 1。然后呢，知道pq里每次把频率最大的取出来，但不记得怎么避免某个特大的被取重复了。例如，aaabc,怎么避免两次取a。之前Rearrange chars in string such that no 2 adjacent are the same 的方法是用一个last来记录，然后下一round再把最大那个放回去。这里呢在leetcood的solution里看到另一种方法：每次取最大的两个出来，然后频率减一丢回去。T:O(NlogC), S:O(C)。主要看heap和hashmap里装啥，因为字母有限，所以把C看作1。

```java
public String reorganizeString(String s) {
    if (s == null || s.length() == 0) {
        return "";
    }

    int len = s.length();
    // count freq
    Map<Character, Integer> hm = new HashMap<>();
    for (int i = 0; i < len; i++) {
        hm.put(s.charAt(i), hm.getOrDefault(s.charAt(i), 0) + 1);
    }

    // put in maxheap
    PriorityQueue<Map.Entry<Character, Integer>> pq = new PriorityQueue<>(len, new Comparator<Map.Entry<Character, Integer>>(){
        public int compare(Map.Entry<Character, Integer> e1, Map.Entry<Character, Integer> e2) {                
            return e2.getValue() - e1.getValue();
        }
    });
    // 这里还能用lamda，逆天的写法，但不知道为毛慢了很多，之前13ms，现在70ms
    // PriorityQueue<Map.Entry<Character, Integer>> pq = new PriorityQueue<>((e1, e2) -> e2.getValue() - e1.getValue()); 

    for (Map.Entry<Character, Integer> entry : hm.entrySet()) {
        pq.offer(entry);
    }

    Map.Entry<Character, Integer> largest = pq.peek();
    if (largest.getValue() > (len + 1) / 2) {
        return "";
    }

    StringBuilder sb = new StringBuilder();
    while (pq.size() >= 2) {
        Map.Entry<Character, Integer> char1 = pq.poll();
        Map.Entry<Character, Integer> char2 = pq.poll();
        sb.append(char1.getKey());
        sb.append(char2.getKey());
        int left1 = char1.getValue() - 1;
        if (left1 > 0) {
            char1.setValue(left1);
            pq.offer(char1);
        }
        int left2 = char2.getValue() - 1;
        if (left2 > 0) {
            char2.setValue(left2);
            pq.offer(char2);
        }
    }

    while (!pq.isEmpty()) {
        sb.append(pq.poll().getKey());
    }
    return sb.toString();
}
```
