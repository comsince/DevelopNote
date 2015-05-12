# Push 推送技术说明
[TOC]
## Java NIO 使用说明
* 参考资料
  [Java NIO Tutorial](http://tutorials.jenkov.com/java-nio/index.html)


* 简介
Java NIO (New IO) is an alternative IO API for Java (from Java 1.4), meaning alternative to the standard Java IO and Java Networking API's. Java NIO offers a different way of working with IO than the standard IO API's. 

### Java NIO: Channels and Buffers
In the standard IO API you work with byte streams and character streams. In NIO you work with channels and buffers. Data is always read from a channel into a buffer, or written from a buffer to a channel. 
### Java NIO: Non-blocking IO
Java NIO enables you to do non-blocking IO. For instance, a thread can ask a channel to read data into a buffer. While the channel reads data into the buffer, the thread can do something else. Once data is read into the buffer, the thread can then continue processing it. The same is true for writing data to channels. 
### Java NIO: Selectors
Java NIO contains the concept of "selectors". A selector is an object that can monitor multiple channels for events (like: connection opened, data arrived etc.). Thus, a single thread can monitor multiple channels for data. 

## Java NIO Overview
### Channels and Buffers
Typically, all IO in NIO starts with a Channel. A Channel is a bit like a stream. From the Channel data can be read into a Buffer. Data can also be written from a Buffer into a Channel
NIO 都是以Channel开始的，Channel就像一个字节流，Channel和Buffer两者相互之间进行数据读写
 There are several Channel and Buffer types. Here is a list of the primary Channel implementations in Java NIO:
 
* FileChannel
* DatagramChannel
* SocketChannel
* ServerSocketChannel
these channels cover UDP + TCP network IO, and file IO. 

Here is a list of the core Buffer implementations in Java NIO:

* ByteBuffer
* CharBuffer
* DoubleBuffer
* FloatBuffer
* IntBuffer
* LongBuffer
* ShortBuffer
These Buffer's cover the basic data types that you can send via IO: byte, short, int, long, float, double and characters. 

### Selectors

A Selector allows a single thread to handle multiple Channel's. This is handy if your application has many connections (Channels) open, but only has low traffic on each connection. For instance, in a chat server. 
Here is an illustration of a thread using a Selector to handle 3 Channel's: 

![image](http://tutorials.jenkov.com/images/java-nio/overview-selectors.png)

A Selector is a Java NIO component which can examine one or more NIO Channel's, and determine which channels are ready for e.g. reading or writing. This way a single thread can manage multiple channels, and thus multiple network connections. 

一个selector可以管理多个channel，这样一个线程就可以管理多个channels
下面具体介绍selector的使用方法及步骤：
#### 1.Creating a Selector
```java
Selector selector = Selector.open();
```
#### 2.Registering Channels with the Selector
In order to use a Channel with a Selector you must register the Channel with the Selector. This is done using the SelectableChannel.register() method, like this: 
为了使用利用Selector管理channel，首先必须注册该channel，调用register()方法：
```java
channel.configureBlocking(false);
SelectionKey key = channel.register(selector, SelectionKey.OP_READ);
```
#### 3.SelectionKey's
#### 4.Selecting Channels via a Selector
#### 5.Full Selector Example
```java
Selector selector = Selector.open();

channel.configureBlocking(false);

SelectionKey key = channel.register(selector, SelectionKey.OP_READ);


while(true) {

  int readyChannels = selector.select();

  if(readyChannels == 0) continue;


  Set<SelectionKey> selectedKeys = selector.selectedKeys();

  Iterator<SelectionKey> keyIterator = selectedKeys.iterator();

  while(keyIterator.hasNext()) {

    SelectionKey key = keyIterator.next();

    if(key.isAcceptable()) {
        // a connection was accepted by a ServerSocketChannel.

    } else if (key.isConnectable()) {
        // a connection was established with a remote server.

    } else if (key.isReadable()) {
        // a channel is ready for reading

    } else if (key.isWritable()) {
        // a channel is ready for writing
    }

    keyIterator.remove();
  }
}

```


* [NIO Example](http://www.javadocexamples.com/java_source/br/ufsc/das/usage/nio/SelectorThread.java.html)

