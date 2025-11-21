# L529 GeoHash

Geohash is a hash function that convert a location coordinate pair into a base32 string.

Check how to generate geohash on wiki:[Geohash](https://en.wikipedia.org/wiki/Geohash)or just google it for more details.Try[http://geohash.co/](http://geohash.co/).

You task is converting a (_latitude_,_longitude_) pair into a geohash string.

**Example**

Given lat =`39.92816697`, lng =`116.38954991`and precision =`12`which indicate how long the hash string should be. You should return`wx4g0s8q3jf9`.

The precision is between 1 \~ 12.

长的那个叫经度（-180， 180），记法，**long**itude，**长**的那个。然后做法是先经后纬，经纬交替。base是去掉a,i,l,o的数字+小写字母。要注意的是substring这玩意各种傲娇，明明substring的时候不包后面的下标，但你截最后一段的时候，只能截到string的长度。总之麻烦就是了。

```java
String base = "0123456789bcdefghjkmnpqrstuvwxyz";
public String encode(double latitude, double longitude, int precision) {
    //Latitudes range from -90 to 90.
    //Longitudes range from -180 to 180.
    // 先经后纬，经纬交替, first longitude then latitude
    // Base32：0-9, a-z 去掉 (a,i,l,o)

    if (latitude > 90.0 || latitude < -90.0 || longitude > 180.0 || longitude < -180.0 
        || precision > 12 || precision < 1) {
        return "";
    }

    int digits = precision * 5 / 2;// 5位为一个字母，因为要交替，所以经纬各要一半
    // get binary digits
    StringBuilder even = getString(longitude, -180, 180, digits);
    StringBuilder odd = getString(latitude, -90, 90, digits);
    // weave them together
    StringBuilder binary = new StringBuilder();
    for (int i = 0; i < digits; i++) {
        binary.append(even.charAt(i));
        binary.append(odd.charAt(i));
    }

    // turn binary digits to char
    StringBuilder res = convertBinToChar(binary);
    return res.toString();
}

private StringBuilder getString(double target, double start, double end, int len) {
    StringBuilder res = new StringBuilder();

    while (res.length() < len) {
        double mid = start + (end - start) / 2;
        if (target > mid) {
            res.append(1);
            start = mid;
        } else {
            res.append(0);
            end = mid;
        }
    }

    return res;
}

private StringBuilder convertBinToChar(StringBuilder bin) {
    StringBuilder res = new StringBuilder();
    for (int i = 0; i < bin.length(); i = i + 5) {
        int end = i + 5 >= bin.length() ? bin.length() : i + 5;// 注意substring的range
        String str = bin.substring(i, end).toString();
        int charLoc = Integer.parseInt(str, 2);
        res.append(base.charAt(charLoc));
    }
    return res;
}
```
