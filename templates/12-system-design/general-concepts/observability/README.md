# Observability

**听说这个有3个pillar，traces, logs and metrics**

[https://www.youtube.com/watch?v=lILY8eSspEo](https://www.youtube.com/watch?v=lILY8eSspEo)

![](<../../../../.gitbook/assets/image (18).png>)

好像在k8s里好像可以收集一堆metrics然后放到Prometheus（time series db），然后grafana来visualize和analyse，起码我们家的teamcenter有用到这些。其实prometheus也有个类似dashboard的东西。grafana主要是看metric和log

**Prometheus**： [https://www.youtube.com/watch?v=h4Sl21AKiDg](https://www.youtube.com/watch?v=h4Sl21AKiDg)

关于prometheus呢，其实有几个部分。它自己是一个server，然后呢，有两种model，push or pull。主要是pull。就是每个host node会有一个agent那样的东西，or你可以deploy一个sidecar在你要monitor的东西的隔壁。隔一段时间呢，Prometheus就会去fetch一下。如果是cron job或者short term的process，可能等不到fetch就run完收工了。所以会有一个push gatway的东东，帮忙存这这些data，等到Prometheus要pull的时候，report back to mothership。

现在好像比较流行那个**opentelemetry**的标准，听说好像现在没有那么多log方面的integration。另外两方面做得挺不错的。

关于**logs**的东东，我们公司用了ELK stack，基本上有个agent样的东西在host那里。然后我们docker compose那里写了parameter，那个agent呢就会grap我们的log，发给elastic search，然后我们就可以在kibana上查找关于我们的log。

