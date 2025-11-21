# 179 Largest Number

Given a list of non negative integers, arrange them such that they form the largest number.

## Notice

The result may be very large, so you need to return a string instead of an integer.

**Example**

Given`[1, 20, 23, 4, 8]`, the largest formed number is`8423201`.

[**Challenge**](http://www.lintcode.com/en/problem/largest-number/#challenge)

Do it in O(nlogn) time complexity.

想了很久，原来能用排序。这...

```java
public String largestNumber(int[] num) {
    if(num==null||num.length==0){
        return "";
    }

    String[] tmp = new String[num.length];
    for(int i=0;i<num.length;i++){
        tmp[i] = String.valueOf(num[i]);
    }

    Arrays.sort(tmp, new Comparator<String>(){
        public int compare(String a, String b){
            return (b+a).compareTo(a+b);
        }
    });

    StringBuilder sb = new StringBuilder();
    for(String s: tmp){
        sb.append(s);
    }

    if(sb.charAt(0)=='0'){return "0";}
    return sb.toString();
}
```
