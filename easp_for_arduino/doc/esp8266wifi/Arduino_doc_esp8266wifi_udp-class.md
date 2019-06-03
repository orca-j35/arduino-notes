---
title: ESP8266WiFi UDP Class
---

[ESP8266WiFi Library :back:](readme.md#udp)


## UDP Class

[TOC]

Methods documented for [WiFiUDP Class](https://www.arduino.cc/en/Reference/WiFiUDPConstructor) in [Arduino](https://github.com/arduino/Arduino)

1.  [begin()](https://www.arduino.cc/en/Reference/WiFiUDPBegin)
2.  [available()](https://www.arduino.cc/en/Reference/WiFiUDPAvailable)
3.  [beginPacket()](https://www.arduino.cc/en/Reference/WiFiUDPBeginPacket)
4.  [endPacket()](https://www.arduino.cc/en/Reference/WiFiUDPEndPacket)
5.  [write()](https://www.arduino.cc/en/Reference/WiFiUDPWrite)
6.  [parsePacket()](https://www.arduino.cc/en/Reference/WiFiUDPParsePacket)
7.  [peek()](https://www.arduino.cc/en/Reference/WiFiUDPPeek)
8.  [read()](https://www.arduino.cc/en/Reference/WiFiUDPRead)
9.  [flush()](https://www.arduino.cc/en/Reference/WiFiUDPFlush)
10.  [stop()](https://www.arduino.cc/en/Reference/WiFIUDPStop)
11.  [remoteIP()](https://www.arduino.cc/en/Reference/WiFiUDPRemoteIP)
12.  [remotePort()](https://www.arduino.cc/en/Reference/WiFiUDPRemotePort)


下面进一步描述的方法和性质是ESP8266特有的。 它们不包括在Arduino WiFi库文档中。 在完整记录之前，请参考以下信息。

>   Methods and properties described further down are specific to ESP8266. They are not covered in [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) documentation. Before they are fully documented please refer to information below.


### Multicast UDP 组播 UDP 

```
uint8_t  beginMulticast (IPAddress interfaceAddr, IPAddress multicast, uint16_t port) 
virtual int  beginPacketMulticast (IPAddress multicastAddress, uint16_t port, IPAddress interfaceAddress, int ttl=1) 
IPAddress  destinationIP () 
uint16_t  localPort ()
```

`WiFiUDP` 类支持在 STA 接口上发送和接受组播数据包。发送组播数据包时，将 `udp.beginPacket(addr, port)` 替换为 `udp.beginPacketMulticast(addr, port, WiFi.localIP())` 。当侦听组播数据包时，将 `udp.begin(port)` 替换为  `udp.beginMulticast(WiFi.localIP(), multicast_ip_addr, port)` 。你可以使用 `udp.destinationIP()` 告诉接收的数据包是发送到组播还是单播地址。

有关代码示例，请参阅单独的部分与示例： [examples :arrow_right:](udp-examples.md)  专用于UDP类。

>   The `WiFiUDP` class supports sending and receiving multicast packets on STA interface. When sending a multicast packet, replace `udp.beginPacket(addr, port)` with `udp.beginPacketMulticast(addr, port, WiFi.localIP())`. When listening to multicast packets, replace `udp.begin(port)` with `udp.beginMulticast(WiFi.localIP(), multicast_ip_addr, port)`. You can use `udp.destinationIP()` to tell whether the packet received was sent to the multicast or unicast address.

For code samples please refer to separate section with [examples :arrow_right:](udp-examples.md) dedicated specifically to the UDP Class.



## Arduino UDP

### WiFiUDP

-   Description

创建一个可以发送和接收UDP消息的WiFi UDP类的命名实例。

>   Creates a named instance of the WiFi UDP class that can send and receive UDP messages.

-   Syntax

WiFiUDP

-   Parameters

none

### WiFiUDP.begin()

-   Description

初始化WiFi UDP库和网络设置。 启动WiFiUDP套接字，在本地端口侦听PORT * /

>   Initializes the WiFi UDP library and network settings. Starts WiFiUDP socket, listening at local port PORT */

-   Syntax

WiFiUDP.begin(port); 

-   Parameters

**port**: the local port to listen on (int)

-   Returns
    -   1: if successful
    -   0: if there are no sockets available to use



### available()

-   Description

获取可用于从缓冲区读取的字节数（字符）。 这是已经到达的数据。

此功能只能在 UDP.parsePacket() 之后成功调用。

available（）从Stream实用程序类继承。

>   Get the number of bytes (characters) available for reading from the buffer. This is data that's already arrived.
>
>   This function can only be successfully called after [WiFiUDP.parsePacket()](https://www.arduino.cc/en/Reference/WiFiUDPParsePacket).
>
>   available() inherits from the [Stream](https://www.arduino.cc/en/Reference/Stream) utility class.

-   Syntax

*WiFiUDP*.available()

-   Parameters

None

-   Returns
    -   the number of bytes available in the current packet
    -   0: if [parsePacket](https://www.arduino.cc/en/Reference/WiFiUDPParsePacket) hasn't been called yet

### WiFiUDP.beginPacket()

-   Description

启动连接以将 UDP 数据写入远程连接

>   Starts a connection to write UDP data to the remote connection

-   Syntax

WiFiUDP.beginPacket(hostName, port); 
WiFiUDP.beginPacket(hostIp, port);

-   Parameters

**hostName**: the address of the remote host. It accepts a character string or an IPAddress
**hostIp**: the IP address of the remote connection (4 bytes)
**port**: the port of the remote connection (int)

-   Returns
    -   1: if successful
    -   0: if there was a problem with the supplied IP address or port

### WiFiUDP.endPacket()

-   Description

在将UDP数据写入远程连接后调用。 它完成数据包并发送它。

>   Called after writing UDP data to the remote connection. It finishes off the packet and send it.

-   Syntax

WiFiUDP.endPacket(); 

-   Parameters

None

-   Returns
    -   1: if the packet was sent successfully
    -   0: if there was an error

### WiFiUDP.write()

-   Description

将UDP数据写入远程连接。 必须包裹在n [beginPacket](https://www.arduino.cc/en/Reference/WiFiUDPBeginPacket)() and [endPacket](https://www.arduino.cc/en/Reference/WiFiUDPEndPacket)() 之间。beginPacket() 初始化数据包，直到调用 endPacket()时才发送。

>   Writes UDP data to the remote connection. Must be wrapped between [beginPacket](https://www.arduino.cc/en/Reference/WiFiUDPBeginPacket)() and [endPacket](https://www.arduino.cc/en/Reference/WiFiUDPEndPacket)(). beginPacket() initializes the packet of data, it is not sent until endPacket() is called.

-   Syntax

WiFiUDP.write(byte);
WiFiUDP.write(buffer, size);

-   Parameters

**byte**: the outgoing byte 输出字节
**buffer**: the outgoing message 输出消息
**size**: the size of the buffer 缓冲区的大小

-   Returns
    -   single byte into the packet 包中的单个字节
    -   bytes size from buffer into the packet 从缓冲区到数据包的字节大小

### WiFiUDP.parsePacket() 包解析

-   Description

它开始处理下一个可用的传入数据包，检查是否存在UDP数据包，并报告大小。 在使用 [UDP.read()](https://www.arduino.cc/en/Reference/WiFiUDPRead) 读取缓冲区之前，必须调用parsePacket() 。

>   It starts processing the next available incoming packet, checks for the presence of a UDP packet, and reports the size. parsePacket() must be called before reading the buffer with [UDP.read()](https://www.arduino.cc/en/Reference/WiFiUDPRead).

-   Syntax

UDP.parsePacket(); 

-   Parameters

None

-   Returns

    -   the size of the packet in bytes
    -   0: if no packets are available

- See Also

    [read()](https://www.arduino.cc/en/Reference/WiFiUDPRead)

### WiFiUDP.peek()

-   peek()

从文件中读取一个字节，而不进入下一个字节。 也就是说，对 peek() 的连续调用将返回相同的值，会持续到下一次调用 read() 为止。

>   Read a byte from the file without advancing to the next one. That is, successive calls to peek() will return the same value, as will the next call to read().

This function inherited from the Stream class. See the [Stream class](https://www.arduino.cc/en/Reference/Stream) main page for more information.

-   Syntax

*WiFiUDP*.peek()

-   Parameters

none

-   Returns
    -   b: the next byte or character
    -   -1: if none is available

### WiFiUDP.read()

-   Description

读取 UDP 数据到指定缓冲区。 如果没有给出参数，它将返回缓冲区中的下一个字符。

此功能只能在WiFiUDP.parsePacket（）之后成功调用。

>   Reads UDP data from the specified buffer. If no arguments are given, it will return the next character in the buffer.
>
>   This function can only be successfully called after [WiFiUDP.parsePacket()](https://www.arduino.cc/en/Reference/WiFiUDPParsePacket).

-   Syntax

WiFiUDP.read();
WiFiUDP.read(buffer, len); 

-   Parameters

**buffer**: buffer to hold incoming packets (char*) 保存数据包的缓冲区
**len**: maximum size of the buffer (int) 缓冲区的最大尺寸

-   Returns
    -   b: the characters in the buffer (char)
    -   size: the size of the buffer
    -   -1: if no buffer is available

### flush()

丢弃已写入客户端但尚未读取的任何字节。

>   Discard any bytes that have been written to the client but not yet read.

flush() inherits from the [Stream](https://www.arduino.cc/en/Reference/Stream) utility class.

-   Syntax

*WiFiUDP*.flush()

-   Parameters

none

-   Returns

none

-   See also

    [Stream.flush()](https://www.arduino.cc/en/Reference/StreamFlush)



### WiFiUDP.stop()

-   Description

断开与服务器的连接。 释放在UDP会话期间使用的任何资源。

>   Disconnect from the server. Release any resource being used during the UDP session.

-   Syntax

*WiFiUDP*.stop()

-   Parameters

none

-   Returns

none

### WiFiUDP.remoteIP()

-   Description

获取远程连接的IP地址。

此函数必须在 WiFiUDP.parsePacket() 之后调用。

>   Gets the IP address of the remote connection.
>
>   This function must be called after [WiFiUDP.parsePacket()](https://www.arduino.cc/en/Reference/WiFiUDPParsePacket).

-   Syntax

WiFiUDP.remoteIP(); 

-   Parameters

None

-   Returns

    4 bytes : the IP address of the host who sent the current incoming packet 
    ​                发送当前传入数据包的主机的IP地址

### WiFIUDP.remotePort()

-   Description

获取远程 UDP 连接的端口。

此函数必须在 UDP.parsePacket( ) 之后调用。

>   Gets the port of the remote UDP connection.
>
>   This function must be called after [UDP.parsePacket()](https://www.arduino.cc/en/Reference/WiFiUDPParsePacket).

-   Syntax

UDP.remotePort(); 

-   Parameters

None

-   Returns

发送当前传入数据包的主机的端口

>   The port of the host who sent the current incoming packet

