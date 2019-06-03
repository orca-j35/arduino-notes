# [Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**station-class.md**

| title                     |
| ------------------------- |
| ESP8266WiFi Station Class |

[ESP8266WiFi Library](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#station)

## Station Class  站类

ESP8266 在站模式下提供的功能数量远远超过原始 [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) 中所包含的功能。 因此，我们决定从头开始写一个新的文档，而不是补充原始文档。

站类的描述分为四个部分。 首先讨论建立与 接入点的连接的方法。 第二部分提供了管理连接的方法，例如。 `reconnect` 或`isConnected`.。 第三部分涵盖了用于获取有关连接信息的属性，例如 MAC 或 IP 地址。 最后，第四部分提供替代方法来连接，例如。 Wi-Fi保护设置 (WPS)。

>   The number of features provided by ESP8266 in the station mode is far more extensive than covered in original [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi). Therefore, instead of supplementing original documentation, we have decided to write a new one from scratch.
>
>   Description of station class has been broken down into four parts. First discusses methods to establish connection to an access point. Second provides methods to manage connection like e.g. `reconnect` or `isConnected`. Third covers properties to obtain information about connection like MAC or IP address. Finally the fourth section provides alternate methods to connect like e.g. Wi-Fi Protected Setup (WPS).

[TOC]

以下各点提供说明和代码片段，以明确如何使用特定方法。

有关更多代码示例，请阅参考针对 Station class 的单独的 [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-examples.md)  章节。

>   Points below provide description and code snippets how to use particular methods.
>
>   For more code samples please refer to separate section with [examples](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-examples.md) dedicated specifically to the Station Class.

### Start Here 从这里开始

使用 `begin` 函数将模块切换到 Station 模式。 传递给 `begin` 的典型参数包括 SSID 和 password ，因此模块可以连接到特定的Access Point。

>   Switching the module to Station mode is done with `begin` function. Typical parameters passed to `begin` include SSID and password, so module can connect to specific Access Point.

```
WiFi.begin(ssid, password)
```

默认情况下，ESP 将尝试在连接被断开时重新连接到 Wi-Fi 网络。 没有必要通过单独的代码来处理。 模拟断开连接的一个好方法是重置接入点。 ESP 将报告断开连接，然后尝试自动重新连接。

>   By default, ESP will attempt to reconnect to Wi-Fi network whenever it is disconnected. There is no need to handle this by separate code. A good way to simulate disconnection would be to reset the access point. ESP will report disconnection, and then try to reconnect automatically.

#### WiFi.begin( ) 语句

`begin` 函数有几个版本( 在 C++ 中这被称为函数重载 )。一种版本是上面所提到的：`WiFi.begin(ssid, password)` 。重载在接受参数的数量或类型方面提供了灵活性。

>   There are several version (called *function overloads* in C++) of `begin` function. One was presented just above: `WiFi.begin(ssid, password)`. Overloads provide flexibility in number or type of accepted parameters.



`begin` 最简单的函数重载如下：

>   The simplest overload of `begin` is as follows:

```
WiFi.begin()
```

调用它将指示模块切换到站模式，并基于保存在闪存中的配置连接到最后使用的 access point 。

下面是所有可能参数的另一个重载的开始的语法：

>   Calling it will instruct module to switch to the station mode and connect to the last used access point basing on configuration saved in flash memory.



下面是 `begin` 在重载中所有可用参数的语法：

>   Below is the syntax of another overload of `begin` with the all possible parameters:

```
WiFi.begin(ssid, password, channel, bssid, connect)
```

参数含义如下：

-   `ssid` - 包含我们需要连接的 Access Point 的 SSID 的字符串，做多包含 32 个字符。
-   `password` AP 的密码，一个字符串，最小长度为 8 个字符，最长不超过 64 个字符。
-   `channel` AP 信道，如果我们操作并使用特定信道，否则可以省略此参数。
-   `bssid` - AP 的 mac 地址，此参数也是可选的。
-   `connect` - 一个 `boolean` 参数，如果设置为 `false` ，将指示模块只保存其他参数，而不实际建立与接入点的连接

>   Meaning of parameters is as follows:
>
>   -   `ssid` - a character string containing the SSID of Access Point we would like to connect to, may have up to 32 characters
>   -   `password` to the access point, a character string that should be minimum 8 characters long and not longer than 64 characters
>   -   `channel` of AP, if we like to operate using specific channel, otherwise this parameter may be omitted
>   -   `bssid` - mac address of AP, this parameter is also optional
>   -   `connect` - a `boolean` parameter that if set to `false`, will instruct module just to save the other parameters without actually establishing connection to the access point

#### WiFi.config( ) 语句

禁用 [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) 客户端（动态主机配置协议），并将站接口的 IP 配置设置为用户定义的任意值。 接口将是静态 IP 配置，而不是由 DHCP 提供的值。

>   Disable [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) client (Dynamic Host Configuration Protocol) and set the IP configuration of station interface to user defined arbitrary values. The interface will be a static IP configuration instead of values provided by DHCP.

```
WiFi.config(local_ip, gateway, subnet, dns1, dns2) 
```

如果配置更改被成功应用，函数将返回 `true` 。 如果不能应用配置，因为例如。 模块不在 ststion 或 station+AP 模式下，则返回`false`。

可以提供以下 IP 配置：

- `local_ip` - 在此处输入您要分配 ESP 站接口的 IP 地址
- `gateway` - 应包含访问外部网络的网关（路由器）的IP地址
- `subnet` - 这是定义本地网络的 IP 地址范围的掩码
- `dns1` , `dns2`  - 可选参数，用于定义维护域名目录 (like e.g. *www.google.co.uk*) 的域名服务器 (DNS) 的IP地址，并将其转换为 IP 地址。

>   Function will return `true` if configuration change is applied successfully. If configuration can not be applied, because e.g. module is not in station or station + soft access point mode, then `false` will be returned.
>
>   The following IP configuration may be provided:
>
>   -   `local_ip` - enter here IP address you would like to assign the ESP station's interface
>   -   `gateway` - should contain IP address of gateway (a router) to access external networks
>   -   `subnet` - this is a mask that defines the range of IP addresses of the local network
>   -   `dns1`, `dns2` - optional parameters that define IP addresses of Domain Name Servers (DNS) that maintain a directory of domain names (like e.g. *www.google.co.uk*) and translate them for us to IP addresses

*Example code:*

```
#include <ESP8266WiFi.h>

const char* ssid = "********";
const char* password = "********";

IPAddress staticIP(192,168,1,22);
IPAddress gateway(192,168,1,9);
IPAddress subnet(255,255,255,0);

void setup(void)
{
  Serial.begin(115200);
  Serial.println();

  Serial.printf("Connecting to %s\n", ssid);//仅在 ESP 下可使用，不支持AVR
  WiFi.begin(ssid, password);
  WiFi.config(staticIP, gateway, subnet);
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

*Example output:*

```
Connecting to sensor-net
.
Connected, IP address: 192.168.1.22

```

请注意，具有静态 IP 配置的站通常更快地连接到网络。 在上面的例子中花了大约 500ms ( 输出中值显示了一个点 `.`  ) 。 这是因为 DHCP 客户端获取 IP 配置需要时间，在这种情况下，将跳过此步骤。

>   Please note that station with static IP configuration usually connects to the network faster. In the above example it took about 500ms (one dot `.` displayed). This is because obtaining of IP configuration by DHCP client takes time and in this case this step is skipped.

### Manage Connection 管理连接

#### reconnect( ) 重新连接

重新连接 station。通过断开与当前 AP 的连接，然后重新初始化与当前接入点的连接。

>   Reconnect the station. This is done by disconnecting from the access point an then initiating connection back to the same AP.

```
WiFi.reconnect() 
```

注意：

1.   Station 应已连接到接入点。如果不是这样的情况，那么函数将返回 `false` ，不会执行任何操作。
2.  如果返回 `true` ，则表示连接序列以成功启动。用户仍应检查连接状态，等待 `WL_CONNECTED` 报告。

>   Notes:
>
>   1.  Station should be already connected to an access point. If this is not the case, then function will return `false` not performing any action.
>   2.  If `true` is returned it means that connection sequence has been successfully started. User should still check for connection status, waiting until `WL_CONNECTED` is reported:

```
WiFi.reconnect();
while (WiFi.status() != WL_CONNECTED)
{
  delay(500);
  Serial.print(".");
}
```

#### disconnect( ) 断开连接

将当前 SSID 和 password 设置为 `null` 值，并断开 station 和 AP 的连接。

>   Sets currently configured SSID and password to `null` values and disconnects the station from an access point.

```
WiFi.disconnect(wifioff) 
```

`wifioff` 是一个可选的 `boolean` 参数。如果将其设置为 `true` ，station 模式将被关闭。

>   The `wifioff` is an optional `boolean` parameter. If set to `true`, then the station mode will be turned off.

#### isConnected( ) 已连接

如果 station 被连接到一个 AP 会返回 `true` ，如果没有则返回 `false` 。

>   Returns `true` if Station is connected to an access point or `false` if not.

```
WiFi.isConnected() 
```

#### setAutoConnect( ) 设置自动连接

配置模块在上电时自动连接到最后使用的接入点。

>   Configure module to automatically connect on power on to the last used access point.

```
WiFi.setAutoConnect(autoConnect) 
```

`autoConnect` 是可选参数。 如果设置为 `false`，则将禁用自动连接功能。 如果省略或设置为 `true`，则将启用自动连接。

>   The `autoConnect` is an optional parameter. If set to `false` then auto connection functionality up will be disabled. If omitted or set to `true`, then auto connection will be enabled.

#### getAutoConnect( )  获取自动连接状态

这是 `setAutoConnect()` 的“伴随”函数。 如果模块配置为在上电时自动连接到上次使用的接入点，则返回 `true` 。

>   This is "companion" function to `setAutoConnect()`. It returns `true` if module is configured to automatically connect to last used access point on power on.

```
WiFi.getAutoConnect()
```

如果禁用自动连接功能，则函数返回 `false` 。

>   If auto connection functionality is disabled, then function returns `false`.

#### setAutoReconnect( ) 设置自动重连

设置模块是否将尝试重新连接到接入点，以防它断开连接。

>   Set whether module will attempt to reconnect to an access point in case it is disconnected.

```
WiFi.setAutoReconnect(autoReconnect)  
```

如果参数	`autoReconnect` 设置为 `true`，则模块将尝试恢复与 AP 间丢失的连接。如果设置为 `false` ，则模块将保持断开连接。

注意：当模块已断开连接时，运行 `setAutoReconnect(true)` 将不会使其重新连接到接入点。 应该使用  `reconnect()` 。

>   If parameter `autoReconnect` is set to `true`, then module will try to reestablish lost connection to the AP. If set to `false` then module will stay disconnected.
>
>   Note: running `setAutoReconnect(true)` when module is already disconnected will not make it reconnect to the access point. Instead `reconnect()` should be used.

#### waitForConnectResult( ) 等待连接结果

等模块连接到 AP 为止。此功能适用于在 station 或 station + soft AP 模式下配置的模块。

>   Wait until module connects to the access point. This function is intended for module configured in station or station + soft access point mode.

```
WiFi.waitForConnectResult()  
```

函数返回以下连接状态之一：

-   `WL_CONNECTED` 在成功建立连接后被返回
-   `WL_NO_SSID_AVAIL` 如果无法访问已配置的 SSID
-   `WL_CONNECT_FAILED`  如果密码不正确
-   `WL_IDLE_STATUS` 当 Wi-Fi 正在状态之间变化时
-   `WL_DISCONNECTED`  如果模块未在站模式下配置

>   Function returns one of the following connection statuses:
>
>   -   `WL_CONNECTED` after successful connection is established
>   -   `WL_NO_SSID_AVAIL` in case configured SSID cannot be reached
>   -   `WL_CONNECT_FAILED` if password is incorrect
>   -   `WL_IDLE_STATUS` when Wi-Fi is in process of changing between statuses
>   -   `WL_DISCONNECTED` if module is not configured in station mode

### Configuration 配置

#### macAddress( ) mac地址 

获得 ESP station 连接的 MAC 地址

>   Get the MAC address of the ESP station's interface.

```
WiFi.macAddress(mac) 
```

函数应该提供 `mac` ，它是一个指向内存位置的指针 ( 一个 `uint8_t` 的大小为6个元素的数组 ) 来保存 mac 地址。 函数本身返回相同的指针值。

>   Function should be provided with `mac` that is a pointer to memory location (an `uint8_t` array the size of 6 elements) to save the mac address. The same pointer value is returned by the function itself.

*Example code:*

```
if (WiFi.status() == WL_CONNECTED)
{
  uint8_t macAddr[6];
  WiFi.macAddress(macAddr);
  Serial.printf("Connected, mac address: %02x:%02x:%02x:%02x:%02x:%02x\n", macAddr[0], macAddr[1], macAddr[2], macAddr[3], macAddr[4], macAddr[5]);
}// %02x: 显示 2 位 16 进制数
```

*Example output:*

```
Mac address: 5C:CF:7F:08:11:17

```

如果你对指针感觉不舒服，那么这个函数有可选版本可用。与指针不同，其返回一个格式化的 `String` ，其中包含相同的 mac 地址。

>   If you do not feel comfortable with pointers, then there is optional version of this function available. Instead of the pointer, it returns a formatted `String` that contains the same mac address.

```
WiFi.macAddress() 
```

*Example code:*

```
if (WiFi.status() == WL_CONNECTED)
{
  Serial.printf("Connected, mac address: %s\n", WiFi.macAddress().c_str());
}
```

#### localIP( ) 本地 IP

该功能用于获取 ESP 站点连接的 IP 地址。

>   Function used to obtain IP address of ESP station's interface.

```
WiFi.localIP() 
```

返回值的类型是 [IPAddress](https://github.com/esp8266/Arduino/blob/master/cores/esp8266/IPAddress.h) 。有几种方法可用于显示此类型的数据。 这些方法也会出现在下面的示例中，覆盖了  `subnetMask` 、`gatewayIP` 和 `dnsIP` ，它们也会返回 IP地址。

>   The type of returned value is [IPAddress](https://github.com/esp8266/Arduino/blob/master/cores/esp8266/IPAddress.h). There is a couple of methods available to display this type of data. They are presented in examples below that cover description of `subnetMask`, `gatewayIP` and `dnsIP` that return the IPAdress as well.

*Example code:*

```
if (WiFi.status() == WL_CONNECTED)
{
  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());
}
```

*Example output:*

```
Connected, IP address: 192.168.1.10

