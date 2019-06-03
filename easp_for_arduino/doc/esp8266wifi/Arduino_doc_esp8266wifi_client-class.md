[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**client-class.md**

| title                    |
| ------------------------ |
| ESP8266WiFi Client Class |

[ESP8266WiFi Library](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#client)

## Client Class

[TOC]

Arduino 中的 [Client](https://www.arduino.cc/en/Reference/WiFiClientConstructor) 的相关方法： 

>   Methods documented for [Client](https://www.arduino.cc/en/Reference/WiFiClientConstructor) in [Arduino](https://github.com/arduino/Arduino)

1.  [WiFiClient()](https://www.arduino.cc/en/Reference/WiFiClient)
2.  [connected()](https://www.arduino.cc/en/Reference/WiFiClientConnected)
3.  [connect()](https://www.arduino.cc/en/Reference/WiFiClientConnect)
4.  [write()](https://www.arduino.cc/en/Reference/WiFiClientWrite)
5.  [print()](https://www.arduino.cc/en/Reference/WiFiClientPrint)
6.  [println()](https://www.arduino.cc/en/Reference/WiFiClientPrintln)
7.  [available()](https://www.arduino.cc/en/Reference/WiFiClientAvailable)
8.  [read()](https://www.arduino.cc/en/Reference/WiFiClientRead)
9.  [flush()](https://www.arduino.cc/en/Reference/WiFiClientFlush)
10.  [stop()](https://www.arduino.cc/en/Reference/WiFIClientStop)



下面进一步描述 ESP8266 所特有方法和属性。 它们不包括在Arduino WiFi库文档中。 在完整记录之前，请参考以下信息。

>   Methods and properties described further down are specific to ESP8266. They are not covered in [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) documentation. Before they are fully documented please refer to information below.

### setNoDelay( )

```
setNoDelay(nodelay)
```

将 `nodelay` 设置为 `true` ，此函数将禁用 Nagle 算法。

此算法旨在通过组合多个小的输出消息并立即发送它们，来减少通过网络发送的小分组的 TCP/IP 流量。 这种方法的缺点是有效地延迟单个消息，直到组装足够大的分组。

>   With `nodelay` set to `true`, this function will to disable [Nagle algorithm](https://en.wikipedia.org/wiki/Nagle%27s_algorithm).
>
>   This algorithm is intended to reduce TCP/IP traffic of small packets sent over the network by combining a number of small outgoing messages, and sending them all at once. The downside of such approach is effectively delaying individual messages until a big enough packet is assembled.
>

*Example:*

```
clinet.setNoDelay(true);
```

### Other Function Calls

```
uint8_t  status () 
virtual size_t  write (const uint8_t *buf, size_t size) 
size_t  write_P (PGM_P buf, size_t size) 
size_t  write (Stream &stream) 
size_t  write (Stream &stream, size_t unitSize) __attribute__((deprecated)) 
virtual int  read (uint8_t *buf, size_t size) 
virtual int  peek () 
virtual size_t  peekBytes (uint8_t *buffer, size_t length) 
size_t  peekBytes (char *buffer, size_t length) 
virtual  operator bool () 
IPAddress  remoteIP () 
uint16_t  remotePort () 
IPAddress  localIP () 
uint16_t  localPort () 
bool  getNoDelay () 
```

上述功能的文档尚未准备好。

有关代码示例，请参阅专门针对 Client Class 的示例的单独部分。

>   Documentation for the above functions is not yet prepared.
>
>   For code samples please refer to separate section with [examples :arrow_right:](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/client-examples.md) dedicated specifically to the Client Class.

## Arduino Client class

client 类创建可以连接到服务器并发送和接受数据的客户端。

>   The client class creates clients that can connect to servers and send and receive data.

[WiFi](https://www.arduino.cc/en/Reference/WiFi) : *Client* class

### Client( ) 客户端 

Description

客户端是所有 WiFi 客户端的基础调用的 base class。它不会被直接调用，而是在使用依赖它的函数时调用。

>   Client is the base class for all WiFi client based calls. It is not called directly, but invoked whenever you use a function that relies on it.



### WiFiClient()

Description

创建可以连接到指定 internet IP 地址和端口客户端，在  [client.connect()](https://www.arduino.cc/en/Reference/WiFiClientConnect) 中定义。

>   Creates a client that can connect to to a specified internet IP address and port as defined in [client.connect()](https://www.arduino.cc/en/Reference/WiFiClientConnect).

Syntax

WiFiClient()

Parameters

none

Example

```c
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "myNetwork";    //  your network SSID (name) 
char pass[] = "myPassword";   // your network password

int status = WL_IDLE_STATUS;
IPAddress server(74,125,115,105);  // Google

// Initialize the client library
WiFiClient client;

void setup() {
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    // don't do anything else:
    while(true);
  } 
  else {
    Serial.println("Connected to wifi");
    Serial.println("\nStarting connection...");
    // if you get a connection, report back via serial:
    // 如果你连接都了 wifi，通过串口回报信息。
    if (client.connect(server, 80)) {
      Serial.println("connected");
      // Make a HTTP request:
      client.println("GET /search?q=arduino HTTP/1.0");
      client.println();
    }
  }
}

void loop() {

}
```

### connected()

Description

是否连接客户端。 请注意，如果连接已关闭但仍有未读数据，则认为客户端已连接。

>   Whether or not the client is connected. Note that a client is considered connected if the connection has been closed but there is still unread data.

Syntax

*client*.connected()

Parameters

none

Returns

>   Returns true if the client is connected, false if not.

Example

```c
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "myNetwork";          //  your network SSID (name) 
char pass[] = "myPassword";   // your network password

int status = WL_IDLE_STATUS;
IPAddress server(74,125,115,105);  // Google

// Initialize the client library
WiFiClient client;

void setup() {
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    // don't do anything else:
    while(true);
  } 
  else {
    Serial.println("Connected to wifi");
    Serial.println("\nStarting connection...");
    // if you get a connection, report back via serial:
    if (client.connect(server, 80)) {
      Serial.println("connected");
      // Make a HTTP request:
      client.println("GET /search?q=arduino HTTP/1.0");
      client.println();
    }
  }
}

void loop() {
   if (client.available()) {
    char c = client.read();
    Serial.print(c);
  }

  if (!client.connected()) {
    Serial.println();
    Serial.println("disconnecting.");
    client.stop();
    for(;;)
      ;
  }
}
```

### connect()

Description

在构造函数中连接到指定 IP 地址和端口。返回值表示成功或失败。当使用域名时 (ex:google.com)，connect() 也提供 DNS 查找。

>   Connect to the IP address and port specified in the constructor. The return value indicates success or failure. connect() also supports DNS lookups when using a domain name (ex:google.com).

Syntax

*client*.connect(ip, port)
*client*.connect(URL, port)



Parameters

ip: the IP address that the client will connect to (array of 4 bytes)

URL: the domain name the client will connect to (string, ex.:"arduino.cc")

port: the port that the client will connect to (int)

Returns

Returns true if the connection succeeds, false if not.

Example

```c
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "myNetwork";          //  your network SSID (name) 
char pass[] = "myPassword";   // your network password

int status = WL_IDLE_STATUS;
char servername[]="google.com";  // remote server we will connect to

WiFiClient client;

void setup() {
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    // don't do anything else:
    while(true);
  } 
  else {
    Serial.println("Connected to wifi");
    Serial.println("\nStarting connection...");
    // if you get a connection, report back via serial:
    if (client.connect(servername, 80)) {
      Serial.println("connected");
      // Make a HTTP request:
      client.println("GET /search?q=arduino HTTP/1.0");
      client.println();
    }
  }
}

void loop() {

}
```

### write()

Description

将数据写入客户端连接的服务器。

>   Write data to the server the client is connected to.

Syntax

*client*.write(data)

Parameters

data: the byte or char to write

Returns

byte: the number of characters written. it is not necessary to read this value.
写入的字符数。 没有必要读取这个值。

### print()

Description

将数据打印到客户端连接的服务器。 以数字序列打印数字，每个ASCII字符（例如，数字 1 2 3 作为三个字符'1'，'2'，'3'发送）。

>   Print data to the server that a client is connected to. Prints numbers as a sequence of digits, each an ASCII character (e.g. the number 123 is sent as the three characters '1', '2', '3').

Syntax

*client*.print(data) 
*client*.print(data, BASE)

Parameters

data: the data to print (char, byte, int, long, or string)

BASE (optional): the base in which to print numbers:, DEC for decimal (base 10), OCT for octal (base 8), HEX for hexadecimal (base 16).其中要打印数字的基数:, DEC用于十进制（基本10），OCT用于八进制（基本8），HEX用于十六进制（基本16）。

Returns

byte : returns the number of bytes written, though reading that number is optional 
返回写入的字节数，通过读取该数字是可选的

### println()

Description

打印数据，后跟回车符和换行符，到客户端连接的服务器。 以数字序列打印数字，每个ASCII字符（例如，数字123作为三个字符'1'，'2'，'3'发送）。

>   Print data, followed by a carriage return and newline, to the server a client is connected to. Prints numbers as a sequence of digits, each an ASCII character (e.g. the number 123 is sent as the three characters '1', '2', '3').

Syntax

*client*.println() 
*client*.println(data) 
*client*.print(data, BASE)

Parameters

data (optional): the data to print (char, byte, int, long, or string)

BASE (optional): the base in which to print numbers: DEC for decimal (base 10), OCT for octal (base 8), HEX for hexadecimal (base 16).

Returns

byte: return the number of bytes written, though reading that number is optional

### available()

Description

返回可用于读取的字节数（即，由其连接的服务器写入客户端的数据量）。

>   Returns the number of bytes available for reading (that is, the amount of data that has been written to the client by the server it is connected to).

available() inherits from the [Stream](https://www.arduino.cc/en/Reference/Stream) utility class.

Syntax

*client*.available()

Parameters

none

Returns

The number of bytes available.

Example

```c
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "myNetwork";          //  your network SSID (name) 
char pass[] = "myPassword";   // your network password

int status = WL_IDLE_STATUS;
char servername[]="google.com";  // Google

WiFiClient client;

void setup() {
  Serial.begin(9600);
  Serial.println("Attempting to connect to WPA network...");
  Serial.print("SSID: ");
  Serial.println(ssid);

  status = WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    // don't do anything else:
    while(true);
  } 
  else {
    Serial.println("Connected to wifi");
    Serial.println("\nStarting connection...");
    // if you get a connection, report back via serial:
    if (client.connect(servername, 80)) {
      Serial.println("connected");
      // Make a HTTP request:
      client.println("GET /search?q=arduino HTTP/1.0");
      client.println();
    }
  }
}

void loop() {
  // if there are incoming bytes available 
  // from the server, read them and print them:
  if (client.available()) {
    char c = client.read();
    Serial.print(c);
  }

  // if the server's disconnected, stop the client:
  if (!client.connected()) {
    Serial.println();
    Serial.println("disconnecting.");
    client.stop();

    // do nothing forevermore:
    for(;;)
      ;
  }
}
```

### stop()

Disconnect from the server

Syntax

*client*.stop()

Parameters

none

Returns

none

### flush()

丢弃已写入客户端但尚未读取的任何字节。

Discard any bytes that have been written to the client but not yet read.

flush() inherits from the [Stream](https://www.arduino.cc/en/Reference/Stream) utility class.

Syntax

*client*.flush()

Parameters

none

Returns

none

### read()

Read the next byte received from the server the client is connected to (after the last call to read()).

read() inherits from the [Stream](https://www.arduino.cc/en/Reference/Stream) utility class.

Syntax

*client*.read()

Parameters

none

Returns

The next byte (or character), or -1 if none is available.

See Also

-   [Stream.read()](https://www.arduino.cc/en/Reference/StreamRead)