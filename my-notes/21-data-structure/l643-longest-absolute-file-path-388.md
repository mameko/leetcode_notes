# L643 Longest Absolute File Path (388)

Suppose we abstract our file system by a string in the following manner:

The string`"dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"`represents:

```
dir
    subdir1
    subdir2
        file.ext
```

The directory`dir`contains an empty sub-directory`subdir1`and a sub-directory`subdir2`containing a file`file.ext`.

The string`"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"`represents:

```
dir
    subdir1
        file1.ext
        subsubdir1
    subdir2
        subsubdir2
            file2.ext
```

The directory`dir`contains two sub-directories`subdir1`and`subdir2`.`subdir1`contains a file`file1.ext`and an empty second-level sub-directory`subsubdir1`.`subdir2`contains a second-level sub-directory`subsubdir2`containing a file`file2.ext`.

We are interested in finding the longest (number of characters) absolute path to a file within our file system. For example, in the second example above, the longest absolute path is`"dir/subdir2/subsubdir2/file2.ext"`, and its length is`32`(not including the double quotes).

Given a string representing the file system in the above format, return the length of the longest absolute path to file in the abstracted file system. If there is no file in the system, return`0`.

**Note:**

* The name of a file contains at least a`.`and an extension.
* The name of a directory or sub-directory will not contain a`.`.

Time complexity required:`O(n)`where`n`is the size of the input string.

Notice that`a/aa/aaa/file1.txt`is not the longest file path, if there is another path`aaaaaaaaaaaaaaaaaaaaa/sth.png`.

这题首先用split把每部分拆出来。然后要注意每一层跟stack size的关系。如果层数小于stack的size时，就pop。每次pop完，就push新的part。stack里为了节省计算时间，存的是prefix sum。prefix sum多开一位方便计算。所以一开始多push一个0.这里还巧妙得用到了lastIndexOf()。真心很久没写java，好虚。T:O(n), S:O(n)

```java
public int lengthLongestPath(String input) {
    // write your code here
    if (input == null || input.length() == 0) {
        return 0;
    }

    String[] parts = input.split("\n");
    Stack<Integer> stack = new Stack<>();
    // prefix sum，多开一位
    stack.push(0);
    int max = 0;
    for (String part : parts) {
        // 多加了一格，根目录会返回-1，所以+2，最后会等于第一层
        int level = part.lastIndexOf("\t") + 2;
        // 记得算好长度，减去那些\t，最震惊的是，一开始一位\t算2个，但发现其实算1个，所以不用X2
        int len = part.length() - (level - 1);
        while (stack.size() > level) {
            stack.pop();
        }
        stack.push(len + stack.peek());
        if (part.contains(".")) {
            // 只有碰到文件时才算一下长，因为每一层都会有分隔符，所以最后要加上分隔符的长度。
            // 3层会有2个分隔符(a/aa/aaa.txt)，所以level-1.
            max = Math.max(max, stack.peek() + level - 1);
        }
    }

    return max;
}
```

下面是九章的答案，其实我们可以用一个数组代替stack。

```java
public int lengthLongestPath(String input) {
    // Write your code here
    if (input.length() == 0) {
        return 0;
    }
    int ans = 0;
    int[] sum = new int[input.length() + 1];

    for (String line : input.split("\n")) {
        int level = line.lastIndexOf('\t') + 2;
        int len = line.length() - (level - 1);
        if (line.contains(".")) {
            ans = Math.max(ans, sum[level - 1] + len);
        } else {
            sum[level] = sum[level - 1] + len + 1;
        }
    }
    return ans;
}
```
