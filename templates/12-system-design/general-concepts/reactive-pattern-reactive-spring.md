# Reactive pattern（Reactive Spring）

这个是之前因为Azure AD connector container有performance issue。我们只能开200个thread。所以利用这个来utilize少点的resources。[https://subscription.packtpub.com/book/web-development/9781783287314/1/ch01lvl1sec09/the-reactor-pattern](https://subscription.packtpub.com/book/web-development/9781783287314/1/ch01lvl1sec09/the-reactor-pattern)

我们要发请求到azure ad那边，每个request需要起码5s的response time。如果我们用的是spring mvc，那么正常的流程，每个request，servlet给我们一个thread，但这个thread会一直wait for network IO回来。但如果用了这个reactor pattern，我们这个thread就可以release back to pool，等网络I/O回来之后，再找一个thread去handle。这样，同样的thread number，会被更大幅度地利用起来。

具体解析，怎么从spring mvc过度到reactive spring。其实spring mvc也能做，就是不intuitive，所以同事stuck了半天。

[https://dzone.com/articles/understanding-spring-reactiveintroducing-spring-we](https://dzone.com/articles/understanding-spring-reactiveintroducing-spring-we)
