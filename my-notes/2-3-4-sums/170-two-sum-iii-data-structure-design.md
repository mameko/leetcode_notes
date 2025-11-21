# 170 Two Sum III - Data structure design

Design and implement a TwoSum class. It should support the following operations:`add`and`find`.

`add`- Add the number to an internal data structure.\
`find`- Find if there exists any pair of numbers which sum is equal to the value.

For example,

```
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

这题思路跟2sum hashmap实现很像。每次add的时候，加到hashmap里。然后要找是否存在的时候，我们跟map里的元素（这个队列一直输入的元素）算个差。然后在map中找这个diff是否存在。因为要判断是否存在2个数相加=target，所以当我们找到的时候要判断是否同一个数重复去了，例如 3 + 3 = 6, 我们需要保证有两个3，而不是1个被重复取了。

```java
// 几年后再抄了抄九章模板，java8了
// <num, count>
Map<Integer, Integer> storage = new HashMap<>();

public void add(int number) {
    storage.put(number, storage.getOrDefault(number, 0) + 1);
}

public boolean find(int value) {
    for (Integer num : storage.keySet()) {
        int diff = value - num;
        int countRequired = (diff == num) ? 2 : 1;
        if (storage.getOrDefault(diff, 0) >= countRequired) {
            return true;
        }
    }

    return false;
}


public class TwoSum {
    HashMap<Integer, Integer> storage = new HashMap<>();

    // Add the number to an internal data structure.
    public void add(int number) {
        if (storage.containsKey(number)) {
            storage.put(number, storage.get(number) + 1);
        } else {
            storage.put(number, 1);
        }
        // Java 8 : hm.put(number, hm.getOrDefault(number, 0) + 1);
    }

    // Find if there exists any pair of numbers which sum is equal to the value.
    public boolean find(int value) {
        for (HashMap.Entry<Integer, Integer> elem : storage.entrySet()) {
            int diff = value - elem.getKey();
            if (storage.containsKey(diff)) {
                if (diff == elem.getKey() && storage.get(diff) > 1) {
                    return true;
                } else if (diff != elem.getKey()) {
                    return true;
                }
            }
        }

        return false;
    }
}
```
