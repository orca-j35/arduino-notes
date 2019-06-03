---
title: ESP8266WiFi UDP Class - Sketch Examples
---

[ESP8266WiFi Library :back:](readme.md#udp)

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**udp-examples.md**

## UDP

下面这个示例应用程序的目的是演示 ESP8266 和外部客户端之间的 UDP 通讯。应用程序(执行服务器的角色 ) 在 `loop()` 内部检测 UDP 数据包是否到达。当接收到有效的数据包时，确认数据包将被发送回客户端同一端口( 发送该数据包的端口 )

>   The purpose of example application below is to demonstrate UDP communication between ESP8266 and an external client. The application (performing the role of a server) is checking inside the `loop()` for an UDP packet to arrive. When a valid packet is received, an acknowledge packet is sent back to the client to the same port it has been sent out.

[TOC]

### 声明 Declarations

在 sketch 的开头，我们需要包含两个库。

>   At the beginning of sketch we need to include two libraries:

```
#include <ESP8266WiFi.h>
#include <WiFiUdp.h>
```

如果我们需要使用 ESP8266 的WiFi 功能，`ESP8266WiFi.h` 默认情况下是必须的。需要 `WiFiUdp.h` 用于 UDP 例程的编程。

一旦在部署了库，我们需要创建一个 `WiFiUDP` 对象。接下来我们应该指定一个端口用于监听传入的数据包。有关端口号使用惯例的信息，请参考  [List of TCP and UDP port numbers](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) 。最后，我们需要为传入的数据包设置缓冲区，并定义一个回复消息。

>   The first library `ESP8266WiFi.h` is required by default if we are using ESP8266's Wi-Fi. The second one `WiFiUdp.h` is needed specifically for programming of UDP routines.
>
>   Once we have libraries in place we need to create a `WiFiUDP` object. Then we should specify a port to listen to incoming packets. There are conventions on usage of port numbers, for information please refer to the [List of TCP and UDP port numbers](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers). Finally we need to set up a buffer for incoming packets and define a reply message.

```
WiFiUDP Udp;
unsigned int localUdpPort = 4210;// 注意是无符号int
char incomingPacket[255];
char  replyPacekt[] = "Hi there! Got the message :-)";
```

### Wi-Fi Connection 

在 `setup()` 函数开始处，利用典型代码实现与接入点的连接。 这已在快速入门中讨论过。 如有需要请参考。

>   At the beginning of `setup()` let's implement typical code to connect to an access point. This has been discussed in [Quick Start](readme.md#quick-start). Please refer to it if required.


### UDP Setup

一旦连接被建立，便可以开始收听传入的数据包。

>   Once connection is established, you can start listening to incoming packets.

```
Udp.begin(localUdpPort);
```

这便是所有需要准备的步骤。让我们去到 `loop()` 函数，在这里我们将处理实际发生的 UDP 通讯。

>   That is all required preparation. We can move to the `loop()` that will be handling actual UDP communication.


### An UDP Packet Arrived!

等待传入 UDP 包由以下代码完成：

>   Waiting for incoming UDP packed is done by the following code:

```
int packetSize = Udp.parsePacket();//包解析
if (packetSize)
{
  Serial.printf("Received %d bytes from %s, port %d\n", packetSize, Udp.remoteIP().toString().c_str(), Udp.remotePort());
  int len = Udp.read(incomingPacket, 255);
  if (len > 0)
  {
    incomingPacket[len] = 0;//需要加入终止字符
  }
  Serial.printf("UDP packet contents: %s\n", incomingPacket);

  (...)
}
```

一旦接收到数据包，以上代码将打印出发送方的 IP 地址和端口号，以及接受到的数据包的长度。如果数据包不为空，其内容也将被打印出来。

>   Once a packet is received, the code will printing out the IP address and port of the sender as well as the length of received packet. If the packet is not empty, its contents will be printed out as well.


### An Acknowledge Send Out 

对于每个已经被接受的数据包，我们发送一个应答数据包：

>   For each received packet we are sending back an acknowledge packet:

```
Udp.beginPacket(Udp.remoteIP(), Udp.remotePort());
Udp.write(replyPacekt);
Udp.endPacket();
```

请注意，我们通过使用 `Udp.remoteIP()` 和 `Udp.remotePort()` 向发送方的 IP 和 端口发送应答。

>   Please note we are sending reply to the IP and port of the sender by using `Udp.remoteIP()` and `Udp.remotePort()`.


### Complete Sketch

执行以上描述的所有功能的 sketch 如下：

>   The sketch performing all described functionality is presented below:

```
#include <ESP8266WiFi.h>
#include <WiFiUdp.h>

const char* ssid = "********";
const char* password = "********";

WiFiUDP Udp;
unsigned int localUdpPort = 4210;  // local port to listen on
char incomingPacket[255];  // buffer for incoming packets
char  replyPacekt[] = "Hi there! Got the message :-)";  // a reply string to send back


void setup()
{
  Serial.begin(115200);
  Serial.println();

  Serial.printf("Connecting to %s ", ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println(" connected");

  Udp.begin(localUdpPort);
  Serial.printf("Now listening at IP %s, UDP port %d\n", WiFi.localIP().toString().c_str(), localUdpPort);
}


void loop()
{
  int packetSize = Udp.parsePacket();
  if (packetSize)
  {
    // receive incoming UDP packets
    Serial.printf("Received %d bytes from %s, port %d\n", packetSize, Udp.remoteIP().toString().c_str(), Udp.remotePort());
    int len = Udp.read(incomingPacket, 255);
    if (len > 0)
    {
      incomingPacket[len] = 0;
    }
    Serial.printf("UDP packet contents: %s\n", incomingPacket);

    // send back a reply, to the IP address and port we got the packet from
    Udp.beginPacket(Udp.remoteIP(), Udp.remotePort());
    Udp.write(replyPacekt);
    Udp.endPacket();
  }
}
```

### 如何检查工作状态

How to Check It?

将 sketch 上传到模块并打开串行监视器。您应该看到ESP连接到Wi-Fi并开始监听UDP数据包的确认信息：

>   Upload sketch to module and open serial monitor. You should see confirmation that ESP has connected to Wi-Fi and started listening to UDP packets:

```
Connecting to twc-net-3 ........ connected
Now listening at IP 192.168.1.104, UDP port 4210
```

现在我们需要另一个应用程序将一些数据包发送到上面的ESP所示的IP和端口。

这里不需要使用另外一个 ESP 编程，我们

而不是编写另一个ESP，我们让它更容易，并使用目的构建应用程序。 我选择了Packet Sender。 它适用于流行的操作系统。 下载，安装并执行。

一旦Packet Sender的窗口出现，输入以下信息：

- 数据包的名称
  -   要在数据包中发送的消息的ASCII文本
  -   我们的ESP显示的IP地址
  -   ESP显示的端口
  -   选择UDP

我输入的内容如下所示：

>   Now we need another application to send some packets to IP and port shown by ESP above.
>
>   Instead of programming another ESP, let's make it easier and use a purpose build application. I have selected the [Packet Sender](https://packetsender.com/download). It is available for popular operating systems. Download, install and execute it. 
>
>   Once Packet Sender's window show up enter the following information:
>   * *Name* of the packet
>     * *ASCII* text of the message to be send inside the packet
>     * IP *Address* shown by our ESP
>     * *Port* shown by the ESP
>     * Select *UDP* 
>

我输入的内容如下所示：

What I have entered is shown below: 

![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/udp-packet-sender.png)

Now click *Send*. 

紧接着，您应该在ESP的串行监视器上看到以下内容：

>   Immediately after that you should see the following on ESP's serial monitor:

```
Received 12 bytes from 192.168.1.106, port 55056
UDP packet contents: Hello World!
```

文本 `192.168.1.106, port 55056` 标识发送数据包的 PC。 你可能会看到不同的值。

当ESP发回确认数据包时，您应该在Packet Sender窗口的底部的日志中看到它。

>   The text `192.168.1.106, port 55056` identifies a PC where the packet is send from. You will likely see different values.
>
>   As ESP sends an acknowledge packet back, you should see it in the log in the bottom part of the Packet Sender's window. 

上面的那个软件下载太慢了，自己用 python 写了一个

```
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
for data in [b'Michael']:
    # 发送数据:
    s.sendto(data, ('192.168.95.3', 4210))
    # 接收数据:
    print(s.recv(1024).decode('utf-8'))
s.close()
```


### Conclusion

这个简单的例子显示了如何在ESP和外部应用程序之间发送和接收UDP数据包。 一旦在这个最小的设置中进行测试，您应该能够编程ESP与任何其他UDP设备通信。 在与新设备建立通信的情况下，请使用Packet Sender或其他类似的程序进行故障排除

有关用于发送和接收UDP数据包的功能的审查，请参阅UDP类：arrow_right：文档。

>   This simple example shows how to send and receive UDP packets between ESP and an external application. Once tested in this minimal set up, you should be able to program ESP to talk to any other UDP device. In case of issues to establish communication with a new device, use the [Packet Sender](https://packetsender.com) or other similar program for troubleshooting 
>
>   For review of functions provided to send and receive UDP packets, please refer to the [UDP Class :arrow_right:](udp-class.md) documentation.