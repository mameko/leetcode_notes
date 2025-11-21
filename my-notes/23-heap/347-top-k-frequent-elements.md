# 347 Top K Frequent Elements

Given a non-empty array of integers, return the **k** most frequent elements.

For example,\
Given`[1,1,1,2,2,3]`and k = 2, return`[1,2]`.

**Note:**

* You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
* Your algorithm's time complexity **must be** better than O(nlogn), where n is the array's size.

看完cc189，还有nlogk的解法。用小堆

```java
public List<Integer> topKFrequent(int[] nums, int k) {
    List<Integer> res = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return res;
    }

    // count freq
    Map<Integer, Integer> freq = new HashMap<>();
    for (Integer elem : nums) {
        if (freq.containsKey(elem)) {
            freq.put(elem, freq.get(elem) + 1);
        } else {
            freq.put(elem, 1);
        }
    }

    // use heap to get top k
    PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>(k, new Comparator<Map.Entry<Integer, Integer>>() {
        public int compare(Map.Entry<Integer, Integer> n1, Map.Entry<Integer, Integer> n2) {
            return n1.getValue().compareTo(n2.getValue());
        }
    });

    for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
            if (pq.size() < k) {
                pq.offer(entry);
            } else if (entry.getValue() > pq.peek().getValue()) { // } else {                
                pq.poll();                                        //     pq.offer(entry); 
                pq.offer(entry);                                  //     pq.poll();
            }                                                     // }                           
        }

        for (int i = 0; i < k; i++) {                // while (!pq.isEmpty()) {
            res.add(pq.poll().getKey());             //   res.add(pq.poll().getKey());
        }                                            // }
        Collections.reverse(res);                    // no need
        return res; 
}
```

nlogn，用大堆，看了答案才发现这题原来还可以bucket sort...O(n)...

```java
public List<Integer> topKFrequent(int[] nums, int k) {
    List<Integer> res = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return res;
    }

    // count freq
    Map<Integer, Integer> freq = new HashMap<>();
    for (Integer elem : nums) {
        if (freq.containsKey(elem)) {
            freq.put(elem, freq.get(elem) + 1);
        } else {
            freq.put(elem, 1);
        }
    }

    // use heap to get top k
    PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>(k, new Comparator<Map.Entry<Integer, Integer>>() {
        public int compare(Map.Entry<Integer, Integer> n1, Map.Entry<Integer, Integer> n2) {
            return n2.getValue().compareTo(n1.getValue());
        }
    });

    for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
        pq.offer(entry);
    }

    for (int i = 0; i < k; i++) {
        res.add(pq.poll().getKey());
    }

    return res;   
}
```

下面是discuss里大神手稿：因为n个数的freq最多也只是n，也就是全是同一个数。所以我们可以建立数目为n的bucekt。数好freq以后，我们把频率对应的数字加到对应的bucket里。最后从后往前遍历（因为后面的频率高，求top k），找够k个就返回。

```java
public List<Integer> topKFrequent(int[] nums, int k) {

    List<Integer>[] bucket = new List[nums.length + 1];
    Map<Integer, Integer> frequencyMap = new HashMap<Integer, Integer>();

    for (int n : nums) {
        frequencyMap.put(n, frequencyMap.getOrDefault(n, 0) + 1);
    }

    for (int key : frequencyMap.keySet()) {
        int frequency = frequencyMap.get(key);
        if (bucket[frequency] == null) {
            bucket[frequency] = new ArrayList<>();
        }
        bucket[frequency].add(key);
    }

    List<Integer> res = new ArrayList<>();

    for (int pos = bucket.length - 1; pos >= 0 && res.size() < k; pos--) {
        if (bucket[pos] != null) {
            res.addAll(bucket[pos]);
        }
    }
    return res;
}
```
