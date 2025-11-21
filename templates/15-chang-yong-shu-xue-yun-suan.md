# 15 常用数学运算

## **数位分离，**&#x628A;k进制的数一位一位分出来：

%k 然后 /k， 例子：[L408 Add Binary](../my-notes/math/l408-add-binary.md)

## **进制转换**

* 将10进制转成k进制：

%k 然后 /k，例子：把十进制的6，转成2进制.

```java
int i = 0;
while(n != 0) {
    digit[i++] = n % k;
    n /= k;
} // 最后翻转就ok啦。
```

* 小数点后转2进制。例如：[L180 Binary Representation](../my-notes/math/l180-binary-representation.md)

乘2取整，例如：0.6

```
0.6 × 2 = 1.2 =》1
0.2 × 2 = 0.4 =》 0
0.4 × 2 = 0.8 =》 0
0.8 × 2 = 1.6 =》 1
0.6 × 2 = 1.2 =》1 ... //无限循环小数
```

## **关于mod**：

```
    c mod m = (a ⋅ b) mod m
    c mod m = [(a mod m) ⋅ (b mod m)] mod m 
    c^1230 mod m = (c ^ 123 % m)^10 % m
    
    A few distributive properties of modulo are as follows:
1. ( a + b ) % c = ( ( a % c ) + ( b % c ) ) % c
2. ( a * b ) % c = ( ( a % c ) * ( b % c ) ) % c
3. ( a – b ) % c = ( ( a % c ) - ( b % c ) ) % c ( see notes at bottom )
4. ( a / b ) % c NOT EQUAL TO ( ( a % c ) / ( b % c ) ) % c
So, modulo is distributive over +, * and - but not / . 
```

## Fisher-Yates shuffle ：

例子： [384 Shuffle an Array](../my-notes/math/384-shuffle-an-array.md)

```java
void shuffleArrayIteratively(int[] cards) {
    for (int i = 0; i < cards.length; i++) {
        int k = rand(0, i);// inclusive
        int temp = cards[k];
        cards[k] = cards[i];
        cards[i] = temp;
    }
}
```

## Reservoir sampling

```java
(*
  S has items to sample, R will contain the result
 *)
ReservoirSample(S[1..n], R[1..k])
  // fill the reservoir array
  for i = 1 to k
      R[i] := S[i]

  // replace elements with gradually decreasing probability
  for i = k+1 to n
    j := random(1, i)   // important: inclusive range
    if j <= k
        R[j] := S[i]
```

## 公约数/公倍数：

```java
// 迭代
private int gcd(int a, int b) {
    while (b != 0) {
        int tmp = b;
        b = a % b;
        a = tmp;
    }

    return a;
}

// 最小公倍数 = (a * b) / gcd(a, b)

// 递归
int gcd(int a, int b) {
    if (b == 0) {
        return a;
    } else {
        return gcd(b, a % b);
    }
}
```

## 一次性进位：

例子：[43 Multiply Strings](../my-notes/math/43-multiply-strings.md)

在乘法里，按照平常计算顺序的话，我们是每乘完一步就进一步位。但其实我们可以先全乘完，加好，最后一次性进位。其实加法也可以这样干。

## 哈希怎么算：

例子：[L128 Hash Function](../my-notes/others/l128-hash-function.md)
