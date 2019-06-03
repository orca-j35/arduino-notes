---
title: ESP8266WiFi Server Class - Sketch Examples
---

[ESP8266WiFi Library :back:](readme.md#server)

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**server-examples.md**


## Server

在ESP8266上设置 Web 服务器需要很少的代码，并且非常简单。 这是由于多功能 ESP8266WiFi 库提供的功能。

此示例的目的是准备一个可以在Web浏览器中打开的网页。 该页面应显示ESP的模拟输入引脚的当前原始读数。

>   Setting up web a server on ESP8266 requires very little code and is surprisingly straightforward. This is thanks to functionality provided by the versatile ESP8266WiFi library.
>
>   The purpose of this example will be to prepare a web page that can be opened in a web browser. This page should show the current raw reading of ESP's analog input pin.
>

[TOC]

### The Object

我们将从创建服务器对象开始。

>   We will start off by creating a server object.

```
WiFiServer server(80);
```

服务器响应端口80上的客户端（在这种情况下是Web浏览器），这是一个标准的端口Web浏览器与Web服务器通话。

>   The server responds to clients (in this case - web browsers) on port 80, which is a standard port web browsers talk to web servers.


### The Page

然后让我们写一个简短的函数 `prepareHtmlPage()` ，它将返回一个包含网页内容的 `String` 类变量。 然后，我们将该变量传递给服务器，将其传递给客户端。

>   Then let's write a short function `prepareHtmlPage()`, that will return a `String` class variable containing the contents of the web page. We will then pass this variable to server to pass it over to a client.

```
String prepareHtmlPage()
{
  String htmlPage = 
     String("HTTP/1.1 200 OK\r\n") +
            "Content-Type: text/html\r\n" +
            "Connection: close\r\n" +  // the connection will be closed after completion of the response
            "Refresh: 5\r\n" +  // refresh the page automatically every 5 sec
            "\r\n" +
            "<!DOCTYPE HTML>" +
            "<html>" +
            "Analog input:  " + String(analogRead(A0)) +
            "</html>" +
            "\r\n";
  return htmlPage;
}
```

该功能没有什么花哨，只是将页面的文本标题和HTML内容放在一起。

>   The function does nothing fancy but just puts together a text header and [HTML](http://www.w3schools.com/html/) contents of the page. 


### Header First

header 通知客户什么类型的内容要遵循以及如何提供：

>   The header is to inform client what type of contents is to follow and how it will be served:

```
Content-Type: text/html
Connection: close
Refresh: 5
```
在我们的示例中，内容类型是 `text/html` ，连接将在服务后关闭，客户端每5秒再次请求一次内容。heard 以空行 `\r\n` 结束。 这是为了区分 header  和之后的内容。

>   In our example the content type is `text/html`, the connection will be closed after serving and the content should be requested by the client again every 5 seconds. The header is concluded with an empty line `\r\n`. This is to distinguish header from the content to follow. 

```
<!DOCTYPE HTML>
<html>
Analog input:  [Value]
</html>
```



>   The content contains two basic [HTML](http://www.w3schools.com/html/) tags, one to denote HTML document type `<!DOCTYPE HTML>` and another to mark beginning `<html>` and end `</html>` of the document. Inside there is a raw value read from ESP's analog input `analogRead(A0)` converted to the `String` type.

```
String(analogRead(A0))
```


### The Page is Served

web page 服务将在 `loop()` 中完成，其中服务器持续等待新的客户端连接并发送一些包含请求的数据。

>   Serving of this web page will be done in the `loop()` where server is waiting for a new client to connect and send some data containing a request:

```
void loop()
{
  WiFiClient client = server.available(); 
  if (client)
  {
    // we have a new client sending some request
  }
}
```

一旦连接了新的客户端，服务器将会读取客户端的请求并将其在串行监视器上打印出来。

>   Once a new client is connected, server will read the client's request and print it out on a serial monitor.

```
while (client.connected())
{
  if (client.available())
  {
    String line = client.readStringUntil('\r');
    Serial.print(line);
  }
}
```

来自客户端的请求标记有空白的新行。 如果我们找到这个标记，我们可以发回网页并退出 `while()` 循环使用 `break`。

>   Request from the client is marked with an empty new line. If we find this mark, we can send back the web page and exit `while()` loop using `break`.

```
if (line.length() == 1 && line[0] == '\n')
{
    client.println(prepareHtmlPage());
    break;
}
```

整个过程是通过停止与客户端的连接来得出的：

>   The whole process is concluded by stopping the connection with client:

```
client.stop();
```


### Put it Together

Complete sketch is presented below.

```
#include <ESP8266WiFi.h>

const char* ssid = "********";
const char* password = "********";

WiFiServer server(80);


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

  server.begin();
  Serial.printf("Web server started, open %s in a web browser\n", WiFi.localIP().toString().c_str());
}


// prepare a web page to be send to a client (web browser)
String prepareHtmlPage()
{
  String htmlPage = 
     String("HTTP/1.1 200 OK\r\n") +
            "Content-Type: text/html\r\n" +
            "Connection: close\r\n" +  // the connection will be closed after completion of the response
            "Refresh: 5\r\n" +  // refresh the page automatically every 5 sec
            "\r\n" +
            "<!DOCTYPE HTML>" +
            "<html>" +
            "Analog input:  " + String(analogRead(A0)) +
            "</html>" +
            "\r\n";
  return htmlPage;
}


void loop()
{
  WiFiClient client = server.available(); 
  // wait for a client (web browser) to connect
  if (client)
  {
    Serial.println("\n[Client connected]");
    while (client.connected())
    {
      // read line by line what the client (web browser) is requesting
      if (client.available())
      {
        String line = client.readStringUntil('\r');
        Serial.print(line);
        // wait for end of client's request, that is marked with an empty line
        if (line.length() == 1 && line[0] == '\n')
        {
          client.println(prepareHtmlPage());
          break;
        }
      }
    }
    delay(1); // give the web browser time to receive the data

    // close the connection:
    client.stop();
    Serial.println("[Client disonnected]");
  }
}
```

### Get it Run

在 sketch 中更新 `ssid` 和 `password` 以匹配您的接入点的凭据。 将草图加载到ESP模块并打开串行监视器。 首先，您应该看到确认连接到接入点和Web服务器的模块已启动。

>   Update `ssid` and `password` in sketch to match credentials of your access point. Load sketch to ESP module and open a serial monitor. First you should see confirmation that module connected to the access point and the web server started.

```
Connecting to sensor-net ........ connected
Web server started, open 192.168.1.104 in a web browser
```
在网络浏览器中输入提供的IP地址。 您应该看到由ESP8266提供的页面：

>   Enter provided IP address in a web browser. You should see the page served by ESP8266:

![alt文本](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/server-browser-output.png)

该页面将每 5 秒刷新一次。这样的情况每发生一次，你应该在串口监视器上看到来自客户端( 你的 web 浏览器 )的请求被打印.

页面将每5秒刷新一次。 每次发生这种情况，您应该看到从串行监视器上打印出来的客户端（您的Web浏览器）的请求：

>   The page would be refreshed every 5 seconds. Each time this happens, you should see a request from the client (your web browser) printed out on the serial monitor:

```
[Client connected]
GET / HTTP/1.1
Accept: text/html, application/xhtml+xml, */*
Accept-Language: en-US
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; rv:11.0) like Gecko
Accept-Encoding: gzip, deflate
Host: 192.168.1.104
DNT: 1
Connection: Keep-Alive
[client disonnected]
```

### What Else?

查看 [client examples](client-examples.md) ，您将很快找到与服务器的协议相似之处。 协议以 header 开始，hearder 包含有关通讯的信息。其包含了用于通讯或接受的类容的类型如 `text/html` 。在提交 hearder 后是否保持连接状态或是关闭。它包含发件人的标识，如 User-Agent：Mozilla / 5.0（Windows NT 6.1）等。

>   Looking on [client examples](client-examples.md) you will quickly find out the similarities in protocol to the server. The protocol starts with a header that contains information what communication will be  about. It contains what content type is communicated or accepted like `text/html`. It states whether connection will be kept alive or closed after submission of the header. It contains identification of the sender like `User-Agent: Mozilla/5.0 (Windows NT 6.1)`, etc.


### Conclusion

以上示例显示，ESP8266上的Web服务器几乎可以在任何时间内进行设置。 这样的服务器可以很容易地从诸如具有网络浏览器的PC 等功能更强大的硬件和软件上收集请求。 查看其他类，如 ESP8266WebServer，让您编程更高级的应用程序。

如果您想尝试另一个服务器示例，请查看WiFiWebServer.ino，它提供了通过Web浏览器切换GPIO引脚的功能。

有关实现和管理服务器的功能列表，请参阅服务器类：arrow_right：文档。

>   The above example shows that a web server on ESP8266 can be set up in almost no time. Such server can easily stand up requests from much more powerful hardware and software like a PC with a web browser. Check out other classes like [ESP8266WebServer](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WebServer) that let you program more advanced applications.
>
>   If you like to try another server example, check out [WiFiWebServer.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiWebServer/WiFiWebServer.ino), that provides functionality of toggling the GPIO pin on and off out of a web browser.
>
>
>   For the list of functions provided to implement and manage servers, please refer to the [Server Class :arrow_right:](server-class.md) documentation.
>