```

#### subnetMask( )  子网掩码

>   Get the subnet mask of the station's interface.

```
WiFi.subnetMask()
```

模块应当被连接到 AP 以获得子网掩码。

>   Module should be connected to the access point to obtain the subnet mask.

*Example code:*

```
Serial.print("Subnet mask: ");
Serial.println(WiFi.subnetMask());
```

*Example output:*

```
Subnet mask: 255.255.255.0

```

#### gatewayIP( ) 网关地址

>   Get the IP address of the gateway.

```
WiFi.gatewayIP()
```

*Example code:*

```
Serial.printf("Gataway IP: %s\n", WiFi.gatewayIP().toString().c_str());
```

*Example output:*

```
Gataway IP: 192.168.1.9

```

#### dnsIP( )  DNS地址

>   Get the IP addresses of Domain Name Servers (DNS).

```
WiFi.dnsIP(dns_no)
```

使用输入参数 `dns_no` ，我们可以指定我们需要的域名服务器的IP。 此参数以零为基础，允许的值为 none、0 或 1 。如果没有提供参数，则返回 DNS #1 的IP。

>   With the input parameter `dns_no` we can specify which Domain Name Server's IP we need. This parameter is zero based and allowed values are none, 0 or 1. If no parameter is provided, then IP of DNS #1 is returned.

*Example code:*

```
Serial.print("DNS #1, #2 IP: ");
WiFi.dnsIP().printTo(Serial);
Serial.print(", ");
WiFi.dnsIP(1).printTo(Serial);
Serial.println();
```

*Example output:*

```
DNS #1, #2 IP: 62.179.1.60, 62.179.1.61

