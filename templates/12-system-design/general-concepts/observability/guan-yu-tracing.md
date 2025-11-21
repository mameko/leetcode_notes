# 关于tracing

这个被人在面试问到，google了一堆还是没太太懂。

大概是分为几种东西，一个是Trace，就是从request进入系统到出去这是一个trace。然后每次系统里面经过一个模块的时间用span来存。然后还在call的时候建立trace tree。每个span要存自己的parent是哪个span，自己是那个span，trace id是什么。trace\_id是全局唯一用来表示这个请求的id。然后还有要注意的是，因为如果所有request都记录的话会花很多空间存储这些log，所以很多时候都会sample着存。

spring slueth + zipkin好像挺火的。另外淘宝，京东什么的也有自己一套。

听说这个trace\_id，如果是http（restful）的那种请求的话，会存在header里。如果是rpc或多线程的话，会存在threadlocal里。

不明觉厉，所以贴两个连接有空好好研究：

[https://www.ibm.com/developerworks/cn/web/wa-distributed-systems-request-tracing/index.html](https://www.ibm.com/developerworks/cn/web/wa-distributed-systems-request-tracing/index.html)

[http://www.cnblogs.com/zhengyun\_ustc/p/55solution2.html](http://www.cnblogs.com/zhengyun_ustc/p/55solution2.html)\
\
2022 - 再补一个link，搞了个Jaeger也是干这个的。[https://www.jaegertracing.io/docs/1.26/architecture/](https://www.jaegertracing.io/docs/1.26/architecture/)

现在k8s里有好多这种收集metric的东东。<br>
