# Amicable Pair less than n

**相亲数**（Amicable Pair），又称**亲和数**、**友爱数**、**友好数**，指两个正整数中，彼此的全部[约数](https://zh.wikipedia.org/wiki/%E7%B4%84%E6%95%B8)之和（本身除外）与另一方相等。[毕达哥拉斯](https://zh.wikipedia.org/wiki/%E6%AF%95%E8%BE%BE%E5%93%A5%E6%8B%89%E6%96%AF)曾说：“朋友是你灵魂的倩影，要像220与284一样亲密。”

例如220与284：

* 220的全部因数（除掉本身）相加是：1+2+4+5+10+11+20+22+44+55+110=284
* 284的全部因数（除掉本身）相加的和是：1+2+4+71+142=220

```java
private int sumFactors(int num) {
    int sum = 0;
    for (int i = 1; i <= num / 2; i++) {
        if (num % i == 0) {
            sum += i;
        }
    }
    return sum;
}

public List<List<Integer>> findAmicableNumbers(int n) {
    List<List<Integer>> res = new ArrayList<>();
    int[] arr = new int[n];

    for (int i = 2; i < n; i++) {
        arr[i] = sumFactors(i);
    }

    for (int i = 0; i < n; i++) {
        int j = arr[i];
        if (j < i && i == arr[j]) {
            res.add(Arrays.asList(i, j));
        }
    }
    return res;
}

public static void main(String[] args) {
    S_amicableNumberLessThanN sol = new S_amicableNumberLessThanN();
    int n = 1000;
    System.out.println(sol.findAmicableNumbers(n));
}
```
