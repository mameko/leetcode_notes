# L399 Nuts & bolts Problem

## L399 Nuts & bolts Problem

## Description

Given a set of\_n\_nuts of different sizes and\_n\_bolts of different sizes. There is a one-one mapping between nuts and bolts. Comparison of a nut to another nut or a bolt to another bolt is not allowed. It means nut can only be compared with bolt and bolt can only be compared with nut to see which one is bigger/smaller.

We will give you a compare function to compare nut with bolt.

## Example

Given nuts =`['ab','bc','dd','gg']`, bolts =`['AB','GG', 'DD', 'BC']`.

Your code should find the matching bolts and nuts.

one of the possible return:

nuts =`['ab','bc','dd','gg']`, bolts =`['AB','BC','DD','GG']`.

we will tell you the match compare function. If we give you another compare function.

the possible return is the following:

nuts =`['ab','bc','dd','gg']`, bolts =`['BC','AA','DD','GG']`.

So you must use the compare function that we give to do the sorting.

`The order of the nuts or bolts does not matter. You just need to find the matching bolt for each nut.`

感觉这题强行不能排序？而且我当时还没用模板。郁闷，就抄下来，有空研究。

解法一：brute force

```java
public void sortNutsAndBolts(String[] nuts, String[] bolts, NBComparator compare) {
    if (nuts == null || nuts.length != bolts.length || bolts == null) {
        return;
    }

    int len = nuts.length;
    for (int i = 0; i < len; i++) {
        for (int j = i; j < len; j++) {
            if (compare.cmp(nuts[i], bolts[j]) == 0) {
                String tmp = bolts[i];
                bolts[i] = bolts[j];
                bolts[j] = tmp;
            }
        }
    }    
}
```

解法二：快排

本质是分治的做法

```java
public void sortNutsAndBolts(String[] nuts, String[] bolts, NBComparator compare) {
    if (nuts == null || nuts.length != bolts.length || bolts == null) {
        return;
    }

    match(nuts, bolts, compare, 0, nuts.length - 1);
}
    private void match(String[] nuts, String[] bolts, 
                       NBComparator compare, int s, int e) {
    if (s >= e) {
        return;
    }

    int pivotLoc = partition(nuts, s, e, bolts[e], compare);
    partition(bolts, s, e, nuts[pivotLoc], compare);

    match(nuts, bolts, compare, s, pivotLoc - 1);
    match(nuts, bolts, compare, pivotLoc + 1, e);
}

private int partition(String[] A, int s, int e, 
                      String pivotVal, NBComparator compare) {
    int i = s;
    for (int j = s; j < e; j++) {
        if (compare.cmp(A[j], pivotVal) == -1 || 
            compare.cmp(pivotVal, A[j]) == 1) {
            String tmp = A[i];
            A[i] = A[j];
            A[j] = tmp;
            i++;
        } else if (compare.cmp(pivotVal, A[j]) == 0 || 
                   compare.cmp(A[j], pivotVal) == 0) {
            String tmp = A[j];
            A[j] = A[e];
            A[e] = tmp;
            j--;
        }
    }

    String tmp = A[i];
    A[i] = A[e];
    A[e] = tmp;

    return i;
}
```
