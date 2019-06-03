# [Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**readme.md**

[TOC]

| title               |
| ------------------- |
| ESP8266WiFi Library |

ESP8266 总与 Wi-Fi 相关。 如果您渴望将新的 ESP8266 模块连接到Wi-Fi网络以开始发送和接收数据，这是一个很好的起点。 如果您正在寻找更多关于如何编程特定 Wi-Fi 网络功能的深度细节，您也在正确的地方。

>   ESP8266 is all about Wi-Fi. If you are eager to connect your new ESP8266 module to Wi-Fi network to start sending and receiving data, this is a good place to start. If you are looking for more in depth details of how to program specific Wi-Fi networking functionality, you are also in the right place.



## Introduction

[ESP8266 的 Wi-Fi 库](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) 是基于 [ESP8266 SDK](http://bbs.espressif.com/viewtopic.php?f=51&t=1023) 开发的，使用 [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) 库的命名约定和总体功能理念。 随着时间的推移，丰富的 Wi-Fi 功能从 ESP9266 SDK 被移植到了[esp8266 / Adruino](https://github.com/esp8266/Arduino) ，对 [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) 进行了扩展，因此我们显然需要提供新的文档，以展示新的功能和额外的扩展。

本文档将向您介绍 ESP8266WiFi 库的几个类，方法和属性。 如果你刚接触的 C ++ 和 Arduino，不要担心。 我们将从一般概念开始，然后在转向到每个特定类的成员的详细描述，包括使用示例。

ESP8266WiFi 库提供的功能范围相当广泛，因此本说明已分解成单独的文档标记为！! ==>

>   The [Wi-Fi library for ESP8266](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) has been developed basing on [ESP8266 SDK](http://bbs.espressif.com/viewtopic.php?f=51&t=1023), using naming convention and overall functionality philosophy of [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi). Over time the wealth Wi-Fi features ported from ESP9266 SDK to [esp8266 / Adruino](https://github.com/esp8266/Arduino) outgrow [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) and it became apparent that we need to provide separate documentation on what is new and extra.
>
>   This documentation will walk you through several classes, methods and properties of [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) library. If you are new to C++ and Arduino, don't worry. We will start from general concepts and then move to detailed description of members of each particular class including usage examples.
>
>   The scope of functionality offered by [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) library is quite extensive and therefore this description has been broken up into separate documents marked with ! :arrow_right:

### Quick Start 快速开始

希望你已经熟悉如何加载Blink.ino草图到ESP8266模块，并获得LED闪烁。 如果没有，请检查本教程由Adafruit或Sparkfun开发的另一个伟大的教程。

希望你已熟知如何将 [Blink.ino](https://github.com/esp8266/Arduino/blob/master/libraries/esp8266/examples/Blink/Blink.ino) sketch 加载到 ESP8266 模块，以使得 LED 闪烁。如果你并不熟悉，请查看Adafruit 的 [这个教程](https://learn.adafruit.com/adafruit-huzzah-esp8266-breakout/using-arduino-ide) (该教程load sketch 时需要手动引导)， [还可以参考这个连接](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide) (有引导程序)，或者参阅由 sparkfun 开发的 [另一个非常棒的教程](https://learn.sparkfun.com/tutorials/esp8266-thing-hookup-guide/introduction)。    [还可以参考这个连接](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide) ， [和这个连接](https://learn.sparkfun.com/tutorials/esp8266-thing-development-board-hookup-guide/all)

要将 ESP 模块连接到 Wi-Fi ( 如将手机连接到热点 )，你只需要几行代码：

>   Hopefully you are already familiar how to load [Blink.ino](https://github.com/esp8266/Arduino/blob/master/libraries/esp8266/examples/Blink/Blink.ino) sketch to ESP8266 module and get the LED blinking. If not, please check [this tutorial](https://learn.adafruit.com/adafruit-huzzah-esp8266-breakout/using-arduino-ide) by Adafruit or [another great tutorial](https://learn.sparkfun.com/tutorials/esp8266-thing-hookup-guide/introduction) developed by Sparkfun.
>
>   To hook up ESP module to Wi-Fi (like hooking up a mobile phone to a hot spot), you need just couple of lines of code:

```
#include <ESP8266WiFi.h>

void setup()
{
  Serial.begin(115200);
  Serial.println();

  WiFi.begin("network-name", "pass-to-network");

  Serial.print("Connecting");
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();

  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());
}

void loop() {}
```

将 `WiFi.begin("network-name", "pass-to-network")` 中的 `network-name` 和 `pass-to-network` 替换为你想要连接的 Wi-Fi 网络的名称和密码。然后将此草图上传到 ESP 模块并打开串口监视器。你应该会看到如下内容：

>   In the line `WiFi.begin("network-name", "pass-to-network")` replace `network-name` and `pass-to-network` with name and password to the Wi-Fi network you like to connect. Then upload this sketch to ESP module and open serial monitor. You should see something like:

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/wifi-simple-connect-terminal.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/wifi-simple-connect-terminal.png)

工作的原理是什么喃？sketch 的第一行 `#include <ESP8266WiFi.h>` ，表示我们向该 sketch 中添加了 [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) 库。此库提供 ESP8266 特定的 Wi-Fi 程序，我们可对其进行调用，以连接到网络。

>   How does it work? In the first line of sketch `#include <ESP8266WiFi.h>` we are including [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) library. This library provides ESP8266 specific Wi-Fi routines we are calling to connect to network.

通过以下调用方式初始化与 Wi-Fi 实际连接。

>   Actual connection to Wi-Fi is initialized by calling:

```
WiFi.begin("network-name", "pass-to-network");
```

连接过程可能需要几秒钟，我们正在检查这个完成在以下循环：

>   Connection process can take couple of seconds and we are checking for this to complete in the following loop:

```
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
```

当 `WiFi.status()` 不等于 `WL_CONNECTED` 时，while() 将保持循环。 只有当状态更改为 `WL_CONNECTED` 时，循环才会退出。

>   The `while()` loop will keep looping while `WiFi.status()` is other than `WL_CONNECTED`. The loop will exit only if the status changes to `WL_CONNECTED`.

最后一行将打印出由 [DHCP](http://whatismyipaddress.com/dhcp) 分配给ESP模块的IP地址：

>   The last line will then print out IP address assigned to ESP module by [DHCP](http://whatismyipaddress.com/dhcp):

```
Serial.println(WiFi.localIP());
```

如果你没有看到最后一行，但只是越来越多的点 `.........`，那么可能的名称或Wi-Fi网络的草图中的密码输入不正确。通过从头开始连接到 PC 或移动电话到此 Wi-Fi，验证名称和密码。

注意：如果连接以建立，然后由于某种原因丢失，ESP将自动重新连接到最后使用的接入点，一旦它再次回到在线。 这将由Wi-Fi库自动完成，无需任何用户干预。

这是你需要将 ESP8266 连接到 Wi-Fi 所做的一切。 在以下章节中，我们将解释 ESP 连接后还可做完成哪些更酷的事。

>   If you don't see the last line but just more and more dots `.........`, then likely name or password to the Wi-Fi network in sketch is entered incorrectly. Verify name and password by connecting from scratch to this Wi-Fi a PC or a mobile phone.
>
>   *Note:* if connection is established, and then lost for some reason, ESP will automatically reconnect to last used access point once it is again back on-line. This will be done automatically by Wi-Fi library, without any user intervention.
>
>   That's all you need to connect ESP8266 to Wi-Fi. In the following chapters we will explain what cool things can be done by ESP once connected.

### Who is Who 区别站点和 AP

连接到 Wi-Fi 网络的站点被称为 **stations(STA)**。接入点**(AP)** 提供 Wi-Fi 连接，AP 其充当一个或多个 stations 的集线器。接入点的另一端连接到有线网络。 接入点通常与路由器集成以提供从 Wi-Fi 网络到因特网的接入。 每个接入点由 SSID ( **S**ervice **S**et **ID**entifier 服务集标识符 ) 识别，SSID 实质上是在将 device (station) 连接到 Wi-Fi 网络时，所选择的网络名称。

ESP8266 模块可以作为一个站点，所以我们可以连接到 Wi-Fi 网络。 它也可以作为软接入点 **(soft-AP)**，建立自己的Wi-Fi网络。 因此，我们可以将其他站连接到这样的 ESP 模块。 ESP8266 还能够在**工作站和软接入点模式**下工作。 这提供了建立例如 网状网络 [mesh networks](https://en.wikipedia.org/wiki/Mesh_networking) 的可能。

>   Devices that connect to Wi-Fi network are called stations (STA). Connection to Wi-Fi is provided by an access point (AP), that acts as a hub for one or more stations. The access point on the other end is connected to a wired network. An access point is usually integrated with a router to provide access from Wi-Fi network to the internet. Each access point is recognized by a SSID (**S**ervice **S**et **ID**entifier), that essentially is the name of network you select when connecting a device (station) to the Wi-Fi.
>
>   ESP8266 module can operate as a station, so we can connect it to the Wi-Fi network. It can also operate as a soft access point (soft-AP), to establish its own Wi-Fi network. Therefore we can connect other stations to such ESP module. ESP8266 is also able to operate both in station and soft access point mode. This provides possibility of building e.g. [mesh networks](https://en.wikipedia.org/wiki/Mesh_networking).

![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-station-soft-access-point.png)

 [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi)  库提供了丰富的 C++  [methods](https://en.wikipedia.org/wiki/Method_(computer_programming)) (functions) and [properties](https://en.wikipedia.org/wiki/Property_(programming)) ，可在 station and / or soft access point 模式下配置和操作ESP8266模块。 它们在以下章节中描述。

>   The [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) library provides wide collection of C++ [methods](https://en.wikipedia.org/wiki/Method_(computer_programming)) (functions) and [properties](https://en.wikipedia.org/wiki/Property_(programming)) to configure and operate an ESP8266 module in station and / or soft access point mode. They are described in the following chapters.

## Class Description 类描述

ESP8266WiFi 库分为几个类。在大多数情况下，当编写代码时，用户不关心这种分类。 我们使用它来将这个库的描述分解成更易于管理的部分。

>   The [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) library is broken up into several classes. In most of cases, when writing the code, user is not concerned with this classification. We are using it to break up description of this library into more manageable pieces.

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/doxygen-class-index.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/doxygen-class-index.png)

下面的章节描述了 ESP8266WiFi 特定类中列出的所有函数调用 ([methods](https://en.wikipedia.org/wiki/Method_(computer_programming)) and [properties](https://en.wikipedia.org/wiki/Property_(programming)) in C++ 术语) 。 将通过应用程序示例和代码段来说明如何在实践中使用函数的说明。 这些信息大部分分解成单独的文档。 请按照！ ==>访问它们。

>   Chapters below describe all function calls ([methods](https://en.wikipedia.org/wiki/Method_(computer_programming)) and [properties](https://en.wikipedia.org/wiki/Property_(programming)) in C++ terms) listed in particular classes of [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi). Description is illustrated with application examples and code snippets to show how to use functions in practice. Most of this information is broken up into separate documents. Please follow ! **==>**  access them.



### Station 站点

站点(STA) 模式用于使ESP模块连接到由接入点 AP 建立的 Wi-Fi 网络。

>   Station (STA) mode is used to get ESP module connected to a Wi-Fi network established by an access point.

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-station.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/esp8266-station.png)

Staton 类有几个功能，以方便管理 Wi-Fi 连接。 如果连接丢失，ESP8266 将自动重新连接到最后使用的接入点，一旦它再次可用。 模块重新启动时也会发生同样的情况。 这是可能的，因为ESP 将最后使用的接入点的凭据，保存到闪存（非易失性）存储器中。使用保存的数据，如果 sketch 已更改，但代码不会更改Wi-Fi 模式或凭据，ESP 也将重新连接。

[Station Class documentation ==> : [begin](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#begin) | [config](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#config) | [reconnect](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#reconnect) | [disconnect](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#disconnect) | [isConnected](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#isconnected) | [setAutoConnect](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#setautoconnect) | [getAutoConnect](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#getautoconnect) | [setAutoReconnect](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#setautoreconnect) | [waitForConnectResult](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#waitforconnectresult) | [macAddress](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#macAddress) | [localIP](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#localip) | [subnetMask](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#subnetmask) | [gatewayIP](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#gatewayip) | [dnsIP](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#dnsip) | [hostname](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#hostname) | [status](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#status) | [SSID](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#ssid) | [psk](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#psk) | [BSSID](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#bssid) | [RSSI](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#rssi) | [WPS](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#wps) | [Smart Config](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#smart-config)

Check out separate section with [examples ](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-examples.md)! ==>

>   Station class has several features to facilitate management of Wi-Fi connection. In case the connection is lost, ESP8266 will automatically reconnect to the last used access point, once it is again available. The same happens on module reboot. This is possible since ESP is saving credentials to last used access point in flash (non-volatile) memory. Using the saved data ESP will also reconnect if sketch has been changed but code does not alter the Wi-Fi mode or credentials.



### Soft Access Point 软接入点

[access point (AP)](https://en.wikipedia.org/wiki/Wireless_access_point) 用于提供 devices (stations) 对 Wi-Fi 网络的访问设备，并可将 stations 进一步连接到有线网络。ESP8266 可以提供类似的功能，除了它没有到有线网络的接口。 这种操作模式被称为软接入点（soft-AP）。 连接到软AP的站的最大数量为 **5**。

>   An [access point (AP)](https://en.wikipedia.org/wiki/Wireless_access_point) is a device that provides access to Wi-Fi network to other devices (stations) and connects them further to a wired network. ESP8266 can provide similar functionality except it does not have interface to a wired network. Such mode of operation is called soft access point (soft-AP). The maximum number of stations connected to the soft-AP is five.

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-soft-access-point.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/esp8266-soft-access-point.png)

soft-AP 模式常被用作以 station模式将 ESP 连接到 Wi-Fi 的中间步骤。在 ESP 模块事先并不知道网络的 SSID 和 密码时。ESP 首先以 soft-AP 模式启动，因此我们可以使用笔记本电脑或移动电话连接到该 ESP。 然后，我们能够向目标网络提供凭据。 一旦完成，ESP切换到站模式，并可连接到目标Wi-Fi。

 soft-AP 模式的另一个方便的应用是建立网状网络。 ESP可以在 soft-AP 和站模式中运行，因此它可以充当网状网络的节点。

[Soft Access Point Class documentation ](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-class.md): [softAP](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-class.md#softap) | [softAPConfig](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-class.md#softapconfig) | [softAPdisconnect](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-class.md#softapdisconnect) | [softAPgetStationNum](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-class.md#softapgetstationnum) | [softAPIP](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-class.md#softapip) | [softAPmacAddress](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-class.md#softapmacaddress)

Check out separate section with  [examples ](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-examples.md)

>The soft-AP mode is often used an intermediate step before connecting ESP to a Wi-Fi in a station mode. This is when SSID and password to such network is not known upfront. ESP first boots in soft-AP mode, so we can connect to it using a laptop or a mobile phone. Then we are able to provide credentials to the target network. Once done ESP is switched to the station mode and can connect to the target Wi-Fi.
>
>Another handy application of soft-AP mode is to set up [mesh networks](https://en.wikipedia.org/wiki/Mesh_networking). ESP can operate in both soft-AP and Station mode so it can act as a node of a mesh network.

### Scan 扫描

要将手机连接到热点，您通常需要打开Wi-Fi设置应用，列出可用的网络并选择所需的热点。 然后输入你所需网络的密码（或不输入）。您可以对ESP执行相同操作。 扫描并列出一定范围内可用网络的功能，由 Scan 类实现。

[Scan Class documentation](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md) : [scanNetworks](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#scannetworks) | [scanNetworksAsync](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#scannetworksasync) | [scanComplete](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#scancomplete) | [scanDelete](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#scandelete) | [SSID](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#ssid) | [encryptionType](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#encryptiontype) | [BSSID](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#bssid) | [BSSIDstr](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#bssidstr) | [channel](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#channel) | [isHidden](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#ishidden) | [getNetworkInfo](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-class.md#getnetworkinfo)

Check out separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-examples.md)

>   To connect a mobile phone to a hot spot, you typically open Wi-Fi settings app, list available networks and pick the hot spot you need. Then enter a password (or not) and you are in. You can do the same with ESP. Functionality of scanning for, and listing of available networks in range is implemented by the Scan Class.

### Client 客户端

Client 类创建的客户端可访问服务器提供的服务，以便发送、接受和处理数据。

>   The Client class creates [clients](https://en.wikipedia.org/wiki/Client_(computing)) that can access services provided by [servers](https://en.wikipedia.org/wiki/Server_(computing)) in order to send, receive and process data.

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-client.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/esp8266-client.png)

Check out separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/client-examples.md) / [list of functions ](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/client-class.md)

### Client Secure 客户端安全

客户端安全是 [Client Class](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#client) 的扩展，其中使用 [secure protocol](https://en.wikipedia.org/wiki/Transport_Layer_Security) 与服务器进行连接和数据交换。 它支持TLS 1.1。 不支持TLS 1.2。

>   The Client Secure is an extension of [Client Class](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#client) where connection and data exchange with servers is done using a [secure protocol](https://en.wikipedia.org/wiki/Transport_Layer_Security). It supports [TLS 1.1](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.1). The [TLS 1.2](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.2) is not supported.

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-client-secure.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/esp8266-client-secure.png)

由于需要运行加密算法，安全应用程序具有额外的内存（和处理）开销。 证书的密钥越强，需要的开销越大。 在实践中，不可能一次运行多于一个安全客户端。 这个问题涉及到我们无法添加的内存，闪存的大小通常不是问题。 如果你想了解如何开发 [client secure library](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/WiFiClientSecure.h) ，访问已经过测试的服务器，以及内存限制如何克服，阅读令人着迷的问题报告 [#43](https://github.com/esp8266/Arduino/issues/43)。

Check out separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/client-secure-examples.md) / [list of functions](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/client-secure-class.md)

>   Secure applications have additional memory (and processing) overhead due to the need to run cryptography algorithms. The stronger the certificate's key, the more overhead is needed. In practice it is not possible to run more than a single secure client at a time. The problem concerns RAM memory we can not add, the flash memory size is usually not the issue. If you like to learn how [client secure library](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/WiFiClientSecure.h) has been developed, access to what servers have been tested, and how memory limitations have been overcame, read fascinating issue report [#43](https://github.com/esp8266/Arduino/issues/43).

### Server 服务器

服务器类创建向其他程序或设备（称为 [clients](https://en.wikipedia.org/wiki/Client_(computing))）提供功能的 [servers](https://en.wikipedia.org/wiki/Server_(computing)) 。

>   The Server Class creates [servers](https://en.wikipedia.org/wiki/Server_(computing)) that provide functionality to other programs or devices, called [clients](https://en.wikipedia.org/wiki/Client_(computing)).

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-server.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/esp8266-server.png)

客户端连接到服务器以发送和接收数据并访问提供的功能。

Check out separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/server-examples.md) / [list of functions ](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/server-class.md)

>   Clients connect to sever to send and receive data and access provided functionality.



### UDP

UDP 类使能发送和接收 [User Datagram Protocol (UDP)](https://en.wikipedia.org/wiki/User_Datagram_Protocol) 消息。 UDP使用简单的 "fire and forget" 传输模型，不保证传递，排序或重复保护。 UDP 提供用于数据完整性的校验和端口号，以便在数据报的源和目标处访问不同的功能。

查看各章的 [示例](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/udp-examples.md) / [函数列表](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/udp-class.md)

>   The UDP Class enables the [User Datagram Protocol (UDP)](https://en.wikipedia.org/wiki/User_Datagram_Protocol) messages to be sent and received. The UDP uses a simple "fire and forget" transmission model with no guarantee of delivery, ordering, or duplicate protection. UDP provides checksums for data integrity, and port numbers for addressing different functions at the source and destination of the datagram.
>
>   Check out separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/udp-examples.md) / [list of functions](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/udp-class.md)

### Generic 通用

有几个函数由 ESP8266 的  [SDK](http://bbs.espressif.com/viewtopic.php?f=51&t=1023) 提供， [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) 中不包含这些功能。假如你所需要的函数并不符合于之前所讨论的任何类，那么他有可能在**通用类**中。其中包括管理 Wi-Fi 事件的处理程序，如连接、断开连接、获取 IP、更改 Wi-Fi 模式、管理模块睡眠模式的的函数、主机名到 IP 地址的解析等等。

>   There are several functions offered by ESP8266's [SDK](http://bbs.espressif.com/viewtopic.php?f=51&t=1023) and not present in [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi). If such function does not fit into one of classes discussed above, it will likely be in Generic Class. Among them is handler to manage Wi-Fi events like connection, disconnection or obtaining an IP, Wi-Fi mode changes, functions to manage module sleep mode, hostname to an IP address resolution, etc.

Check out separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/generic-examples.md) / [list of functions ](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/generic-class.md)

## Diagnostics 诊断

有这种技术手段可用于诊断及解决获取 Wi-Fi 连接和保持连接的问题。

>   There are several techniques available to diagnose and troubleshoot issues with getting connected to Wi-Fi and keeping connection alive.

### Check Return Codes 检查返回代码

上面章节中所讲解的函数，几乎每个都会返回一些诊断信息。

“诊断” 可以提供可以提供简单的 `boolean` 类型( `true` 或 `false` ) 以表示运算结果。你可以按照示例中所描述的方式检查这些结果。例如：

>   Almost each function described in chapters above returns some diagnostic information.
>
>   Such diagnostic may be provided as a simple `boolean` type `true` or`false` to indicate operation result. You may check this result as described in examples, for instance:

```
Serial.printf("Wi-Fi mode set to WIFI_STA %s\n", WiFi.mode(WIFI_STA) ? "" : "Failed!");
```

另一些函数可以提供比二进制状态更加丰富的信息。`Wi-Fi.status()` 就是一个很好示例。

>   Some functions provide more than just a binary status information. A good example is `WiFi.status()`.

```
Serial.printf("Connection status: %d\n", WiFi.status());
```

该函数返回以下代码用于描述正在进行的 Wi-Fi 连接：

-   0 : `WL_IDLE_STATUS` 当 Wi-Fi 处于两种状态的转换过程中时
-   1 : `WL_NO_SSID_AVAIL` 已被配置值的 SSID 无法访问的情况
-   3 : `WL_CONNECTED` 在成功连接后被建立 
-   4 : `WL_CONNECT_FAILED` 如果密码不正确
-   6 : `WL_DISCONNECTED` 如果模块在站点模式下未被配置

显示和检查函数返回的信息是一个好习惯。这将有利于排除应用程序开发和故障排除。

>   This function returns following codes to describe what is going on with Wi-Fi connection:
>
>   -   0 : `WL_IDLE_STATUS` when Wi-Fi is in process of changing between statuses
>   -   1 : `WL_NO_SSID_AVAIL`in case configured SSID cannot be reached
>   -   3 : `WL_CONNECTED` after successful connection is established
>   -   4 : `WL_CONNECT_FAILED` if password is incorrect
>   -   6 : `WL_DISCONNECTED` if module is not configured in station mode
>
>   It is a good practice to display and check information returned by functions. Application development and troubleshooting will be easier with that.

### Use printDiag 用户打印诊断信息

有一个特定功能可用于打印关键 Wi-Fi 诊断信息：

>   There is a specific function available to print out key Wi-Fi diagnostic information:

```
WiFi.printDiag(Serial);
```

此函数输出格式的样式如下：

>   A sample output of this function looks as follows:

```
Mode: STA+AP
PHY mode: N
Channel: 11
AP id: 0
Status: 5
Auto connect: 1
SSID (10): sensor-net
Passphrase (12): 123!$#0&*esP
BSSID set: 0

```

Use this function to provide snapshot of Wi-Fi status in these parts of application code, that you suspect may be failing.

### Enable Wi-Fi Diagnostic 开始 Wi-Fi 诊断

默认情况下，当你调用 `Serial.begin` 时，会禁用 Wi-Fi 库的诊断输出。要再次启动调试输出，请调用 `Serial.setDebugOutput(ture)` 。要将调试输出重定向到 `Serial1` ，请调用 `Serial.setDebugOutput(ture)` 。有关使用串口进行诊断的其它信息，请参考 [documentation](https://github.com/esp8266/Arduino/blob/master/doc/reference.md) 。

>   By default the diagnostic output from Wi-Fi libraries is disabled when you call `Serial.begin`. To enable debug output again, call `Serial.setDebugOutput(true)`. To redirect debug output to `Serial1` instead, call `Serial1.setDebugOutput(true)`. For additional details regarding diagnostics using serial ports please refer to [documentation](https://github.com/esp8266/Arduino/blob/master/doc/reference.md).

下面是使用 `Serial.setDebugOutput(true)` 在上面的 [快速入门](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#quick-start) 中讨论的示例草图的输出示例：

>   Below is an example of output for sample sketch discussed in [Quick Start](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#quick-start) above with `Serial.setDebugOutput(true)`:

```
Connectingscandone
state: 0 -> 2 (b0)
state: 2 -> 3 (0)
state: 3 -> 5 (10)
add 0
aid 1
cnt 

connected with sensor-net, channel 6
dhcp client start...
chg_B1:-40
...ip:192.168.1.10,mask:255.255.255.0,gw:192.168.1.9
.
Connected, IP address: 192.168.1.10
```

没有 `Serial.setDebugOutput(true)` 的同一个草图将只打印以下内容：

>   The same sketch without `Serial.setDebugOutput(true)` will print out only the following:

```
Connecting....
Connected, IP address: 192.168.1.10

```

### Enable Debugging in IDE 在 IDE 中启动调试

Arduino IDE 提供了方便的方法来为特定库 [enable debugging](https://github.com/esp8266/Arduino/blob/master/doc/Troubleshooting/debugging.md) 。

>   Arduino IDE provides convenient method to [enable debugging](https://github.com/esp8266/Arduino/blob/master/doc/Troubleshooting/debugging.md) for specific libraries.

## What's Inside? 库内部是什么

如果您想详细分析 ESP8266WiFi 库中的内容，请直接访问 GitHub上 esp8266 / Arduino 存储库的 [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi/src) 文件夹。

为了使分析更容易，而不是查看单个标题或源文件，使用一个免费工具自动生成文档。 上面的 [Class Description](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/class-description) 章节中的类索引已经准备好，使用伟大的 [Doxygen](http://www.stack.nl/~dimitri/doxygen/)，这是用于从有注释的C ++源生成文档的事实上的标准工具。

>   If you like to analyze in detail what is inside of the ESP8266WiFi library, go directly to the [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi/src) folder of esp8266 / Arduino repository on the GitHub.
>
>   To make the analysis easier, rather than looking into individual header or source files, use one of free tools to automatically generate documentation. The class index in chapter [Class Description](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/class-description) above has been prepared in no time using great [Doxygen](http://www.stack.nl/~dimitri/doxygen/), that is the de facto standard tool for generating documentation from annotated C++ sources.

![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/doxygen-esp8266wifi-documentation.png)

该工具会遍历所有的标头和源文件，从格式化的注释块中收集信息。如果特定类的开发者注释了代码，你会看到它像下面的例子。

>   The tool crawls through all header and source files collecting information from formatted comment blocks. If developer of particular class annotated the code, you will see it like in examples below.

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/doxygen-example-station-begin.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/doxygen-example-station-begin.png)

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/doxygen-example-station-hostname.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/doxygen-example-station-hostname.png)

如果代码没有注释，你仍然会看到函数原型包括参数类型，并且可以使用提供的链接直接跳转到源代码自行检查它。 Doxygen 在库成员之间提供了非常优秀的导航方式。

>   If code is not annotated, you will still see the function prototype including types of arguments, and can use provided links to jump straight to the source code to check it out on your own. Doxygen provides really excellent navigation between members of library.

[![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/doxygen-example-udp-begin.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/pictures/doxygen-example-udp-begin.png)

 [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi)  的几个类没有注释。 当准备这个文件时， [Doxygen](http://www.stack.nl/~dimitri/doxygen/)  已经极大地帮助了快速浏览几乎 30 个文件，使这个库。

>   Several classes of [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) are not annotated. When preparing this document, [Doxygen](http://www.stack.nl/~dimitri/doxygen/) has been tremendous help to quickly navigate through almost 30 files that make this library.

























