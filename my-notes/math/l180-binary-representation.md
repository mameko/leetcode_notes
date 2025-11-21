# L180 Binary Representation

## L180 Binary Representation

## Description

Given a (decimal -\_e.g.\_3.72) number that is passed in as a string, return the binary representation that is passed in as a string. If the fractional part of the number can not be represented accurately in binary with at most 32 characters, return`ERROR`.

## Example

For n =`"3.72"`, return`"ERROR"`.

For n =`"3.5"`, return`"11.1"`.

这是两年前一刷写的答案，完全不知道自己干嘛。嘛，2进制小数位后的转换：乘2取整

```java
public String binaryRepresentation(String n) {
    if(n==null||n.equals("")){
        return "ERROR";
    }

    String[] splitedStr = n.split("\\.");
    String frac = generateFrac(splitedStr[1]);
    if(frac.equals("ERROR")){
        return frac;
    }
    String inte = generateDec(splitedStr[0]);
    if(frac.equals("")){
        return inte;
    }
    return inte+"."+frac;
}

private String generateFrac(String fracStr){
    String one = "1";
    String zero = "0";
    double f = Double.parseDouble("0."+fracStr);
    double divisor = 1;
    StringBuilder sb = new StringBuilder();
    int i = 0;
    while (f != 0) {
        divisor = divisor/2;
        if(f > divisor){
           sb.append(one);
           f = f - divisor;
        }else if(f < divisor){
            sb.append(zero);
        }else{
            sb.append(one);
            break;
        }
        i++;
            if (i > 31) {
                break;
            }
    }

    return i > 31 ? "ERROR" : sb.toString();
}

private String generateDec(String DecStr){
    return Integer.toBinaryString(Integer.parseInt(DecStr));

}
```
