HTTP协议

通信模型：(OSI七层参考模型和TCP/IP模型)

![网络通信模型](images/网络通信模型.png)

套接字：

* IP:port(IP.port)
  - tcp/port
  - udp/port
  - IP报文的总长度，使用16位来表示，故最大长度不能超过65535bit，还得减去报文首部等；
  - IP报文的长度还受限于MTU(最大传输单元)，大于网络设备的MTU，将被再次切片；
* Unix Sock(最早由BSD实现)
  - 基于filesystem


传输层协议：

* 面向连接的:tcp(Transmisssion Control Protocol,传输控制协议)。需要建立专有的虚拟连接；
* 无连接的：udp(User Datagram Protocol,用户数据包协议)，在DNS、QQ、tftp等中使用的都是UDP。

套接字类型：

* TCP套接字：IP加TCP端口；
* UDP套接字：IP加UDP端口；
* raw套接字：raw socket，即原始套接字，可以接收本机网卡上的数据帧或者数据包，对于监听网络的流量和分析是很有作用的；


C/S通信模型：

* Client端：使用服务，使用本地的某个套接字，访问Sever端所监听的套接字上，主动打开；
* Server端：提供服务，向内核申请监听在本地的某个套接字上，被动等待(打开)。
* 连接组成：
  - ClientIP
  - ClientPort
  - ServerIP
  - ServerPort
  由同一客户端的同一程序发起多个进程的方式访问服务器端所建立的连接就属于不同的连接；
  此时，多个连接间唯一不同的只是ClientPort，故上述四个组成部分只要有一个不同即可标识一个不同的连接。

* C/S分工：
  - 通信子网(传输)
  - 应用层协议(特定应用)： http、https、smtp、pop、imap、ftp、ldap
  - 基于套接字通信的流程：

![基于套接字通信的流程](images/基于套接字通信的流程.png)


## http协议：

hypertext transport protocol，超文本传输协议；

http协议版本：

* http/0.9 : 1991年，仅用于传输html文档，纯文本；
* http/1.0 : 引入MIME，支持多媒体数据的处理，引入keep-alive(保持连接)，有缓存功能；
* http/1.1 : 支持更多的请求方法，更精细的缓存控制机制，原生支持持久连接；

目前比较流行的是1.0和1.1

MIME： Multipurpose Mail Extension： 多功能、多用途互联网邮件扩展；
引入base64编码：
    将二进制数据编码成文本发送，并能够让接收方还原回原来的格式；
    MIME：多媒体类型；
```
major/minor
    HTML:text/html
    ASCII:text/plagin
    JPEG:image/jpeg
    GIF:image/gif
    QuickTime(流媒体):video/quicktime
```

html：hypertext mark language，超文本标记语言

html格式的文件:

```html
  <html>
     <head>
        <title></title>
     </head>
     <boby>
          <h1></h1>
            <p><img src=""></p>
           <h2></h2>
            <p>  <a href="a.html" > </a>  </p>
     </body>
   </html>
```
浏览器读取到使用html编写的文本后通过特定语法将其显示到浏览器上。

CSS: Cascading style sheet,层叠样式表；

动态页面： 除了html页面，还有程序脚本；

* 客户端脚本：不安全；
* 服务器端脚本： CGI(Common Gateway Interface,通用网关接口)
  - C,C++
  - perl
  - python
  - php 
  - asp.net
  - jsp

页面： 一个页面中，可能会包含多个页面对象；
URI：唯一标识web资源(页面对象)
    web资源引用方式：
        相对地址；
        绝对地址；

URI： Uniform Resource Identifier，同一资源标识符；
URL： Uniform Resource Locator，同一资源定位符，是URI的子集；
    URL： http://www.sslinux.com:80/images/logo.gif

C/S :

* Client: Browser:
  - GUI: Chrome Firefox,IE etc.
  - CLI: lynx elinks
* Server: http server


## HTTP报文

HTTP事务：一次请求以及与其对应的响应；

![一次基本的http事务](images/一次基本的http事务.png)


HTTP方法(资源操作方法，请求方法)：

* GET: 请求一个资源，需要服务器发送；安全方法；
* HEAD：跟GET相似，但其不需要服务器发送资源，而仅传回响应报文的首部；安全的首部；
* POST：支持HTML表单提交，表单中有用户填入的数据，这些数据会发送至服务器端，由服务器存储至某位置(例如发送处理程序)。
* PUT：与GET相反，向服务器写入文档；例如：发布系统；包含主体；
* DELETE： 请求删除URL指向的资源；
* TRACE： 追踪请求资源要经过的防火墙、代理和网关等；
* OPTIONS：探测服务器端对某资源所支持的请求方法；
* 扩展方法：
  - LOCK
  - MKCOL
  - COPY
  - MOVE

