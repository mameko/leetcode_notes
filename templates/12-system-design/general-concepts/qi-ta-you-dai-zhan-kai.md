# 其他有待展开

proxy和reverse proxy和loadbalancer：[https://www.youtube.com/watch?v=AuINJdBPf8I](https://www.youtube.com/watch?v=AuINJdBPf8I)

* proxy就是你是client，去访问server的时候，的man in the middle。企业用来filter你的request譬如上班的时候你去瑟瑟的网站就会被打回。
* reverse proxy就是如果你有一堆server可以服务client。那么client请求过来，会有个reverse proxy把那个request分给server们or replica们
* 这个是不是很像load balancer呢。听说load balancer就是reverse proxy的一个用法
* 另外这里解释了更多load balancer和reverse proxy的不同：
  * [https://www.nginx.com/resources/glossary/reverse-proxy-vs-load-balancer/](https://www.nginx.com/resources/glossary/reverse-proxy-vs-load-balancer/)
  * 具体大概是，reverse proxy也可以用于只有一个server的情况，可以左compression，ssl termination，cacheing response之类的
  * load balancer主要是用来balance load的，而且这个还能提供session persistence。就是同一个client可以访问同一个server -- 当然人们也可以选择用distributed cache和db来存这个信息，譬如，session table什么的

API gateway：[https://www.youtube.com/watch?v=1vjOv\_f9L8I](https://www.youtube.com/watch?v=1vjOv_f9L8I)

* 就是fascade设计模式。如果你有一堆apis，caller不用太了解具体有什么。不然，要refactor或者改动，caller也得改。定好了external api之后，就ok了
* 另外，这个还能左monitoring和security相关的task，因为大家都得经过这个gateway来访问你的service们。
* 有很多open source implementation，而且AWS那些云provider也有提供这种服务
* 通常，为了可靠，你会有一堆这些gateway，然后前面再来一个/堆load balancer分配任务
* 有一种叫backend for frontend pattern的东东，就是，如果你有web client，mobile client，你把api gateway也分成两类
