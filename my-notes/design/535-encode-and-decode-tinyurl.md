# 535 Encode and Decode TinyURL

TinyURL is a URL shortening service where you enter a URL such as`https://leetcode.com/problems/design-tinyurl`and it returns a short URL such as`http://tinyurl.com/4e9iAk`.

Design the`encode`and`decode`methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

这个设计题，具体还是看笔记吧。不过呢，有两种encode/decode的选择，1，用auto increase id，存sql。这样这题就变成十进制id和62进制的tinyurl转换。2，随机生成tinyurl，用两个hashmap来存long To short和short to long。在leetcode上显示方法二快一点。

方法一：

```java
public class Codec {
    Map<String, String> sTol = new HashMap<>();
    Map<String, String> lTos = new HashMap<>();
    Random r = new Random();
    String base = "0123456789abcdefghijklmnopqrestuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        if (lTos.containsKey(longUrl)) {
            return lTos.get(longUrl);
        }
        StringBuilder sb;
        do {
            sb = new StringBuilder();
            for (int i = 0; i < 6; i++) {
                char c = base.charAt(r.nextInt(62));
                sb.append(c);
            }
        } while (sTol.containsKey(sb.toString()));

        String shortUrl = sb.insert(0, "http://tinyurl.com/").toString();
        sTol.put(shortUrl, longUrl);
        lTos.put(longUrl, shortUrl);

        return shortUrl;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return sTol.get(shortUrl);
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

方法二：

```java
public class Codec {
    List<String> storage = new ArrayList<>();
    String base = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        storage.add(longUrl);
        long res = (long)storage.size() - 1;
        StringBuilder sb = new StringBuilder();
        while (res > 0) {
            int dig = (int)res % 62;
            char c = base.charAt(dig);
            sb.insert(0, c);
            res = res / 62;
        }

        sb.insert(0, "http://tinyurl.com/");
        return sb.toString();
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        String idString = shortUrl.substring(19);// remove "http://tinyurl.com/"
        long res = 0L;
        for (int i = 0; i < idString.length(); i++) {
            res = res * 62 + base.indexOf(idString.charAt(i));
        }
        return storage.get((int)res);
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```
