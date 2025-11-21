# L530 Geohash II

This is a followup question for [Geohash](l529-geohash.md).

Convert a Geohash string to latitude and longitude. Try [http://geohash.co/](http://geohash.co/).

Check how to generate geohash on wiki:[Geohash](https://en.wikipedia.org/wiki/Geohash)or just google it for more details.

**Example**

Given`"wx4g0s"`, return lat =`39.92706299`and lng =`116.39465332`.

Return double\[2], first double is latitude and second double is longitude.

首先，把string换成binary串，换的时候要注意有些长度小于5的要补前缀0.然后拆成奇偶串来分经纬。偶是lagitude，奇是latitude。分好以后再二分地找所表示的经纬度。二分需要注意的是，最后return前还得分一次。

```java
String base = "0123456789bcdefghjkmnpqrstuvwxyz";
public double[] decode(String geohash) {
    double[] result = new double[2];
    if (geohash == null || geohash.length() == 0) {
        return result;
    }

    StringBuilder tmp = new StringBuilder();
    StringBuilder even = new StringBuilder();
    StringBuilder odd = new StringBuilder();

    for (int i = 0; i < geohash.length(); i++) {
        int loc = base.indexOf(geohash.charAt(i));
        String str = Integer.toBinaryString(loc);
        while (str.length() < 5) {
            str = "0" + str;
        }
        tmp.append(str);
    }

    for (int i = 0; i < tmp.length(); i++) {
        if (i % 2 == 0) {
            even.append(tmp.charAt(i));
        } else {
            odd.append(tmp.charAt(i));
        }
    }

    double lat = binToDouble(odd, -90, 90);
    double log = binToDouble(even, -180, 180);

    result[0] = lat;
    result[1] = log;

    return result;
}

private double binToDouble(StringBuilder nums, double start, double end) {
    for (int i = 0; i < nums.length(); i++) {
        char cur = nums.charAt(i);
        double mid = start + (end - start) / 2;
        if (cur == '1') {
            start = mid;
        } else {
            end = mid;
        }
    }

    return start + (end - start) / 2;
}
```
