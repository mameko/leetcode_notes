# Weighted ramdom sampling

给一个hashmap里面存了国家对应的频率，求随机返回的时候按概率返回。例如：china 1，us 2， canada 3，要以1/6的机率返回中国，1/3返回美国，1/2返回加拿大。

我的想法需要用O(n)时间和O(sum)空间。把字符从map里按个数倒进arraylist里，然后再random一下。听说能二分。先生成这样的list：china 1，us 3， canada 6。感觉得排序然后再前续和那些frequency。然后生成random数字，看数字落在哪段区间，落在的那个区间就返回那个国家名字。时间是O(nlogn)空间是O(n)，空间上优化了点。因为sum很可能大于n

```java
Random r = new Random();

public String randomContry1(Map<String, Integer> hm) {
    if (hm == null || hm.isEmpty()) {
        return "";
    }

    List<String> strs = new ArrayList<>();
    for (Map.Entry<String, Integer> entry : hm.entrySet()) {
        int times = entry.getValue();
        while (times > 0) {
            strs.add(entry.getKey());
            times--;
        }
    }

    return strs.get(r.nextInt(strs.size()));
}

public static void main(String[] args) {
    WeightedRandomSampling sol = new WeightedRandomSampling();
    Map<String, Integer> hm = new HashMap<>();
    hm.put("china", 1);
    hm.put("us", 2);
    hm.put("canada", 3);

    HashMap<String, Integer> hmback = new HashMap<>();
    for (int i = 0; i < 100; i++) {
        String cur = sol.randomContry1(hm);
        if (hmback.containsKey(cur)) {
            hmback.put(cur, hmback.get(cur) + 1);
        } else {
            hmback.put(cur, 1);
        }
    }

    for (Map.Entry<String, Integer> entry : hmback.entrySet()) {
        System.out.println(entry.getKey() + " : " + entry.getValue() / 100.0);
    }
}
```
