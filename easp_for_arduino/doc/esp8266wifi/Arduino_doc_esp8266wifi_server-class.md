---
title: ESP8266WiFi Server Class
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**server-class.md**

[ESP8266WiFi Library :back:](readme.md#server)


## Server Class

[TOC]

Methods documented for the [Server Class](https://www.arduino.cc/en/Reference/WiFiServerConstructor) in [Arduino](https://github.com/arduino/Arduino)

1.  [WiFiServer()](https://www.arduino.cc/en/Reference/WiFiServer)
2.  [begin()](https://www.arduino.cc/en/Reference/WiFiServerBegin)
3.  [available()](https://www.arduino.cc/en/Reference/WiFiServerAvailable)
4.  [write()](https://www.arduino.cc/en/Reference/WiFiServerWrite)
5.  [print()](https://www.arduino.cc/en/Reference/WiFiServerPrint)
6.  [println()](https://www.arduino.cc/en/Reference/WiFiServerPrintln)


下面进一步描述的方法和性质是ESP8266特有的。 它们不包括在Arduino WiFi库文档中。 在完整记录之前，请参考以下信息。

>   Methods and properties described further down are specific to ESP8266. They are not covered in [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) documentation. Before they are fully documented please refer to information below.


### setNoDelay( )

```cpp
setNoDelay(nodelay)
```

将 `nodelay` 设置为 `true` ，此函数将禁用 Nagle 算法。

此算法旨在通过组合多个小的输出消息并立即发送它们，来减少通过网络发送的小分组的 TCP/IP 流量。 这种方法的缺点是有效地延迟单个消息，直到组装足够大的分组。

>   With `nodelay` set to `true`, this function will to disable [Nagle algorithm](https://en.wikipedia.org/wiki/Nagle%27s_algorithm). 
>
>   This algorithm is intended to reduce TCP/IP traffic of small packets sent over the network by combining a number of small outgoing messages, and sending them all at once. The downside of such approach is effectively delaying individual messages until a big enough packet is assembled.
>

*Example:*
```cpp
server.begin();
server.setNoDelay(true);
```


### Other Function Calls

```
bool  hasClient () 
bool  getNoDelay () 
virtual size_t  write (const uint8_t *buf, size_t size) 
uint8_t  status () 
void  close () 
void  stop ()
```

上述功能的文档尚未准备好。

有关代码示例，请参阅专门针对 Server Class 的示例的单独部分。

>   Documentation for the above functions is not yet prepared.
>
>   For code samples please refer to separate section with [examples :arrow_right:](server-examples.md) dedicated specifically to the Server Class.

## Arduino Server

Server class 用于创建服务器，该服务器可从所连接的客户端发送和接受数据( 客户端的程序运行在另一个电脑或设备上 )

>   The Server class creates servers which can send data to and receive data from connected clients (programs running on other computers or devices).

### Server

-   Description

server 作为所有 WiFi 服务器的基础调用 base class。它不会被直接调用，而是在使用依赖它的函数时调用。

>   Server is the base class for all WiFi server based calls. It is not called directly, but invoked whenever you use a function that relies on it.

### WiFiServer() 设置服务器端口

-   Description

创建侦听指定端口上的传入连接的服务器。

Creates a server that listens for incoming connections on the specified port.

-   Syntax

Server(port);

-   Parameters

**port**: the port to listen on (int) 端口监听

-   Returns

None

-   Example

```
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "myNetwork";          //  your network SSID (name) 
char pass[] = "myPassword";   // your network password
int status = WL_IDLE_STATUS;

WiFiServer server(80);//********

void setup() {
  // initialize serial:
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  else {
    server.begin();
    Serial.print("Connected to wifi. My address:");
    IPAddress myAddress = WiFi.localIP();
    Serial.println(myAddress);

  }
}


void loop() {

}
 
```

### begin( ) 启动服务器

-   Description

告诉 server 开始监听传入连接

>   Tells the server to begin listening for incoming connections.

-   Syntax

*server*.begin()

-   Parameters

None

-   Returns

None

-   Example

```
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "lamaison";          //  your network SSID (name) 
char pass[] = "tenantaccess247";   // your network password
int status = WL_IDLE_STATUS;

WiFiServer server(80);

void setup() {
  // initialize serial:
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  else {
    server.begin();                           //**********
    Serial.print("Connected to wifi. My address:");
    IPAddress myAddress = WiFi.localIP();
    Serial.println(myAddress);

  }
}


void loop() {

}
```

### available()

-   Description

获取连接到服务器并具有可读数据的数据的 client。 当返回的客户端对象超出范围时，连接仍然存在; 你可以通过调用 *client*.stop() 关闭它。

available() 从Stream实用程序类继承。

>   Gets a client that is connected to the server and has data available for reading. The connection persists when the returned client object goes out of scope; you can close it by calling *client*.stop().
>
>   available() inherits from the [Stream](https://www.arduino.cc/en/Reference/Stream) utility class.

-   Syntax

*server*.available()

-   Parameters

None

-   Returns

一个客户端对象；如果没有客户端有可用于读取的数据，则此对象将在 if 语句中求值为 false。

>   a Client object; if no Client has data available for reading, this object will evaluate to false in an if-statement

-   Example

```
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "Network";          //  your network SSID (name) 
char pass[] = "myPassword";   // your network password
int status = WL_IDLE_STATUS;

WiFiServer server(80);

void setup() {
  // initialize serial:
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  else {
    server.begin();
    Serial.print("Connected to wifi. My address:");
    IPAddress myAddress = WiFi.localIP();
    Serial.println(myAddress);

  }
}

void loop() {
  // listen for incoming clients
  WiFiClient client = server.available();
  if (client) {

    if (client.connected()) {
      Serial.println("Connected to client");
    }

    // close the connection:
    client.stop();
  }
}
```

### write()

-   Description

Write data to all the clients connected to a server.

-   Syntax

*server*.write(data)

-   Parameters

**data**: the value to write (byte or char)

-   Returns

byte : the number of bytes written. It is not necessary to read this.

-   Example

```
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "yourNetwork";
char pass[] = "yourPassword";
int status = WL_IDLE_STATUS;

WiFiServer server(80);

void setup() {
  // initialize serial:
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  else {
    server.begin();
  }
}

void loop() {
  // listen for incoming clients
  WiFiClient client = server.available();
  if (client == true) {
       // read bytes from the incoming client and write them back
    // to any clients connected to the server:
    server.write(client.read());
  }
```

### print()

-   Description

将数据打印到连接到服务器的所有客户端。 以数字序列打印数字，每个ASCII字符（例如，数字123作为三个字符'1'，'2'，'3'发送）。

>   Print data to all the clients connected to a server. Prints numbers as a sequence of digits, each an ASCII character (e.g. the number 123 is sent as the three characters '1', '2', '3').

-   Syntax

*server*.print(data) 
*server*.print(data, BASE)

-   Parameters

data: the data to print (char, byte, int, long, or string)

BASE (optional): the base in which to print numbers: BIN for binary (base 2), DEC for decimal (base 10), OCT for octal (base 8), HEX for hexadecimal (base 16).

-   Returns

byte print() will return the number of bytes written, though reading that number is optional

### println()

-   Description

Prints data, followed by a newline, to all the clients connected to a server. Prints numbers as a sequence of digits, each an ASCII character (e.g. the number 123 is sent as the three characters '1', '2', '3').

-   Syntax

*server*.println() 
*server*.println(data) 
*server*.println(data, BASE)

-   Parameters

data (optional): the data to print (char, byte, int, long, or string)

BASE (optional): the base in which to print numbers: DEC for decimal (base 10), OCT for octal (base 8), HEX for hexadecimal (base 16).

-   Returns

byte
println() will return the number of bytes written, though reading that number is optional



