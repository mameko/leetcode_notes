# 19 Network stuff

* 连了vpn，走vpn的还是走public的。走vpn的
* 如果连了网线和wifi，packet走哪边？可config，好像是优先走LAN，LAN不行再走wifi，可以用ping来试一试
* 如果两边连了TCP，中间路由出事了，会怎么样？物理和Link layer会发现连不上，所以会丢包。然后TCP和上面的还是完全不知道，TCP会等Ack，发现没收到ACK就继续retry，直到试够了，才发现断了。如果在retry的期间路由突然好了，TCP还是会完全不知情地继续发。
* 经典问题：地址栏输入google.com以后发生什么：去dns server一级一级地上去，直到找到ip为止；然后拿着ip去找web server。（之前脑抽，答完dns就说找到网站了，囧）