```

#### hostname() 主机名

获取分配给ESP站的DHCP主机名。

>   Get the DHCP hostname assigned to ESP station.

```
WiFi.hostname()
```

函数返回 `String` 类型。 默认主机名格式为 `ESP_24xMAC`，其中 24xMAC 是模块MAC地址的最后24位。

>   Function returns `String` type. Default hostname is in format `ESP_24xMAC`where 24xMAC are the last 24 bits of module's MAC address.

可以使用以下功能更改主机名：

>   The hostname may be changed using the following function:

```
WiFi.hostname(aHostname) 
```

输入参数 `aHostname` 可以是 `char*`, `const char*` or `String` 类型。 分配的主机名的最大长度为 32 个字符。 函数根据结果返回 `true` 或 `false` 。 例如，如果超过32个字符的限制，函数将返回`false` ，而不分配新的主机名。

>   Input parameter `aHostname` may be a type of `char*`, `const char*` or `String`. Maximum length of assigned hostname is 32 characters. Function returns either `true` or `false` depending on result. For instance, if the limit of 32 characters is exceeded, function will return `false` without assigning the new hostname.

*Example code:*

```
Serial.printf("Default hostname: %s\n", WiFi.hostname().c_str());
WiFi.hostname("Station_Tester_02");
Serial.printf("New hostname: %s\n", WiFi.hostname().c_str());
```

*Example output:*

```
Default hostname: ESP_081117
New hostname: Station_Tester_02
```

#### status( ) 连接状态 

Return the status of Wi-Fi connection.

```
WiFi.status()
```

函数返回以下连接状态之一：

-   `WL_CONNECTED` 在成功建立连接后被返回
-   `WL_NO_SSID_AVAIL` 如果无法访问已配置的 SSID
-   `WL_CONNECT_FAILED`  如果密码不正确
-   `WL_IDLE_STATUS` 当 Wi-Fi 正在状态之间变化时
-   `WL_DISCONNECTED`  如果模块未在站模式下配置

返回值是  `wl_status_t` 类型，在 [wl_definitions.h](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/include/wl_definitions.h) 中被定义。

>   Function returns one of the following connection statuses:
>
>   -   `WL_CONNECTED` after successful connection is established
>   -   `WL_NO_SSID_AVAIL`in case configured SSID cannot be reached
>   -   `WL_CONNECT_FAILED` if password is incorrect
>   -   `WL_IDLE_STATUS` when Wi-Fi is in process of changing between statuses
>   -   `WL_DISCONNECTED` if module is not configured in station mode
>
>   Returned value is type of `wl_status_t` defined in [wl_definitions.h](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/include/wl_definitions.h)

*Example code:*

```
#include <ESP8266WiFi.h>

