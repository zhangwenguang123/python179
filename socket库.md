# **socket模块**

## socket类

```
参数：
	family:地址簇（用于确定使用的通信协议）
			AF_UNIX:unix本机之间进行通信
			AF_INET:使用IPv4
			AF_INET6:使用IPv6
	type:套接字类型
		SOCK_STREAM：TCP套接字
		SOCK_DGRAM：UDP套接字类型
		其余根据不同电脑，还有一些类型，但一般以上两种都支持
		如：SOCK_RAW ：原始套接字类型，这个套接字比较强大,创建这种套接字可以监听网卡上的所有数据帧，通常仅限于高级用户或管理员运行的程序使用。
		   SOCK_RDM：是一种可靠的UDP形式，即保证交付数据但不保证顺序
	proto：协议号，通常为0，可以省略，或者在地址簇为AF_CAN的情况下，协议应为CAN_RAW或CAN_BCM 。
	fileno：一般忽略，有兴趣请自行学习
```

## bind(address)方法

```
#将socket绑定到地址（常用于服务端）
参数 ：
	address：地址
address地址的格式取决于地址族，在AF_INET下，以元组（‘ip’,端口号）的形式表示地址。
```

## listen方法(backlog)

```
开始监听传入连接。backlog指定在拒绝连接之前，可以挂起的最大连接数量。
backlog等于5，表示内核已经接到了连接请求，但服务器还没有调用accept进行处理的连接个数最大为5
 这个值不能无限大，因为要在内核中维护连接队列
```

## **setblocking方法(bool)**  　　

```
是否阻塞（默认True），如果设置False，那么accept和recv时一旦无数据，则报错
```

## accept方法

```
接受连接并返回（conn,address）,其中conn是新的套接字对象，可以用来接收和发送数据。address是连接客户端的地址。
```

## **connect方法(address)** 

```
连接到address处的套接字。一般，address的格式为元组（主机名,端口号）,如果连接出错，返回socket.error错误。
```

## **recv方法(bufsize[,flag])** 

```python
接受套接字的数据。数据以字符串形式返回，bufsize指定最多可以接收的数量。flag提供有关消息的其他信息，通常可以忽略。
#TCP协议
```

## **recvfrom方法(bufsize[.flag])** 

```python
与recv()类似，但返回值是（data,address）。其中data是包含接收数据的字符串，address是发送数据的套接字地址。
#UDP协议
```

## **send方法(string[,flag])** 

```python
将str中的数据发送到连接的套接字。返回值是要发送的字节数量，该数量可能小于str的字节大小。即：可能未将指定内容全部发送。
#TCP协议
```

## **sendall方法(string[,flag])** 

```python
将string中的数据发送到连接的套接字，但在返回之前会尝试发送所有数据。成功返回None，失败则抛出异常。
#TCP协议
内部通过递归调用send，将所有内容发送出去。
```

## **sendto方法(string[,flag],address)** 

```python
将数据发送到套接字，address是形式为（ipaddr，port）的元组，指定远程地址。返回值是发送的字节数。
#UDP协议
```

## **settimeout方法(timeout)**  

```
设置套接字操作的超时期，timeout是一个浮点数，单位是秒。值为None表示没有超时期。一般，超时期应该在刚创建套接字时设置，因为它们可能用于连接的操作（如 client 连接最多等待5s ）
```

## TCP

```markdown
传输控制协议（TCP，Transmission Control Protocol）是为了在不可靠的互联网络上提供可靠的端到端字节流而专门设计的一个传输协议,是一种面向连接的、可靠的、基于字节流的传输层通信协议
```

```markdown
应用层向TCP层发送用于网间传输的、用8位字节表示的数据流，然后TCP把数据流分区成适当长度的报文段（通常受该计算机连接的网络的数据链路层的最大传输单元（MTU）的限制）。之后TCP把结果包传给IP层，由它来通过网络将包传送给接收端实体的TCP层。TCP为了保证不发生丢包，就给每个包一个序号，同时序号也保证了传送到接收端实体的包的按序接收。然后接收端实体对已成功收到的包发回一个相应的确认（ACK）；如果发送端实体在合理的往返时延（RTT）内未收到确认，那么对应的数据包就被假设为已丢失将会被进行重传。TCP用一个校验和函数来检验数据是否有错误；在发送和接收时都要计算校验和。
```

## UDP

```
UDP 是User Datagram Protocol的简称， 中文名是用户数据报协议，是OSI（Open System Interconnection，开放式系统互联） 参考模型中一种无连接的传输层协议，提供面向事务的简单不可靠信息传送服务
```

```
UDP是OSI参考模型中一种无连接的传输层协议，它主要用于不要求分组顺序到达的传输中，分组传输顺序的检查与排序由应用层完成，提供面向事务的简单不可靠信息传送服务。UDP 协议基本上是IP协议与上层协议的接口。UDP协议适用端口分别运行在同一台设备上的多个应用程序。
```



## TCP接收端

```python
socket实例化 #建立套接字对象
bing方法 #将套接字绑定固定地址
listen方法 #开启监听传入连接
x1,x2=accept方法    #接受连接，x1为全新的套接字对象，x2为连接发送端的地址
x1.recv方法 #接收数据 此时，需要我们对传输的字节数量进行核对
```

## TCP发送端

```python
socket实例化 #建立套接字对象
connect方法 #连接到接收端套接字
send或sendall方法 #发送数据 需要对传输的字节总数进行先传输，用于核对的成功
```

## UDP接收端

```python
socket实例化 #建立套接字对象
bing方法 #将套接字绑定固定地址
x1,x2=recvfrom方法    #其中x1是包含接收数据的字符串，x2是发送数据的套接字地址
```

## UDP发送端

```python
socket实例化 #建立套接字对象
sendto方法 #将数据发送到指定的套接字
```

```
注意：调试过程中，最好在错误处理中添加关闭套接字指令 ------ 套接字对象.close()
```



