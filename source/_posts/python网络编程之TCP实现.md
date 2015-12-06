title:  python网络编程之TCP实现
date: 2014-12-26 23:51:00
categories: python开发
tags: [python,TCP实现]
toc: true
---
python的tcp是也是面向连接，服务器的过程和客户端的过程和其他语言类似，下面分析TCP实现。
<!--more-->
### **一、原理**

 tcp是面向连接，服务器的过程如下:
    1.创建一个socket(socket的类型，socket的协议)
    2.绑定(bind)一个端口，使客户端连接、
    3.设置监听队列(listen)的大小
    4.进入无限循环，使用accpet()接收客户端请求
    5.通过send/recv()对socket进行读写操作
  客户端的过程如下:
    1.创建一个socket(socket的类型，socket的协议)
    2.用connect连接远程端口、
    3.通过send/recv()对socket进行读写操作


### **二、代码实现如下:**

   1.服务器端:

```
import socket
from time import ctime
def tcpServer():
    address=("127.0.0.1",8080)
    #初始化socket
    tcpsock=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    #绑定端口地址
    tcpsock.bind(address)
    #设置监听
    tcpsock.listen(5)
    while True:
        print "wait a client connect..."
        #接受客户端连接
        con,addr=tcpsock.accept()
        while True:
            try:
                con.settimeout(20)
                #接收数据
                buf =con.recv(1024)
               #  con.send('[%s]%s' %(ctime(),buf))
                if buf=="1":
                    con.send("1")
                elif buf=="2":
                    con.send("2")
                elif buf=="3":
                    con.send("3")
                    break
                else:
                    con.send("unknow command")
            except socket.timeout:
                print "time out"
        con.close()
        print "a clinet exit..."
if __name__=="__main__":
    tcpServer() 
```

  2.客户端

```
def tcpClient():
    address=("127.0.0.1",8080)
    #初始化socket
    tcpClientsock =socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    #建立连接
    tcpClientsock.connect(address)
    while True:
        #输入口令
        data=raw_input("input the command:")
        #发送请求
        tcpClientsock.send(data)
        print tcpClientsock.recv(1024)
        if data==3:
            break
    tcpClientsock.close() 
if __name__=="__main__":
    tcpClient()
```