void setup(void)
{
  Serial.begin(115200);
  Serial.printf("Connection status: %d\n", WiFi.status());
  Serial.printf("Connecting to %s\n", ssid);
  WiFi.begin(ssid, password);
  Serial.printf("Connection status: %d\n", WiFi.status());
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.printf("\nConnection status: %d\n", WiFi.status());
  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());
}

void loop() {}
```

*Example output:*

```
Connection status: 6
Connecting to sensor-net
Connection status: 6
......
Connection status: 3
Connected, IP address: 192.168.1.10

```

可以在 [wl_definitions.h](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/include/wl_definitions.h) 中查找特定连接状态 6 和 3，如下所示：

>   Particular connection statuses 6 and 3 may be looked up in [wl_definitions.h](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/include/wl_definitions.h) as follows:

```
3 - WL_CONNECTED
6 - WL_DISCONNECTED

```

基于此示例，当运行以上代码时，模块最初从网络断开并返回连接状态 `6 - WL_DISCONNECTED`。 它同样会在运行`WiFi.begin(ssid, password)` 后立即断开连接。 然后大约3秒钟（基于每 500ms 显示的点数），它最终连接返回状态 `3 - WL_CONNECTED` 。

>   Basing on this example, when running above code, module is initially disconnected from the network and returns connection status `6 - WL_DISCONNECTED`. It is also disconnected immediately after running `WiFi.begin(ssid, password)`. Then after about 3 seconds (basing on number of dots displayed every 500ms), it finally gets connected returning status `3 - WL_CONNECTED`.

#### SSID( ) 网络名称

返回Wifi网络的名称，正式的名称是服务集标识(SSID) [Service Set Identification (SSID)](http://www.juniper.net/techpubs/en_US/network-director1.1/topics/concept/wireless-ssid-bssid-essid.html#jd0e34)

>   Return the name of Wi-Fi network, formally called [Service Set Identification (SSID)](http://www.juniper.net/techpubs/en_US/network-director1.1/topics/concept/wireless-ssid-bssid-essid.html#jd0e34).

```
WiFi.SSID()
```

返回值是 `String` 类型。

>   Returned value is of the `String` type.

*Example code:*

```
Serial.printf("SSID: %s\n", WiFi.SSID().c_str());
```

*Example output:*

```
SSID: sensor-net

