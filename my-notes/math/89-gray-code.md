# 89 Gray Code

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integernrepresenting the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, givenn= 2, return`[0,1,3,2]`. Its gray code sequence is:

```
00 - 0
01 - 1
11 - 3
10 - 2
```

**Note:**\
For a givenn, a gray code sequence is not uniquely defined.

For example,`[0,2,3,1]`is also a valid gray code sequence according to the above definition.

For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

这题本来我用的是镜像的解法，无奈，那个解法生成的结果不是OJ的。然后看到discuss里大神用3行写完了。利用的是G(i) = i ^ (i / 2)这条性质。

```java
public List<Integer> grayCode(int n) {
    List<Integer> res = new ArrayList<>();
    if (n < 0) {
        return res;
    } else if (n == 0) {
        res.add(0);
        return res;
    } else if (n == 1) {
        res.add(0);
        res.add(1);
        return res;
    }

    List<String> resMid = new ArrayList<>();
    resMid.add("0");
    resMid.add("1");

    for (int i = 2; i <= n; i++) {
        ArrayList<String> tmp = new ArrayList<>(resMid);
        Collections.reverse(tmp);

        for (int j = 0; j < tmp.size(); j++) {
            String cur = tmp.get(j);
            cur = "0" + cur;
            tmp.set(j, cur);
        }

        for (int j = 0; j < resMid.size(); j++) {
            String cur = resMid.get(j);
            cur = "1" + cur;
            resMid.set(j, cur);
        }

        resMid.addAll(tmp);
    }

    for (String num : resMid) {
        res.add(Integer.parseInt(num, 2));
    }

    return res;
}
```

```java
public List<Integer> grayCode(int n) {
   List<Integer> result = new LinkedList<>();
    for (int i = 0; i < 1<<n; i++) result.add(i ^ i>>1);
    return result;
}
```
