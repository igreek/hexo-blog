title:  python网络编程之UDP实现
date: 2014-12-27 00:01:00
categories: python开发
tags: [python,udp实现]
toc: true
---
python udp是无连接，没有TCP的三次握手，错误重传机制，发的只管发，收的只管收，效率比TCP高，运用于对数据帧不高的地方，如视频，音频的传输。
<!--more-->
### **一、简介:**

python udp是无连接，没有TCP的三次握手，错误重传机制，发的只管发，收的只管收，效率比TCP高，运用于对数据帧不高的地方，如视频，音频的传输。
### **二、实现过程:**
   服务器端过程如下:
1.建立UDP的SOCKET
2.绑定一个接口，让客户端连接
3.接受数据

 客户端过程如下:
1.创建一个socket
2.收发数据报

### python网络编程之UDP实现
   1.服务器端：
```
import socket
from time import ctime
def udpServer():
    buffer=2048
    address=("127.0.0.1",8080)
    udpsock=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    udpsock.bind(address)
    while True:
        print "wait for message..."
        data,addr=udpsock.recvfrom(buffer)
        udpsock.sendto('[%s]%s' %(ctime(),data),addr)
        print '...received from and retuned to:',addr 
    udpsock.close()

if __name__=="__main__":
    udpServer() 
```

  2.客户端:

```
import socket
def udpClient():
    address=('localhost',8080)
    udpClientSocket=socket.socket(socket.AF_INET,socket.SOCK_DGRAM) #创建socket
    while True:
        data=raw_input('>')
        if not data:
            break
        udpClientSocket.sendto(data,address) #发送数据
        data,addr=udpClientSocket.recvfrom(2048)
        print data
    udpClientSocket.close()
 
if __name__=="__main__":
     udpClient()  
```