```

#### psk( ) 预共享密码

返回与 Wi-Fi 网络相关联的当前预共享密钥（密码）。

>   Return current pre shared key (password) associated with the Wi-Fi network.

```
WiFi.psk()
```

返回值是 `String` 类型。

>   Function returns value of the `String` type.

#### BSSID( ) 

返回连接 ESP 模块的接入点的 MAC 地址。 此地址正式称为基本服务集标识（BSSID）。

>   Return the mac address the access point where ESP module is connected to. This address is formally called [Basic Service Set Identification (BSSID)](http://www.juniper.net/techpubs/en_US/network-director1.1/topics/concept/wireless-ssid-bssid-essid.html#jd0e47).

```
WiFi.BSSID()
```

BSSID( ) 函数返回指向保存 BSSID 的内存位置（大小为6个元素的 `uint8_t` 数组）的指针。

以下是类似的功能，返回BSSID，但是会作为 `String` 类型。

>   The `BSSID()` function returns a pointer to the memory location (an `uint8_t` array with the size of 6 elements) where the BSSID is saved.

Below is similar function, but returning BSSID but as a `String` type.

```
WiFi.BSSIDstr()  
```

*Example code:*

```
Serial.printf("BSSID: %s\n", WiFi.BSSIDstr().c_str());
```

*Example output:*

```
BSSID: 00:1A:70:DE:C1:68

