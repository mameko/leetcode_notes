# Rearrange chars in string such that no 2 adjacent are the same

如题，这题是上一题[358 Rearrange String k Distance Apart](358-rearrange-string-k-distance-apart.md)的简化版。在geeksforgeeks上有答案。在snapchat的onsite被考到，然后看uber phone interview 面经看到，geeksforgeeks放在amazon的tag里。看来还是写写好。

做法是：

1. 数每个char的频率
2. 把每个char按照频率放进堆里
3. 每次把频率最高的取出来，加到结果里，然后减一
4. 因为每次加的char都得不一样，所以加了一个last，表示上一次用过的char，如果freq还不为0我们加进堆里下次再用
5. 重复过程直到堆为空，我们检查一下所构成的字符串跟原字符串的大小是否相同，相同表示我们成功rearange了。

```java
    public String rearange(String str) {
        if (str == null || str.isEmpty()) {
            return str;
        }

        HashMap<Character, Integer> cnt = new HashMap<>();
        for (int i = 0; i < str.length(); i++) {
            char cur = str.charAt(i);
            if (cnt.containsKey(cur)) {
                cnt.put(cur, cnt.get(cur) + 1);
            } else {
                cnt.put(cur, 1);
            }
        }

        PriorityQueue<RevFreqNode> queue = new PriorityQueue<>(10, new Comparator<RevFreqNode>() {
            public int compare(RevFreqNode n1, RevFreqNode n2) {
                return n2.freq - n1.freq;
            }
        });

        for (Map.Entry<Character, Integer> entry : cnt.entrySet()) {
            queue.offer(new RevFreqNode(entry.getKey(), entry.getValue()));
        }

        RevFreqNode last = null;
        StringBuilder sb = new StringBuilder();
        while (!queue.isEmpty()) {
            RevFreqNode cur = queue.poll();
            sb.append(cur.val);
            cur.freq = cur.freq - 1;

            if (last != null && last.freq > 0) {
                queue.offer(last);
            }

            last = cur;
        }

        if (sb.length() != str.length()) {
            return "Not Possible";
        }

        return sb.toString();
    }

    public static void main(String[] args) {
        UberPhoneSnapOnsiteRearrangeChar sol = new UberPhoneSnapOnsiteRearrangeChar();
        String str1 = "aaabc";
        String str2 = "aaabb";
        String str3 = "aa";
        String str4 = "aaaabc";
        System.out.println(sol.rearange(str1));
        System.out.println(sol.rearange(str2));
        System.out.println(sol.rearange(str3));
        System.out.println(sol.rearange(str4));
    }


class RevFreqNode {
    Character val;
    int freq;

    public RevFreqNode(char v, int f) {
        val = v;
        freq = f;
    }
}
```
