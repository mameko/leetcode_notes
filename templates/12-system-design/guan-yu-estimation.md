# 关于estimation

除了read/write qps，storage size，还得估算bandwidth和TCP connection count什么的。

一个机构需要多种多样的server，最minimual的有web，app，和storage。storage又分为blob storage，s3之类的，放vedio。bigtable之类的，放thumbnails等等。sql放metadata，user info之类的。non-sql。hdfs用来弄anaytics。

其他还有，config server呀，monitoring的server呀，Prometheus什么的，还有AAA呀，cache呀，elastic search之类之类的，都得另外的server。

关于速度的一些基本number：

* L1，2，3 cache，nanosecond
* Main Ram，100，nanosecond
* read 1MB sequentially from RAM，9 microsecond
* read 1MB sequentially from SSD，200 microseconds
* Roundtrip within same data center，500 microsecond
* Disk seek，4 millisecond
* read 1MB sequentially from disk，2 millisecond
* Send packet from SF to NYC 71 millisecond

