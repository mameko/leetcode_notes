# L471 Top K Frequent Words

Given a list of words and an integer k, return the top k frequent words in the list.

## Notice

You should order the words by the frequency of them in the return list, the most frequent one comes first. If two words has the same frequency, the one with lower alphabetical order come first.

**Example**

Given

```
[
    "yes", "lint", "code",
    "yes", "code", "baby",
    "you", "baby", "chrome",
    "safari", "lint", "code",
    "body", "lint", "code"
]
```

for k =`3`, return`["code", "lint", "baby"]`.

for k =`4`, return`["code", "lint", "baby", "yes"]`,

[**Challenge**](http://www.lintcode.com/en/problem/top-k-frequent-words/#challenge)

Do it in O(nlogk) time and O(n) extra space.

Extra points if you can do it in O(n) time with O(k) extra space approximation algorithms.

这题做法是先把所有的词数一次次数，然后全部仍进heap里，从里面取k个。这里排序时用了反序，所以poll k个出来就ok了。

```java
    public String[] topKFrequentWords(String[] words, int k) {
        if (words == null || words.length == 0 || k < 1) {
            return new String[0];
        }

        HashMap<String, Integer> count = new HashMap<>();
        for (String s : words) {
            if (count.containsKey(s)) {
                count.put(s, count.get(s) + 1);
            } else {
                count.put(s, 1);
            }
        }

        PriorityQueue<HeapNode> heap = new PriorityQueue<>();
        for (Map.Entry<String, Integer> e : count.entrySet()) {
            heap.offer(new HeapNode(e.getKey(), e.getValue()));
        }

        String[] res = new String[k];
        for (int i = 0; i < k; i++) {
            res[i] = heap.poll().word;
        }
        return res;
    }


class HeapNode implements Comparable<HeapNode> {
    String word;
    int freq;

    public HeapNode(String w, int f) {
        word = w;
        freq = f;
    }

    @Override
    public int compareTo(HeapNode other) {
        if (other.freq == freq) {
           return word.compareTo(other.word);
        }
        return other.freq - freq;
    }
}
```
