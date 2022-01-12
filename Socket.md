> ### TCP



![TCP分层](https://gitee.com/rainopen/images/raw/master/TCPLayer.webp)

[TCP协议详解](https://www.jianshu.com/p/ef892323e68f)

##### TCP连接三次握手

三次握手(Three-way handshake) 其实就是指建立一个TCP连接时，需要客户端与服务器间总共发送三个包，进行三次握手的主要作用是为了确认双方的接收能力和发送能力是否正常，指定自己的初始化序列号为后面的可靠性传送做准备。实质就是服务器指定端口，建立TCP连接，并同步连接双方的序列化和确认号，交换TCP信息。

刚开始客户端处于closed 状态，服务端处于listen 状态。此时进行三次握手：   

第一次握手：客户端向服务端发送一个SYN(synchronize ['sɪŋkrə.naɪz] 同步)报文，并指明客户端的初始化序列号ISN(initial sequence number  [ɪ'nɪʃ(ə)l] ['sikwəns]  ['nʌmbər])，此时客户端处于SYN_SENT状态。首部的同步位SYN=1，初始序号seq=x，SYN=1的报文不能携带数据，但要消耗掉一个序号。   

第二次握手：服务端收到客户端的SYN报文后，会以自己的SYN报文作为应答，并且也是指定了自己的初始化序列号ISN，同时把客户端的ISN+1作为ACK(acknowledge [ək'nɒlɪdʒ] 应答)的值，表示自己已经收到了客户端的SYN，此时服务器处于SYN_RCVD状态。在确认报文段中SYN=1，ACK=1，确认号ack=x+1，初始序号seq=y。

第三次握手：客户端收到SYN报文后，会发送一个ACK报文，当然，也是一样把服务器的ISN+1作为ACK的值，表示已经收到了服务端的SYN报文，此时客户端处于ESTABKISHED状态，服务器收到ACK报文后也处于ESTABLISHED状态，此时双方建立了连接。确认报文段ACK=1，确认号ack=y+1，序号seq=x+1(初始为seq=x，第二个报文段所以要+1)，ACK报文段可以携带数据，不携带数据则不消耗序号。

发送第一个SYN的一端将执行主动打开(active open)，接收这个SYN并发回下一个SYN的另一端被动打开(passive open)

在socket编程中，客户端执行connect()时，将触发三次握手。

[深究：三次握手和四次挥手](https://www.jianshu.com/p/64b172779647)   
[卧槽！牛皮了，面试官居然把TCP三次握手四次挥手问的这么详细](https://www.jianshu.com/p/9c8a07ee3f1e)   
[English TCP Three-way Handshake](https://www.sciencedirect.com/topics/computer-science/three-way-handshake)






* [WebSocket简介以及在Unity中使用WebSocket](https://www.pianshen.com/article/2904948666/)   
* [unity 使用WebSocket开发聊天(HeartBeat/TrimEndBytes)]()   
* [Unity使用WebSocket（非Best Http）结合UniRx，UniTask实现自动重连，同步更新UGUI(发送接收二进制，UniRx定时)](http://bbs.cskin.net/thread-326-1-1.html)
* [Socket 中粘包问题浅析及其解决方案](https://blog.csdn.net/qq_31967569/article/details/82894063)
* [Socket粘包问题的3种解决方案，最后一种最完美！](https://www.cnblogs.com/vipstone/p/14239160.html)
* [WebSocket断开原因、心跳机制防止自动断开连接](https://www.cnblogs.com/gxp69/p/11736749.html)
* [Unity Socket服务器（客户端）通用框架](https://www.jianshu.com/p/266acca9e21e)