```

#### RSSI( ) 网络信号强度

返回 Wi-Fi 网络的信号强度，即正式称为接收信号强度指示（RSSI）。

>   Return the signal strength of Wi-Fi network, that is formally called [Received Signal Strength Indication (RSSI)](https://en.wikipedia.org/wiki/Received_signal_strength_indication).

```
WiFi.RSSI() 
```

信号强度值以 dBm 为单位。 返回值的类型为int32_t。

>   Signal strength value is provided in dBm. The type of returned value is `int32_t`.

*Example code:*

```
Serial.printf("RSSI: %d dBm\n", WiFi.RSSI());
```

*Example output:*

```
RSSI: -68 dBm

```

### Connect Different 备用连接方式

ESP8266 SDK 提供了将 ESP 站连接到接入点的备用方法。 其中  [esp8266 / Arduino](https://github.com/esp8266/Arduino)  内核实现 [WPS](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#wps)  和  [Smart Config](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#smart-config)  ，详细描述如下。

>   [ESP8266 SDK](http://bbs.espressif.com/viewtopic.php?f=51&t=1023) provides alternate methods to connect ESP station to an access point. Out of them [esp8266 / Arduino](https://github.com/esp8266/Arduino) core implements [WPS](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#wps) and [Smart Config](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/station-class.md#smart-config) as described in more details below.

#### WPS

以下 `beginWPSConfig` 函数允许使用 Wi-Fi 保护设置（WPS）连接到网络。 目前仅支持按钮配置 (`WPS_TYPE_PBC` mode) is supported (SDK 1.5.4)。

>   The following `beginWPSConfig` function allows connecting to a network using [Wi-Fi Protected Setup (WPS)](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Setup). Currently only [push-button configuration](http://www.wi-fi.org/knowledge-center/faq/how-does-wi-fi-protected-setup-work) (`WPS_TYPE_PBC` mode) is supported (SDK 1.5.4).

```
WiFi.beginWPSConfig()
```

根据连接结果，函数返回`true` or `false` (`boolean` type)。

>   Depending on connection result function returns either `true` or `false` (`boolean` type).

*Example code:*

```
#include <ESP8266WiFi.h>

