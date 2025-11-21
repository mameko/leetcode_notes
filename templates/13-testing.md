# 13 Testing

**自己惯用测试用例：**

Boundary： size = 0， 1， 2 & very big

robustness ：null input, or \[null]

functional : positive test case & negative test case

* 也得注意特殊输入，例如：一个数组\[0， 0， 0]
* 除以0
* 加和时注意越界，例如：\[1,2147483647]
* 如果要数字符数目的时候，要注意刚好相等的情况，例如：\["a"], 1, 68 Text Justification
* 虽然输入只包含貌似合法的字符，数字跟点（.），但要注意00，01这些非法输入。例如：[165 Compare Version](../my-notes/other/165-compare-version-numbers.md)

**下面的从stackoverflow上抄下来的：**

Based on the content of the algorithm you can identify what data structures/types/constructs are used. Then, you try to understand the (possible) weak points of those and try to come up with an execution plan that will make it run in those cases.

For example, the algorithm takes a string and an integer as input and does some sorting of the characters of the string.

Here we have:

**String with some known special cases:**

```
Empty string

Long string

Unicode string \(special characters\)

If limited to a specific set of characters, what happens when some are not in the range

Odd/even length string

Null \(as argument\)

Non-null terminated
```

**Integer with known special cases:**

```
0

Min/MaxInt

Negative/Positive
```

**Sort algorithm that could fail in the following boundary cases:**

```
Empty input

1 element input

Very long input \(maybe of length max\(data type used for index\)\)

Garbage inside the collection that will be sorted

Null input

Duplicate elements

Collection with all elements equal

Odd/even length input
```

**Then, take all these cases and create a long list trying to understand how they overlap. Ex:**

```
Empty string case covers the empty collection case

Null string == null collection
```

etc.

Now create test cases for them :)

Short summary: break the algorithm in basic blocks for which you know the boundary cases and then reassemble them, creating global boundary cases
