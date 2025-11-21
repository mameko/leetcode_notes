# Service Mesh

istio其实是control plane的，然后每个envoy side car负责执行任务。譬如，你想收集metrics，你想terminate tls，都是那个side car在做。都是通过yaml来控制的，你的business logic不用加code。

好像主要是用来handle distributed system里service之间的security。譬如能管理service之间的authn，authz。

[https://learn.hashicorp.com/tutorials/consul/service-mesh\
https://dzone.com/articles/service-mesh-101-the-role-of-envoy](https://learn.hashicorp.com/tutorials/consul/service-meshhttps://dzone.com/articles/service-mesh-101-the-role-of-envoy)

[https://dzone.com/articles/what-is-a-service-mesh-nginx\
https://dzone.com/articles/what-does-a-service-mesh-do](https://dzone.com/articles/what-is-a-service-mesh-nginxhttps://dzone.com/articles/what-does-a-service-mesh-do)

还有就是，本来k8s里就有提供相关的功能，这个service mesh只是在k8s上再加一层，让你更加方便使用。而且这个功能更强大



[<br>](https://learn.hashicorp.com/tutorials/consul/service-meshhttps://dzone.com/articles/service-mesh-101-the-role-of-envoy)
