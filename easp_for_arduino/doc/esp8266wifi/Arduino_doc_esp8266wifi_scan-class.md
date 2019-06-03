# [Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**scan-class.md**

| title                  |
| ---------------------- |
| ESP8266WiFi Scan Class |

[ESP8266WiFi Library ![:back:]](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#scan)

## Scan Class

This class is represented in [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) by [scanNetworks()](https://www.arduino.cc/en/Reference/WiFiScanNetworks) function. Developers of esp8266 / Arduino core extend this functionality by additional methods and properties.

[TOC]

这个类的文档分为两部分。 首先是包含可用网络的函数。其次是，在扫描过程中收集到的信息，及如何访问。

>   Documentation of this class is divided into two parts. First covers functions to scan for available networks. Second describes what information is collected during scanning process and how to access it.

### Scan for Networks 扫描网络

扫描网络可能需要几百毫秒的时间才能完成。可在单次运行中被完成，当我们触发扫描程序、等待完成、并提供结果 - 所有步骤由一个单独的函数完成。另一个选项是将这些分成多个步骤，每个步骤由单独的函数完成。这样我们可以在扫描过程中执行其它任务。这被称为异步扫描。这里提供对两种扫描方式的讲解。

>   Scanning for networks takes hundreds of milliseconds to complete. This may be done in a single run when we are triggering scan process, waiting for completion, and providing result - all by a single function. Another option is to split this into steps, each done by a separate function. This way we can execute other tasks while scanning is in progress. This is called asynchronous scanning. Both methods of scanning are documented below.

#### scanNetworks( ) 扫描网络

一次运行扫描可用的 Wi-Fi 网络，并返回已发现的网络数。

>   Scan for available Wi-Fi networks in one run and return the number of networks that has been discovered.

```
WiFi.scanNetworks() 
```

这个函数的重载接受两个可选参数，以提供异步扫描的扩展功能以及寻找隐藏网络。

>   There is on [overload](https://en.wikipedia.org/wiki/Function_overloading) of this function that accepts two optional parameters to provide extended functionality of asynchronous scanning as well as looking for hidden networks.

```
WiFi.scanNetworks(async, show_hidden) 
```

这两个函数参数都是布尔类型。 它们提供流功能：

- `asysnc` - 如果设置为true，则扫描将在后台启动，函数将退出而不等待结果。 要检查结果使用单独的函数 `scanComplete` ，如下所述。
- `show_hidden`  - 将其设置为 `true`  以在扫描结果中包含隐藏 SSID 的网络。

>   Both function parameters are of `boolean` type. They provide the flowing functionality:
>
>   -   `asysnc` - if set to `true` then scanning will start in background and function will exit without waiting for result. To check for result use separate function `scanComplete` that is described below.
>   -   `show_hidden` - set it to `true` to include in scan result networks with hidden SSID.

#### scanComplete( ) 扫描结果

检查异步扫描结果。

>   Check for result of asynchronous scanning.

```
WiFi.scanComplete() 
```

扫描完成函数返回发现的网络数。

如果未执行扫描，则返回值  < 0，如下：

- 正在扫描：-1
- 扫描未触发：-2

>   On scan completion function returns the number of discovered networks.
>
>   If scan is not done, then returned value is < 0 as follows:
>
>   -   Scanning still in progress: -1
>   -   Scanning has not been triggered: -2

#### scanDelete( ) 删除扫描结果

从内存中删除最后一个扫描结果。

>   Delete the last scan result from memory.

```
WiFi.scanDelete() 
```

#### scanNetworksAsync( ) 

开始扫描可用的Wi-Fi网络。 完成后执行另一个函数。

>   Start scanning for available Wi-Fi networks. On completion execute another function.

```
WiFi.scanNetworksAsync(onComplete, show_hidden) 
```

功能参数：

-   `onComplete` - 扫描完成时，执行事件处理程序
-   `show_hidden` - 可选`boolean` 参数, 设置为 `true` 以扫描隐藏网络

>   Function parameters:
>
>   -   `onComplete` - the event handler executed when the scan is done
>   -   `show_hidden` - optional `boolean` parameter, set it to `true` to scan for hidden networks

*Example code:*

```
#include "ESP8266WiFi.h"

void prinScanResult(int networksFound)
{
  Serial.printf("%d network(s) found\n", networksFound);
  for (int i = 0; i < networksFound; i++)
  {
    Serial.printf("%d: %s, Ch:%d (%ddBm) %s\n", i + 1, WiFi.SSID(i).c_str(), WiFi.channel(i), WiFi.RSSI(i), WiFi.encryptionType(i) == ENC_TYPE_NONE ? "open" : "");
  }
}


void setup()
{
  Serial.begin(115200);
  Serial.println();

  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);

  WiFi.scanNetworksAsync(prinScanResult);
}


void loop() {}
```

*Example output:*

```
5 network(s) found
1: Tech_D005107, Ch:6 (-72dBm)
2: HP-Print-A2-Photosmart 7520, Ch:6 (-79dBm)
3: ESP_0B09E3, Ch:9 (-89dBm) open
4: Hack-4-fun-net, Ch:9 (-91dBm)
5: UPC Wi-Free, Ch:11 (-79dBm)

```

### Show Results 显示结果

以下函数提供对扫描结果的访问。 无论扫描是在同步还是异步模式下完成，扫描结果都可以使用相同的API。

可以通过提供一个`networkItem` 来访问单个结果， `networkItem`  是用于标识发现的网络的索引( 从 0 开始 )。

>   Functions below provide access to result of scanning. It does not matter if scanning has been done in synchronous or asynchronous mode, scan results are available using the same API.
>
>   Individual results are accessible by providing a `networkItem` that identifies the index (zero based) of discovered network.
>

#### SSID( )

返回在扫描期间发现的网络的 SSID。

>   Return the SSID of a network discovered during the scan.

```
WiFi.SSID(networkItem) 
```

返回的 SSID 是 `String` 类型。`networkItem` 是在扫描期间所发现的网络的索引( 从0 开始索引 )

>   Returned SSID is of the `String` type. The `networkItem` is a zero based index of network discovered during scan.

#### encryptionType( ) 加密类型

返回在扫描期间发现的网络的加密类型。

>   Return the encryption type of a network discovered during the scan.

```
WiFi.encryptionType(networkItem) 
```

函数返回一个编码加密类型的数字，如下所示：

-   5 : `ENC_TYPE_WEP` - WEP
-   2 : `ENC_TYPE_TKIP` - WPA / PSK
-   4 : `ENC_TYPE_CCMP` - WPA2 / PSK
-   7 : `ENC_TYPE_NONE` - open network
-   8 : `ENC_TYPE_AUTO` - WPA / WPA2 / PSK

`networkItem` 是在扫描期间所发现的网络的索引( 索引从 0 开始 )

>   Function returns a number that encodes encryption type as follows:
>
>   -   5 : `ENC_TYPE_WEP` - WEP
>   -   2 : `ENC_TYPE_TKIP` - WPA / PSK
>   -   4 : `ENC_TYPE_CCMP` - WPA2 / PSK
>   -   7 : `ENC_TYPE_NONE` - open network
>   -   8 : `ENC_TYPE_AUTO` - WPA / WPA2 / PSK
>
>   The `networkItem` is a zero based index of network discovered during scan.
>

#### RSSI( ) 接受信号强度指示

返回在扫描网络期间发现的网络的  [RSSI](https://en.wikipedia.org/wiki/Received_signal_strength_indication) ( 接受信号强度指示 )

>   Return the [RSSI](https://en.wikipedia.org/wiki/Received_signal_strength_indication) (Received Signal Strength Indication) of a network discovered during the scan.

```
WiFi.RSSI(networkItem) 
```

返回的 RSSI 是 `int32_t` 类型。`networkItem` 是在扫描期间所发现的网络的索引( zero based )

>   Returned RSSI is of the `int32_t` type. The `networkItem` is a zero based index of network discovered during scan.

#### BSSID 基本服务集标识 MAC

返回在扫描期间所发现的网络的 MAC 地址的另一个名称 [BSSID](https://en.wikipedia.org/wiki/Service_set_(802.11_network)#Basic_service_set_identification_.28BSSID.29) (Basic Service Set Identification 基本服务集标识) 

>   Return the [BSSID](https://en.wikipedia.org/wiki/Service_set_(802.11_network)#Basic_service_set_identification_.28BSSID.29) (Basic Service Set Identification) that is another name of MAC address of a network discovered during the scan.

```
WiFi.BSSID(networkItem) 
```

函数返回指向保存BSSID的内存位置（大小为6个元素的uint8_t数组）的指针。

如果你不喜欢指针，那么还有另一个版本的这个函数返回一个 `String`。

>   Function returns a pointer to the memory location (an `uint8_t` array with the size of 6 elements) where the BSSID is saved.
>
>   If you do not like to pointers, then there is another version of this function that returns a `String`.
>

```
WiFi.BSSIDstr(networkItem) 
```

`networkItem` 是在扫描期间所发现的网络的索引( zero based )

>   The `networkItem` is a zero based index of network discovered during scan.

#### channel( ) 网络通道

返回在扫描期间发现的网络的通道。

>   Return the channel of a network discovered during the scan.

```
WiFi.channel(networkItem) 
```

返回的网络通道是 `int32_t` 类型。`networkItem` 是在扫描期间所发现的网络的索引( zero based )

>   Returned channel is of the `int32_t` type. The `networkItem` is a zero based index of network discovered during scan.

#### isHidden( )  网络隐藏

返回在扫描期间发现的网络是否隐藏的信息

>   Return information if a network discovered during the scan is hidden or not.

```
WiFi.isHidden(networkItem)
```

返回值是 `bolean` 类型，`true` 意味着网络被隐藏。`networkItem` 是在扫描期间所发现的网络的索引( zero based )

>   Returned value if the `bolean` type, and `true` means that network is hidden. The `networkItem` is a zero based index of network discovered during scan.

#### getNetworkInfo( )  获取网络信息

在单个函数调用中返回上面本章中讨论的所有网络信息。

>   Return all the network information discussed in this chapter above in a single function call.

```
WiFi.getNetworkInfo(networkItem, &ssid, &encryptionType, &RSSI, *&BSSID, &channel, &isHidden) 
```

`networkItem` 是在扫描期间所发现的网络的索引( zero based )。所有其他输入参数通过引用传递给函数。 因此，这些值将依据对特定  `networkItem` 的检索，来更新实际值。函数本身返回布尔值 `true` or `false`  以确认信息检索是否成功。

>   The `networkItem` is a zero based index of network discovered during scan. All other input parameters are passed to function by reference. Therefore they will be updated with actual values retrieved for particular `networkItem`. The function itself returns `boolean` `true` or `false` to confirm if information retrieval was successful or not.

*Example code:*

```
int n = WiFi.scanNetworks(false, true);

String ssid;
uint8_t encryptionType;
int32_t RSSI;
uint8_t* BSSID;
int32_t channel;
bool isHidden;

for (int i = 0; i < n; i++)
{
  WiFi.getNetworkInfo(i, ssid, encryptionType, RSSI, BSSID, channel, isHidden);
  Serial.printf("%d: %s, Ch:%d (%ddBm) %s %s\n", i + 1, ssid.c_str(), channel, RSSI, encryptionType == ENC_TYPE_NONE ? "open" : "", isHidden ? "hidden" : "");
}
```

*Example output:*

```
6 network(s) found
1: Tech_D005107, Ch:6 (-72dBm)
2: HP-Print-A2-Photosmart 7520, Ch:6 (-79dBm)
3: ESP_0B09E3, Ch:9 (-89dBm) open
4: Hack-4-fun-net, Ch:9 (-91dBm)
5: , Ch:11 (-77dBm)  hidden
6: UPC Wi-Free, Ch:11 (-79dBm)

```

For code samples please refer to separate section with [examples ![:arrow_right:](https://assets-cdn.github.com/images/icons/emoji/unicode/27a1.png)](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/scan-examples.md) dedicated specifically to the Scan Class.