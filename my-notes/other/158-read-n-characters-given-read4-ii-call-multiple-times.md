# 158 Read N Characters Given Read4 II - Call multiple times

The API:`int read4(char *buf)`reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the`read4`API, implement the function`int read(char *buf, int n)`that readsncharacters from the file.

**Note:**\
The`read`function may be called multiple times.

这题很囧，一开始想得很复杂，用很多变量，然后没想通。看了答案，发现人家写得很简单。看了一遍，理解了，然后自己写了。然后写完发现，比之前看的答案还简单，真的有点怀疑自己。嘛，贴上来有待以后研究。基本做法：一开始把初始状态设置成读完buffer时那样，然后进入循环。循环时，每次复制一个字符到结果的buf里，如果发现现在已经满了，就去读4个出来把buffer填满。要注意，每次填了以后，要测一下文件是否已经读完了，读完了的话，我们跳出循环，返回下标。（这是下标可能比n小）

```java
public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */

    int readOnce = 4;
    int bufferInd = 4;
    char[] buffer = new char[4];

    public int read(char[] buf, int n) {
        if (n < 0) {
            return 0;
        }

        int ind = 0;
        while (ind < n) {
            if (bufferInd >= readOnce) {
                readOnce = read4(buffer);
                bufferInd = 0;
            }

            if (readOnce == 0) {
                break;
            }

            buf[ind++] = buffer[bufferInd++];
        }

        return ind;

    }
}
```

下面是九章老师提点后的做法。比较容易理解，基本就是用大循环读request#ofchars，在读的时候，用一个storage把多余的存起来，下次call的时候可以继续读。在storage空的时候再调用read4去多读点进来。这时要注意我们还有没有能读的内容，没有立刻返回，不然会死循环。

```java
/**
 * @param buf destination buffer
 * @param n maximum number of characters to read
 * @return the number of characters read
 */
char[] storage = new char[4];
int head = 0;
int tail = 0;
public int read(char[] buf, int n) {
    int i = 0;
    // read until we get all the requested chars
    while (i < n) {
        // read till empty
        if (head >= tail) {
            // then use read4 to read more and store in storage
            int r = read4(storage);
            // reset pointer to right position
            head = 0;
            tail = r;
            // 注意要看我们还有没有内容，没有就立刻返回。
            if (r == 0) {
                break;
            }
        } else {
            // if we have things in storage, we keep reading
            buf[i++] = storage[head++];
        }
    }

    return i;
}
```
