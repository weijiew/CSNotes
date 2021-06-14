
Reverse Connection : 反向链接。

> 例如 A 想要链接 B ，但是 A 把这个想法告诉 C ，C 转告 B ，此时由 B 主动连接 A 。而最初则是 A 想要主动连接 B 的。

计算机网络层数有多种划分，其中七层是最全的，最少的可以划分为四层。

Application,Transport,Network,Link.

Transport:提供了可信赖的数据交付功能，保证数据被接收。TCP 协议就在这一层。与之相应的是 UDP 协议，该协议不保证数据被可靠交付。

早期的网络是四层：

![figure-1](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.3xgo7qbrela0.png)

链路层（Link）由链接和路由组成，数据包包含了要传递的数据以及目的地。

IP 处于网络层（Network），将主机连接在了一起。

 IP 协议不能保证可信赖的数据交付功能，此功能在 IP 层之上由其他协议实现！

传输层中主要有 TCP 协议。

