# 692 Top K Frequent Words

Given a non-empty list of words, return thekmost frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

**Example 1:**

```
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]

Explanation:
 "i" and "love" are the two most frequent words.
 Note that "i" comes before "love" due to a lower alphabetical order.
```

**Example 2:**

```
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]

Explanation:
 "the", "is", "sunny" and "day" are the four most frequent words,
 with the number of occurrence being 4, 3, 2 and 1 respectively.
```

**Note:**

1. You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
2. Input words contain only lowercase letters.

**Follow up:**

1. Try to solve it in O(nlogk) time and O(n) extra space.

```java
public List<String> topKFrequent(String[] words, int k) {
    List<String> res = new LinkedList<>();
    if (words == null || words.length == 0 || k < 0) {
        return res;
    }

    // count words freq
    Map<String, Integer> freq = new HashMap<>();
    for (String word : words) {
        freq.put(word, freq.getOrDefault(word, 0) + 1);
    }

    // k most freq so use min heap
    PriorityQueue<Map.Entry<String, Integer>> pq = new PriorityQueue<>(k, new Comparator<Map.Entry<String, Integer>>() {
        public int compare(Map.Entry<String, Integer> e1, Map.Entry<String, Integer> e2) {
            if (e1.getValue() == e2.getValue()) {
                return e2.getKey().compareTo(e1.getKey());
            }
            return e1.getValue() - e2.getValue();
        }
    });

    for (Map.Entry<String, Integer> entry : freq.entrySet()) {
        pq.offer(entry);
        if (pq.size() > k) {
            pq.poll();
        }
    }

    while (!pq.isEmpty()) {
        res.add(0, pq.poll().getKey());
    }

    return res;
}
```
