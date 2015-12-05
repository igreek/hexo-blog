title:  核心java系列——I/O流详解（一）
date: 2015-11-24 15:47:17
categories: 核心java
tags: [java]
toc: true
---
流的概念流是一系列有顺序的字节的集合，是网络传输的载体，流可以包装成基本数据类型或对象，流有输入和输出，输入时是从流从数据源流向程序输出时是流从程序传向数据源，而数据源可以是内存，文件，网络或程序等。
<!--more-->
### 一、流的概念
流是一系列有顺序的字节的集合，是网络传输的载体，流可以包装成基本数据类型或对象，流有输入和输出，输入时是从流从数据源流向程序输出时是流从程序传向数据源，而数据源可以是内存，文件，网络或程序等，下面是流的输入和输出的图形化：
![这里写图片描述](http://img.blog.csdn.net/20151126200300792)

###  二、流的分类
**(1)、流有字节流，字符流，输入流和输出流等。**

 - 根据处理方向不同可分为：输入流和输出流。
 - 根据处理数据类型不同可分为：字节流和字符流。
字节流和字符流的处理原理是相同的，只是处理的数据类型不同。
字节流是以字节为单位来传输，一个字节是8bit;
字符流是以字符为单位来传输，一个字符是16bit。
 - 根据分工的不同可分为：节点流和处理流

**(2)、java I/O流基本类有**
字节流的抽象基类：
InputStream(字节输入流),OutputStream(字节输出流)
字符流的抽象基类:
Reader(字符输入流)，writer(字符输出流)
其他类都由这4个抽象基本派生出来，详细的流的类图结构如下:
![这里写图片描述](http://img.blog.csdn.net/20151126200445656)



### 三、输入和输出流
**(1)、输入可使用：**
1.InputStream – 一个字节一个字节(byte)地从数据源读取。
*读取一个字节并以整数的形式返回(0~255),如果返回-1已到输入流的末尾。
int read() ；
*读取一系列字节并存储到一个数组buffer，返回实际读取的字节数，如果读取前已到输入流的末尾返回-1。
int read(byte[] buffer) ；
*读取length个字节并存储到一个字节数组buffer，从off位置开始存,最多len， 返回实际读取的字节数，如果读取前以到输入流的末尾返回-1。
int read(byte[] buffer, int off, int len)  ；                  
*关闭流释放内存资源。
void close() ;
2.Reader – 一个字符一个字符(char)地从数据源读取。
*读取一个字符并以整数的形式返回(0~255),如果返回-1已到输入流的末尾。
int read() ；
*读取一系列字符并存储到一个数组buffer，返回实际读取的字符数，如果读取前已到输入流的末尾返回-1。
int read(char[] cbuf) ；
*读取length个字符,并存储到一个数组buffer，从off位置开始存,最多读取len，返回实际读取的字符数，如果读取前以到输入流的末尾返回-1。
int read(char[] cbuf, int off, int len)                  
*关闭流释放内存资源。
void close() 
**(2)、输入流可使用**
1.OutputStream:
*向输出流中写入一个字节数据,该字节数据为参数b的低8位。
void write(int b) ;
*将一个字节类型的数组中的数据写入输出流。
void write(byte[] b);
*将一个字节类型的数组中的从指定位置（off）开始的,len个字节写入到输出流。
void write(byte[] b, int off, int len);
*关闭流释放内存资源。
void close();
*将输出流中缓冲的数据全部写出到目的地。
void flush();

2.Writer:
*向输出流中写入一个字符数据,该字节数据为参数b的低16位。
void write(int c);
*将一个字符类型的数组中的数据写入输出流，
void write(char[] cbuf) throws IOException
*将一个字符类型的数组中的从指定位置（offset）开始的,length个字符写入到输出流。
void write(char[] cbuf, int offset, int length);
*将一个字符串中的字符写入到输出流。
void write(String string);
*将一个字符串从offset开始的length个字符写入到输出流。
void write(String string, int offset, int length);
*关闭流释放内存资源。
void close() throws IOException 
*将输出流中缓冲的数据全部写出到目的地。
void flush() throws IOException

### 四、读写文本文件
**(1).写文本文件**
1.FileWriter是继承writer类，可调用write()方法往文本中写入内容。下面程序为调用FileWriter类的write(type c)方法的实现:

```
String fileName="d:\\demo1.txt";
FileWriter write=new FileWriter(fileName);
write.write("hell,io\n");
write.write("welcome to study\n");
write.write("加油，谢谢！");
write.close();
```

2.BufferedWriter类是带缓冲区的，比FileWriter要高效些，若写入的内容多优先使用此类，此类有一个newLine()方法，可换行。
下面程序为BufferedWriter类write(type c)方法的实现:

```
//处理内容较多的数据是，用BufferedWriter更高效
BufferedWriter bWrite=new BufferedWriter(new FileWriter(fileName));
bWrite.write("hell,io");
bWrite.newLine();
bWrite.write("welcome to study");
bWrite.newLine();
bWrite.write("加油，谢谢！");
bWrite.close();
```

不论使用哪种方式，结束时都需要close()关闭流，否则会导致资源耗尽问题。
**(2).读文本文件**
1.FileReader类是从文本文件读取字符，下面代码为FileReader类读取文本文件的实现，返回的是一个int类型数。若读取到末尾，则返回-1，下面为具体的实现:

```
FileReader read=new FileReader(fileName);
int len=read.read();
while(len!=-1){
	System.out.println("len:"+len);
	}
```

2.BufferedReader类是文本文件读取的缓存器类，调用readLine()方法，可一行一行的读取出整行字符，若读取到末尾返回null,下面为具体的实现:

```
BufferedReader read=new BufferedReader(new FileReader(fileName));
String line=read.readLine();
while(line!=null){
	System.out.println("line:"+line);
	line=read.readLine();
}
read.close();
```

同时不论使用哪种方式，结束时都需要调用close()关闭流，否则会导致资源耗尽问题.

### 五、读写二进制数据
对于纯文本文件里的内容都可解释为字符，可以用Reader和writer进行读写，但对于那些图片，声音的不是纯文本的内容，则需要利用二进制的字节方式进行读写，利用二进制字节读取数据要比字符快，且编码问题小。
**(1).写二进制字节数据**
FileOutputStream类:用于字节的输出；DataOutputStream类:用于将数据写到另一个输出流。下面为具体的实现:

```
DataOutputStream out =new DataOutputStream(new FileOutputStream(fileName));
out.writeInt(1);
out.writeDouble(11.20);
out.writeUTF("test");
out.close();
```

**(2).读二进制文件**

通过另外一个流来构造一个过滤流，常用的子类有
DataInputStream 和 BufferedInputStream。后者是将字节数据读取到缓冲区.相对要高效些。下面为具体的实现:

```
String fileName="d:\\demo.dat";
DataInputStream in=new	DataInputStream(new BufferedInputStream(new FileInputStream(fileName)));
System.out.println(in.readInt()+"-"+in.readDouble()+"-"+in.readUTF());
in.close();
```

还有几个特殊的类,如:
LineNumberInputStream:构造一个读取指定的输入流的输入​​的新行号输入流。
PushbackInputStream:构建一个可预览一个字节或具有指定尺寸的缓冲区的流。
总结：本文主要分析了流的作用，流的分类，流的类图结构以及流的一些操作，比较基础，下篇将介绍流的文件管理，文档的压缩和对象序列化等操作。