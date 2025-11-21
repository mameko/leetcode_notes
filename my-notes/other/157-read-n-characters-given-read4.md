# 157 Read N Characters Given Read4

The API:`int read4(char *buf)`reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the`read4`API, implement the function`int read(char *buf, int n)`that readsncharacters from the file.

**Note:**\
The`read`function will only be called once for each test case.

这个题感觉题意非常不明了。看了答案才知道是想干嘛。总之很郁闷。

```java
/* The read4 API is defined in the parent class Reader4.
      int read4(char[] buf); */

public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    public int read(char[] buf, int n) {
        int byteRead = 0;
        boolean eof = false;
        char[] buffer = new char[4];

        while (byteRead < n && !eof) {
            int charCnt = read4(buffer);
            if (charCnt != 4) {
                eof = true;
            }

            int len = Math.min(charCnt, n - byteRead);

            for (int i = 0; i < len; i++) {
                buf[byteRead + i] = buffer[i];
            }
            byteRead += len;
        }

        return byteRead;
    }
}
```
