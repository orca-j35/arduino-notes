# [Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**soft-access-point-class.md**



## Soft Access Point Class

以下部分是 ESP8266 所特有的，因为 Arduino WiFi 库文档不包括软接入点。 API描述分为三个简短的章节。 它们介绍如何设置 soft-AP，管理连接和获取有关 soft-AP 接口配置的信息。

>   Section below is ESP8266 specific as [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) documentation does not cover soft access point. The API description is broken down into three short chapters. They cover how to setup soft-AP, manage connection, and obtain information on soft-AP interface configuration.

[TOC]

### Set up Network 设置网络

本节介绍在软接入点 (soft-AP) 模式下设置和配置 ESP8266 的功能。

>   This section describes functions to set up and configure ESP8266 in the soft access point (soft-AP) mode.

#### soft-AP( )

设置 soft-AP 以建立Wi-Fi网络。

此函数的最简单版本([an overload in C++ terms](https://en.wikipedia.org/wiki/Function_overloading))只需要一个参数，用于设置打开的 Wi-Fi 网络。

>   Set up a soft access point to establish a Wi-Fi network.
>
>   The simplest version ([an overload in C++ terms](https://en.wikipedia.org/wiki/Function_overloading)) of this function requires only one parameter and is used to set up an open Wi-Fi network.

```
WiFi.softAP(ssid)
```

要设置密码保护网络或配置其他网络参数，请使用以下重载：

>   To set up password protected network, or to configure additional network parameters, use the following overload:

```
WiFi.softAP(ssid, password, channel, hidden)
```

此函数的第一个参数是必需的，其余三个是可选的。

所有参数的含义如下：

- `ssid` - 包含网络 SSID 的字符串（最多63个字符）
- `password` - 带有密码的可选字符串。 对于WPA2-PSK网络，它应至少为8个字符长。 如果未指定，接入点将打开以供任何人连接。
- channel - 设置 Wi-Fi 通道的可选参数，从1到13 默认channel = 1。
- hidden - 可选参数，如果设置为 `true` 将隐藏SSID

根据设置 soft-AP 的结果，函数将返回 `true` or `false` 。

>   The first parameter of this function is required, remaining three are optional.
>
>   Meaning of all parameters is as follows:
>
>   -   `ssid` - character string containing network SSID (max. 63 characters)
>   -   `password` - optional character string with a password. For WPA2-PSK network it should be at least 8 character long. If not specified, the access point will be open for anybody to connect.
>   -   `channel` - optional parameter to set Wi-Fi channel, from 1 to 13. Default channel = 1.
>   -   `hidden` - optional parameter, if set to `true` will hide SSID
>
>   Function will return `true` or `false` depending on result of setting the soft-AP.

注意：

- 通过 soft-AP 建立的网络将具有192.168.4.1的默认IP地址。这个地址可以通过使用 `softAPConfig` （见下文）来改变。
- 尽管 ESP8266 可以在soft-AP+station 模式下工作，但它实际上只有一个硬件通道。 因此，在 soft-AP+station 模式下，soft-AP 通道将默认为 station 使用的编号。 有关更多信息，这可能会影响连接到ESP8266的 soft-AP 的 stations 的操作，请在Espressif 论坛检查此常见问题条目。

>   Notes:
>
>   -   The network established by softAP will have default IP address of 192.168.4.1. This address may be changed using `softAPConfig` (see below).
>   -   Even though ESP8266 can operate in soft-AP + station mode, it actually has only one hardware channel. Therefore in soft-AP + station mode, the soft-AP channel will default to the number used by station. For more information how this may affect operation of stations connected to ESP8266's soft-AP, please check [this FAQ entry](http://bbs.espressif.com/viewtopic.php?f=10&t=324) on Espressif forum.



#### softAPConfig( ) 

配置 soft-AP 的网络接口。

>   Configure the soft access point's network interface.

```
softAPConfig (local_ip, gateway, subnet) 
```

所有参数都是 `IPAddress` 类型，定义如下：

-   `local_ip` - soft-AP 的 IP 地址

-   `gateway` - 网关 IP 地址

-   `subnet` - 子网掩码

函数将根据更改配置的结果,返回true或false。

>   All parameters are the type of `IPAddress` and defined as follows:
>
>   -   `local_ip` - IP address of the soft access point
>   -   `gateway` - gateway IP address
>   -   `subnet` - subnet mask
>
>   Function will return `true` or `false` depending on result of changing the configuration.

*Example code:*

```
#include <ESP8266WiFi.h>

IPAddress local_IP(192,168,4,22);
IPAddress gateway(192,168,4,9);
IPAddress subnet(255,255,255,0);

void setup()
{
  Serial.begin(115200);
  Serial.println();

  Serial.print("Setting soft-AP configuration ... ");
  Serial.println(WiFi.softAPConfig(local_IP, gateway, subnet) ? "Ready" : "Failed!");

  Serial.print("Setting soft-AP ... ");
  Serial.println(WiFi.softAP("ESPsoftAP_01") ? "Ready" : "Failed!");

  Serial.print("Soft-AP IP address = ");
  Serial.println(WiFi.softAPIP());
}

void loop() {}
```

*Example output:*

```
Setting soft-AP configuration ... Ready
Setting soft-AP ... Ready
Soft-AP IP address = 192.168.4.22

```

### Manage Network 管理网络

一旦 soft-AP 被建立，您可以使用以下函数，检查已连接 stations 的数量或关闭 stations。

>   Once soft-AP is established you may check the number of stations connected, or shut it down, using the following functions.

#### softAPgetStationNum( )

获取连接到 soft-AP 接口的站的数量。

>   Get the count of the stations that are connected to the soft-AP interface.

```
WiFi.softAPgetStationNum() 
```

*Example code:*

```
Serial.printf("Stations connected to soft-AP = %d\n", WiFi.softAPgetStationNum());
```

*Example output:*

```
Stations connected to soft-AP = 2

```

注意：可连接到ESP8266软AP的站的最大数量为 5。

>   Note: the maximum number of stations that may be connected to ESP8266 soft-AP is five.

#### softAPdisconnect( )

断开 station 与 soft-AP 建立的网络的连接。

>   Disconnect stations from the network established by the soft-AP.

```
WiFi.softAPdisconnect(wifioff) 
```

函数将当前 soft-AP 配置的的SSID和密码设置为null 值。 参数 `wifioff` 是可选的。 如果设置为true，它将关闭soft-AP模式。如果操作成功，函数将返回true，否则返回false。

>   Function will set currently configured SSID and password of the soft-AP to null values. The parameter `wifioff` is optional. If set to `true` it will switch the soft-AP mode off.
>
>   Function will return `true` if operation was successful or `false` if otherwise.
>

### Network Configuration 网络配置

下面的函数提供了 ESP8266 的 soft-AP 的 IP 和 MAC 地址。

>   Functions below provide IP and MAC address of ESP8266's soft-AP.

#### softAPIP( )

返回 soft-AP 网络接口的 IP 地址。

>   Return IP address of the soft access point's network interface.

```
WiFi.softAPIP() 
```

返回  `IPAddress` 类型的值。

>   Returned value is of `IPAddress` type.

*Example code:*

```
Serial.print("Soft-AP IP address = ");
Serial.println(WiFi.softAPIP());
```

*Example output:*

```
Soft-AP IP address = 192.168.4.1

```

#### softAPmacAddress( )

返回 soft-AP 的 MAC 地址。此函数有两个版本，它们的返回值类型不同。 第一种返回一个指针，第二种是 `String` 。

>   Return MAC address of soft access point. This function comes in two versions, which differ in type of returned values. First returns a pointer, the second a `String`.

##### Pointer to MAC 指向MAC的指针

```
WiFi.softAPmacAddress(mac)
```

函数接受一个参数 mac，它是指向内存位置的指针（uint8_t 数组的大小为6个元素），以保存mac 地址。 函数本身返回相同的指针值。

>   Function accepts one parameter `mac` that is a pointer to memory location (an `uint8_t` array the size of 6 elements) to save the mac address. The same pointer value is returned by the function itself.

*Example code:*

```
uint8_t macAddr[6];
WiFi.softAPmacAddress(macAddr);
Serial.printf("MAC address = %02x:%02x:%02x:%02x:%02x:%02x\n", macAddr[0], macAddr[1], macAddr[2], macAddr[3], macAddr[4], macAddr[5]);
```

*Example output:*

```
MAC address = 5e:cf:7f:8b:10:13

```

##### MAC as a String 

第二种可选方式是，你可以使用不含任何参数的函数，这会返回一个 String 类型的值。

（可选）您可以使用函数而不返回任何返回`String` 类型值的参数。

>   Optionally you can use function without any parameters that returns a `String` type value.

```
WiFi.softAPmacAddress()
```

*Example code:*

```
Serial.printf("MAC address = %s\n", WiFi.softAPmacAddress().c_str());
```

*Example output:*

```
MAC address = 5E:CF:7F:8B:10:13

```

有关代码示例，请参阅专门针对 Soft Access Point 类的单独的 [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-examples.md) 部分。

>   For code samples please refer to separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/soft-access-point-examples.md) dedicated specifically to the Soft Access Point Class.