void setup(void)
{
  Serial.begin(115200);
  Serial.println();

  Serial.printf("Wi-Fi mode set to WIFI_STA %s\n", WiFi.mode(WIFI_STA) ? "" : "Failed!");
  Serial.print("Begin WPS (press WPS button on your router) ... ");
  Serial.println(WiFi.beginWPSConfig() ? "Success" : "Failed");

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

*Example output:*

```
Wi-Fi mode set to WIFI_STA 
Begin WPS (press WPS button on your router) ... Success
.........
Connected, IP address: 192.168.1.102

```

#### Smart Config

ESP 模块的接入点的 Smart Config 连接，通过嗅探包含所需 AP 的 SSID 和密码的特殊数据包来完成。 为此，移动设备或计算机应该具有广播编码的 SSID 和密码的功能。

提供以下三个功能来实现 Smart Config。

通过嗅探包含所需接入点的SSID和密码的特殊数据包启动智能配置模式。 根据结果返回 `true` or `false`。

>   The Smart Config connection of an ESP module an access point is done by sniffing for special packets that contain SSID and password of desired AP. To do so the mobile device or computer should have functionality of broadcasting of encoded SSID and password.
>
>   The following three functions are provided to implement Smart Config.
>
>   Start smart configuration mode by sniffing for special packets that contain SSID and password of desired Access Point. Depending on result either `true` or `false` is returned.

```
beginSmartConfig() 
```

查询Smart Config状态，决定何时停止配置。 函数返回布尔类型的true或false。

>   Query Smart Config status, to decide when stop configuration. Function returns either `true` or `false` of `boolean` type.

```
smartConfigDone()
```

停止智能配置，释放由 `beginSmartConfig()` 采取的缓冲区。 根据结果函数返回 true 或 false 布尔类型。

>   Stop smart config, free the buffer taken by `beginSmartConfig()`. Depending on result function return either `true` or `false`of `boolean` type.

```
stopSmartConfig() 
```

有关Smart Config的其他详细信息，请参阅ESP8266 API用户指南。

>   For additional details regarding Smart Config please refer to [ESP8266 API User Guide](http://bbs.espressif.com/viewtopic.php?f=51&t=1023).



















