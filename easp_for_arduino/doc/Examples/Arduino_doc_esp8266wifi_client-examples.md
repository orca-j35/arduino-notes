---
title: ESP8266WiFi Client Class - Sketch Examples
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**client-examples.md**

[ESP8266WiFi Library :back:](readme.md#client)


## Client 客户端

我们编写一个简单的客户端程序来访问单个网页，并将其内容显示在串行监视器上。 这是客户端访问服务器 API 以检索特定信息的典型操作。 例如，我们可能想联系 GitHub 的 API 来定期检查在 esp8266 / Arduino 存储库上报告的公开问题的数量。

>   Let's write a simple client program to access a single web page and display its contents on a serial monitor. This is typical operation performed by a client to access server's API to retrieve specific information. For instance we may want to contact GitHub's API to periodically check the number of open issues reported on [esp8266 / Arduino](https://github.com/esp8266/Arduino/issues) repository. 

[TOC]

### 简介 Introduction

这次我们将专注于检索由服务器发送的网页内容，以演示基本客户端的功能。 一旦您能够从服务器检索信息，您应该能够依据相应的短语提取所需的特定数据。

>   This time we are going to concentrate just on retrieving a web page contents sent by a server, to demonstrate basic client's functionality. Once you are able to retrieve information from a server, you should be able to phrase it and extract specific data you need.

### 连接到 Wi-Fi

Get Connected to Wi-Fi

我们应该首先将模块连接到接入点，以获得互联网的访问权限。 提供此功能的代码已经在“快速入门”一章中讨论过。 详情请参阅。

>   We should start with connecting the module to an access point to obtain an access to internet. The code to provide this functionality has been already discussed in chapter [Quick Start](readme.md#quick-start). Please refer to it for details.

### 选择服务器

Select a Server

一旦连接到网络，我们应该连接到特定的服务器。 该服务器的Web地址在 `host` 字符串中声明如下。

>   Once connected to the network we should connect to the specific server. Web address of this server is declared in `host` character string as below. 

```
const char* host = "www.example.com";
```
我选择了www.example.com域名，您可以选择任何其他域名。 只需检查您是否可以使用网络浏览器访问它。

>   I have selected `www.example.com` domain name and you can select any other. Just check if you can access it using a web browser.



![替代文字](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/client-example-domain.png)

### 实例化客户端 Instantiate the Client

现在我们要声明一个客户端，该客户端将于 host(server) 连接：

>   Now we should declare a client that will be contacting the host (server):

```
WiFiClient client;
```

### 连接到服务器

Get Connected to the Server

在下一行我们将连接到主机并检查连接结果。 注80，这是用于Web访问的标准端口号。

>   In next line we will connect to the host and check the connection result. Note `80`, that is the standard port number used for web access.

```
if (client.connect(host, 80))
{
  // we are connected to the host!
}
else
{
  // connection failure
}
```

### 请求数据

Request the Data

如果连接成功，我们应该发送请求主机提供我们需要的具体信息。 这是使用 HTTP GET 请求完成的，如以下行所示：

>   If connection is successful, we should send request the host to provide specific information we need. This is done using the [HTTP GET](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) request as in the following lines:

```
client.print(String("GET /") + " HTTP/1.1\r\n" +
             "Host: " + host + "\r\n" +
             "Connection: close\r\n" +
             "\r\n"
            );
```

### 从服务器读取回复

Read Reply from the Server

然后，我们的客户端依然保持连接时 (`while (client.connected())`, see below) ，我们可以逐行读取并打印出服务器的响应

>   Then, while connection by our client is still alive (`while (client.connected())`, see below) we can read line by line and print out server's response:

```
while (client.connected())
{
  if (client.available())
  {
    String line = client.readStringUntil('\n');
    Serial.println(line);
  }
}
```

内部 `if (client.available())` 检查是否有可用的服务器数据。 如果有，则将其进行打印。

一旦服务器发送所有请求的数据，它将断开连接，程序将退出 `while`循环。

>   The inner `if (client.available())` is checking if there are any data available from the server. If so, then they are printed out.
>
>   Once server sends all requested data it will disconnect and program will exit the `while` loop. 
>


### Now to the Sketch

完整草 sketch ，包括与服务器争用失败的情况，如下所示。

Complete sketch, including a case when contention to the server fails, is presented below.

```
#include <ESP8266WiFi.h>

const char* ssid = "********";
const char* password = "********";

const char* host = "www.example.com";


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
}


void loop()
{
  WiFiClient client;

  Serial.printf("\n[Connecting to %s ... ", host);
  if (client.connect(host, 80))
  {
    Serial.println("connected]");

    Serial.println("[Sending a request]");
    client.print(String("GET /") + " HTTP/1.1\r\n" +
                 "Host: " + host + "\r\n" +
                 "Connection: close\r\n" +
                 "\r\n"
                );

    Serial.println("[Response:]");
    while (client.connected())
    {
      if (client.available())
      {
        String line = client.readStringUntil('\n');
        Serial.println(line);
      }
    }
    client.stop();
    Serial.println("\n[Disconnected]");
  }
  else
  {
    Serial.println("connection failed!]");
    client.stop();
  }
  delay(5000);
}
```


### Test it Live

上传 sketch 模块并打开串行监视器。 您应该看到类似于下面所示的日志。

首先，建立Wi-Fi连接后，应该会看到确认，该客户端连接到服务器并发送请求：

>   Upload sketch the module and open serial monitor. You should see a log similar to presented below. 
>
>   First, after establishing Wi-Fi connection, you should see confirmation, that client connected to the server and send the request:
>

```
Connecting to sensor-net ........ connected

[Connecting to www.example.com ... connected]
[Sending a request]
```

然后，在获得请求后，服务器将首先响应一个header，header 表明了后面的信息类型 (e.g. `Content-Type: text/html`)，信息的长度 (like `Content-Length: 1270`)，等等

指定要跟踪的信息类型（例如Content-Type：text / html），它是多长时间（如Content-Length：1270）等。

>   Then, after getting the request, server will first respond with a header that specifies what type of information will follow (e.g. `Content-Type: text/html`), how long it is (like `Content-Length: 1270`), etc.:

```
[Response:]
HTTP/1.1 200 OK

Cache-Control: max-age=604800
Content-Type: text/html
Date: Sat, 30 Jul 2016 12:30:45 GMT
Etag: "359670651+ident"
Expires: Sat, 06 Aug 2016 12:30:45 GMT
Last-Modified: Fri, 09 Aug 2013 23:54:35 GMT
Server: ECS (ewr/15BD)
Vary: Accept-Encoding
X-Cache: HIT
x-ec-custom-error: 1
Content-Length: 1270
Connection: close
```

header 结尾标有空行，然后您应该看到所请求的网页的HTML代码。

>   End of header is marked with an empty line and then you should see the HTML code of requested web page.

```
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">

(...)

</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is established to be used for illustrative examples in documents. You may use this
    domain in examples without prior coordination or asking for permission.</p>
    <p><a href="http://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>

[Disconnected]
```

### Test it More

如果服务器的网址不正确或服务器无法访问，则应在串行监视器上看到以下简短短信：

>   In case server's web address is incorrect, or server is not accessible, you should see the following short and simple message on the serial monitor:

```
Connecting to sensor-net ........ connected

[Connecting to www.wrong-example.com ... connection failed!]
```


### 总结 Conclusion

通过这个简单的例子，我们演示了如何设置客户端程序，将其连接到服务器，请求网页并检索它。 现在您应该能够为ESP8266编写自己的客户端程序，并与服务器进行更高级的对话，例如 使用HTTPS协议与客户端安全。

有关更多的客户端示例，请检查

- [WiFiClientBasic.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiClientBasic/WiFiClientBasic.ino)  - 一个向TCP服务器发送消息的简单草图
- [WiFiClient.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiClient/WiFiClient.ino) - 此草图通过HTTP GET请求发送数据到data.sparkfun.com服务。

有关管理客户端的功能列表，请参阅客户端类[Client Class ​:arrow_right:​](client-class.md) 文档。

>   With this simple example we have demonstrated how to set up a client program, connect it to a server, request a web page and retrieve it. Now you should be able to write your own client program for ESP8266 and move to more advanced dialogue with a server, like e.g. using [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protocol with the [Client Secure](readme.md#client-secure). 
>
>   For more client examples please check
>   * [WiFiClientBasic.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiClientBasic/WiFiClientBasic.ino) - a simple sketch that sends a message to a TCP server
>     * [WiFiClient.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiClient/WiFiClient.ino) - this sketch sends data via HTTP GET requests to data.sparkfun.com service. 
>
>
>   For the list of functions provided to manage clients, please refer to the [Client Class :arrow_right:](client-class.md) documentation.