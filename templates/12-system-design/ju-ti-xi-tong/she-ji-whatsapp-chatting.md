# 设计whatsapp chatting

关于http，因为chatting system里，server要找client，然后http只能是从client发过去server，所以有三种方法keep in touch。

1. websocket，bidirectional两边都可以发起通信（有sticky session功能，可以自动找到之前是那个server服务这个client的）
2. Bosh，bidirectional stream over synchronous http。就是client发一个request，server拿住request一段时间，如果这段时间有人发消息给这个client的话就在resonance里返回那些消息，如果没有就返回空。当client接到这个以后，再发起另外一个requst。
3. 另外一种是polling，就像以前做的web chat那样，隔一段时间去问一下server有没有人找我。

这里选websocket，然后在redis里创建一个session table记录那个user去哪个server，然后heart beat最后什么时间收到。（这个表能做“显示用户什么时候在线”这个功能）

真正的message存在Cassandra里，如果用户在线，那么在server端写完DB以后，会直接从session table找出谁在跟to\_client连着，然后把消息发过去。如果用户不再先，写DB的时候，加上一个unread==true。然后用户上线时会去DB里读在离线的时候收到什么message。

conversation table （noSQL）

| conversation\_id | unique id for the conversation                                  |
| ---------------- | --------------------------------------------------------------- |
| time stamp       | 什么时候send的                                                       |
| from\_user       | 那个user send的                                                    |
| content          | 这个可以是text或者url，url是在传图片的时候用。通常图片会存在blob storage，我们只记录url的地址就ok了 |
