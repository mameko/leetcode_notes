# TinyURL

就基本写一下方便复习参考：

**Senario**就两个：

* 通过short查long
* 创建时通过long查short，如果已经创建过了，就返回创建好的short url

要跟面试官商量是否要long跟short一一对应，是否要删除很久没人用的short url。

（通常不建议删，short to long是一定要一一的，但long to short不一定。就像你可以创建很多个short url for google，但每个short url都得对着google.com，不然就查不出来了。）

然后得估算一下QPS：假如我们有100M日活

写：每人10天产生一条short url -- 100M X 0.1 / 86400 \~ 100, peak write \~ 300

读：每天每人读（点击）1条short url -- 100M X 1 / 86400 ～ 1k，peak read ～ 3k

存：每天产生的url数目 100M X 0.1 = 10M条，如果每条算100，一共1G，1T硬盘够用3年

（读多写少）

**Service**也是两个：

GET ： shorturls/shorturl (find long with short)

POST : shorturls/ with data"longurl" (create short url)

**Storage**:

因为就是简单的key value，不需要transaction，不需要丰富的query，所以用nosql就ok啦。

建议存两份，longToshort & shortTolong，因为这样可以方便sharding。

然后建议用random的方法生成shorturl，这样就不依赖于自增id，不用sql啦。

**Scale**：

* 因为读多写少，用cache优化。（cache也存两类，shortTolong，longToshort。然后我们可以通过cache来限制同一个longurl产生不同shorturl的数目，因为短时间内你再创建，就直接从cache里拿给你。）
* 利用地里位置信息来优化，例如域名是中国的，放中国的服务器，中国的cache，美国的放美国的。也可以通过用户的ip来知道他们的ip来自哪里。
* sharding的话，因为存两个表，所以分别shard就ok啦

**算法**：[535 Encode and Decode TinyURL](../../../my-notes/design/535-encode-and-decode-tinyurl.md)

这题还可以结合rate limiter/ data dog来考，例如，那个shorturl在什么时候被人点了多少次。按分钟，小时，天，月，年etc。[L215 Rate Limiter](../../../my-notes/design/l215-rate-limiter.md)， [359 Design Hit Counter](../../../my-notes/design/359-logger-rate-limiter.md)
