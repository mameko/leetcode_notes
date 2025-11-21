# Bloom Filter + Count-min sketch

用来快速排出不存在的东西。

用hash函数，加一个bloom filter表，然后把key都hash一下，存表里。

例如：hash（”张三“）= 26，hash（”李四“） = 30，那么bloom filter表里，26和30会设成true。因为只存一个位数组所以比直接存hash值省位置

如果hash（”某key“）在表里的值是false，那么表示这个key不存在。如果是true的话，表示**有可能存在**。

通常会用多于一个hash来做bloom filter。例如用2个hash，存同一个位数组，这样可以减低误判率

bloom filter的精确度跟下面3个点有关：

1. 哈希函数个数，多一点准确点
2. 位数组的长度，长一点准确点，少点collide
3. 加入的字符串数目，少一点准确点，少点collide

下面是个栗子：（具体公式请参照wiki）

如果有15个hash，位数组大小为200w，加入的字符串数量是10w个的话，判断2000w个字符串，误判率会大概是3～4%

这个count-min sketch是用在NLP和数据流数数目的情况。

因为如果只用hashmap来数，通常需要花费O(N)的空间来存这些结果。这个sketch aim at sublinnear的空间存。

具体做法也是用多个hash function，但这次是用来数数。

例子：

例如我们有4个hash function，和一条数据流。我们会弄一个table来记录数字for each hash function

\[A, B, C, A, A, B, C...]，

譬如我们**h1(A) = 1,** h2(A) = 0, h3(A) = 2, h4(A) = 2，我们到下表里把对应的地方加1.

| h1 |   | 1 |   |   |
| -- | - | - | - | - |
| h2 | 1 |   |   |   |
| h3 |   |   | 1 |   |
| h4 |   |   | 1 |   |

譬如我们**h1(B) = 1**, h2(B) = 3, h3(B) = 1, h4(B) = 0，我们到下表里把对应的地方加1.

| h1 |   | 2 |   |   |
| -- | - | - | - | - |
| h2 | 1 |   |   | 1 |
| h3 |   | 1 | 1 |   |
| h4 | 1 |   | 1 |   |

我们以此类推，把后面的字母扔进hash function里算一算，然后加到表中对应的位置统计数目。

我们可以发现hash会用collision，例如上面的A和B就在h1里collide了。

因为是”min“，所以我们查询的时候，例如:查A的个数。我们把A扔到4个hash function里，得到了相应的格子和数目。我们把这些数目取min。除非所有都collide了，这个数目会返回准确的数字。

因为hash有collide所以越多的hash function对准确性越有帮助。

这个算法返回的数字只会多算，不会少算。

另外还有一个count-**mean**-min sketch的改进版。听说通过算每一行的mean值，然后把表里的格子减去这个值会提高准确性。
