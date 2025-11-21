# 9 Bit Manipulation 常用公式

数1的数目：L365 Count 1 in Binary

```java
int num = ...// count 1's in this number
int count = 0;
for (int c = num; c != 0; c = c & (c - 1)) {
    count++;
}
```

Set Union:

```
A | B
```

Set intersection:

```
A & B
```

Set Subtraction:

```
A & ~B
```

Set negation:

```
All_Bits1 ^ A
```

Set bit

```
A |= 1 << bit
```

Clear bit

```
A & ~ (1 << bit)
```

Test bit

```
(A & 1 << bit) != 0
```

Find right most bit = 1

```
x & ~(x - 1) // 如果是java的话，可以用num & -num,因为负一下会是二补码
```

Clear the least bit = 1

```
c & (c - 1)
```

check if num is 2's power or num == 0

```
n & (n - 1) == 0
```

Rotate right shift:

```
bits >>> k | bits >>> (Integer.size - k);
```

Rotate left shift is similar:

```
bits << k | bits >>> (Integer.size - k);
```

Swap bits in integer

```
x = ((x & Oxaaaaaaaa) >> 1) | ((x & Ox5555555) << 1)
```

| x ^ 0s = x   | x & 0s = 0 | x \| 0s = x  |
| ------------ | ---------- | ------------ |
| x ^ 1s = \~x | x & 1s = x | x \| 1s = 1s |
| x ^ x = 0    | x & x = x  | x \| x = x   |

swap number with xor

```
    int x = 10, y = 5; 
    // Code to swap 'x' (1010) and 'y' (0101) 
    x = x ^ y; // x now becomes 15 (1111) 
    y = x ^ y; // y becomes 10 (1010) 
    x = x ^ y; // x becomes 5 (0101)
```

swap number with math

```
    int x = 10, y = 5; 

    // Code to swap 'x' and 'y' 
    x = x + y; // x now becomes 15 
    y = x - y; // y becomes 10 
    x = x - y; // x becomes 5
```
