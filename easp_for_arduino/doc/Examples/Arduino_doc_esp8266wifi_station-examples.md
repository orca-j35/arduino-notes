---
title: ESP8266WiFi Station Class - Sketch Examples
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**station-examples.md**

[ESP8266WiFi Library :back:](readme.md#station)

## Station

连接到某个接入点的示例在 Quick Start 章节中进行了展示。如果连接丢失，一旦该接入点可用时，ESP8266 将自动重新连接最后使用的接入点。

那么我们能否提供相比于示例中更强大的 Wi-Fi 连接么？ 

>   Example of connecting to an access point has been shown in chapter [Quick Start](readme.md#quick-start). In case connection is lost, ESP8266 will automatically reconnect to the last used access point, once it is again available. 
>
>   Can we provide more robust connection to Wi-Fi than that?



[TOC]

### 简介 ESP8266WiFiMulti

Introduction 

依照 Quick Start 中的示例，我们想做更进一步的改进，如果当前接入点丢失的话，使 ESP 连接到下一个可用的接入点。此功能随 "ESP8266WiFiMulti" 类一同提供，并在下面的 sketch 中演示。

注意：此示例的的串口上有多余的数据包。如：`VMDPE_1:2:139682:0:245|139682_VMDPE` 。目前不清楚原因。

>   Following the example in [Quick Start](readme.md#quick-start), we would like to go one step further and made ESP connect to next available access point if current connection is lost. This functionality is provided with 'ESP8266WiFiMulti' class and demonstrated in sketch below.    

```
#include <ESP8266WiFi.h>
#include <ESP8266WiFiMulti.h>

ESP8266WiFiMulti wifiMulti;
boolean connectioWasAlive = true;

void setup()
{
  Serial.begin(115200);
  Serial.println();

  wifiMulti.addAP("primary-network-name", "pass-to-primary-network");
  wifiMulti.addAP("secondary-network-name", "pass-to-secondary-network");
  wifiMulti.addAP("tertiary-network-name", "pass-to-tertiary-network");
}

void monitorWiFi()
{
  if (wifiMulti.run() != WL_CONNECTED)
  {
    if (connectioWasAlive == true)
    {
      connectioWasAlive = false;
      Serial.print("Looking for WiFi ");
    }
    Serial.print(".");
    delay(500);
  }
  else if (connectioWasAlive == false)
  {
    connectioWasAlive = true;
    Serial.printf(" connected to %s\n", WiFi.SSID().c_str());
  }
}

void loop()
{
  monitorWiFi();
}
```

### 准备接入点

Prepare Access Points

若想要在动态的情况下尝试 sketch，你需要 2 个(或更多) 接入点。在下面的行中 `primary-network-name` 和 `pass-to-primary-network` 分别是主网络名称和密码。不要在次网络中填入相同的信息。

>   To try this sketch in action you need two (or more) access points. In lines below replace `primary-network-name` and `pass-to-primary-network` with name and password to your primary network. Do the same for secondary network. 

```
wifiMulti.addAP("primary-network-name", "pass-to-primary-network");
wifiMulti.addAP("secondary-network-name", "pass-to-secondary-network");
```
如果你有更多的接入点，你可以添加更多的网络。

>   You may add more networks if you have more access points. 

```
wifiMulti.addAP("tertiary-network-name", "pass-to-tertiary-network");
...
```

### 试试看

Try it Out

现在将更新后的 sketch 上传到 ESP 模块，并打开串行监视器。模块将首先扫描可用的网络。然后模块会选择信号最强的网络进行连接。如果连接丢失，模块将连接到下一个可用模块。

>   Now upload updated sketch to ESP module and open serial monitor. Module will first scan for available networks. Then it will select and connect to the network with stronger signal. In case connection is lost, module will connect to next one available. 

This process may look something like:

```
Looking for WiFi ..... connected to sensor-net-1
Looking for WiFi ....... connected to sensor-net-2
Looking for WiFi .... connected to sensor-net-1
```

In above example ESP connected first to `sensor-net-1`. Then I have switched `sensor-net-1` off. ESP discovered that connection is lost and started searching for another configured network. That happened to be `sensor-net-2` so ESP connected to it. Then I have switched `sensor-net-1` back on and shut down `sensor-net-2`. ESP reconnected automatically to `sensor-net-1`.

功能monitorWiFi（）显示当连接丢失显示寻找WiFi。 在搜索另一个配置的接入点的过程中显示点.... 然后，当建立连接时，显示连接到sensor-net-2的消息。

>   Function `monitorWiFi()` is in place to show when connection is lost by displaying `Looking for WiFi`. Dots ` ....` are displayed during process of searching for another configured access point. Then a message like `connected to sensor-net-2` is shown when connection is established. 

### 我们可以让它更简单么？

Can we Make it Simpler?

请注意，可以通过删除 `minitorWiFi()` 函数，并在 `loop()` 函数中只放置 `wifiMulti.run()` ，以简化原有程序。如果需要，ESP 仍将在以配置好的接入点之间重新连接。这样做的话，你将无法在串行监视器上看到这个过程。除非你使用  `Serial.setDebugOutput(true)` ( 其功能在  [Enable Wi-Fi Diagnostic](readme.md#enable-wi-fi-diagnostic) 有进行描述 )

>   Please note that you may simplify this sketch by removing function `monitorWiFi()` and putting inside `loop()` only `wifiMulti.run()`. ESP will still reconnect between configured access points if required. Now you won't be able to see it on serial monitor unless you add `Serial.setDebugOutput(true)` as described in point [Enable Wi-Fi Diagnostic](readme.md#enable-wi-fi-diagnostic). 

Updated sketch for such scenario will look as follows:

```
#include <ESP8266WiFi.h>
#include <ESP8266WiFiMulti.h>

ESP8266WiFiMulti wifiMulti;

void setup()
{
  Serial.begin(115200);
  Serial.setDebugOutput(true);
  Serial.println();

  wifiMulti.addAP("primary-network-name", "pass-to-primary-network");
  wifiMulti.addAP("secondary-network-name", "pass-to-secondary-network");
  wifiMulti.addAP("tertiary-network-name", "pass-to-tertiary-network");
}

void loop()
{
  wifiMulti.run();
}
```

就是这样！这真的是所有需要使ESP自动重新连接可用网络的代码。

上传草图并打开串行监视器后，消息将如下所示。

在上电时初始连接到 sensor-net-1：

>   That's it! This is really all the code you need to make ESP automatically reconnecting between available networks. 
>
>   After uploading sketch and opening the serial monitor, the messages will look as below.
>
>   *Initial connection to sensor-net-1 on power up:*

```
f r0, scandone
f r0, scandone
state: 0 -> 2 (b0)
state: 2 -> 3 (0)
state: 3 -> 5 (10)

add 0
aid 1
cnt
chg_B1:-40

connected with sensor-net-1, channel 1
dhcp client start...
ip:192.168.1.10,mask:255.255.255.0,gw:192.168.1.9
```

与传感器网络 1 的失去连接并建立与传感器网络 2 的连接：

>   *Lost connection to sensor-net-1 and establishing connection to sensor-net-2:*

```
bcn_timout,ap_probe_send_start
ap_probe_send over, rest wifi status to disassoc
state: 5 -> 0 (1)
rm 0
f r-40, scandone
f r-40, scandone
f r-40, scandone
state: 0 -> 2 (b0)
state: 2 -> 3 (0)
state: 3 -> 5 (10)
add 0

aid 1
cnt

connected with sensor-net-2, channel 11
dhcp client start...
ip:192.168.1.102,mask:255.255.255.0,gw:192.168.1.234
```

与传感器网络2的连接丢失并建立连接回传感器网络1：

>   *Lost connection to sensor-net-2 and establishing connection back to sensor-net-1:*

```
bcn_timout,ap_probe_send_start
ap_probe_send over, rest wifi status to disassoc
state: 5 -> 0 (1)
rm 0
f r-40, scandone
f r-40, scandone
f r-40, scandone
state: 0 -> 2 (b0)
state: 2 -> 3 (0)
state: 3 -> 5 (10)
add 0
aid 1
cnt

connected with sensor-net-1, channel 6
dhcp client start...
ip:192.168.1.10,mask:255.255.255.0,gw:192.168.1.9
```

### 总结

Conclusion

相信这个使用 `ESP8266WiFiMulti` 类的简单的 sketch 是一个很酷的示例，仅需要数行代码就可使 ESP8266 在幕后为我们工作。

如上例所示，接入点之间的重新连接需要时间，并且不是无缝的。 因此，在实际应用中，您可能需要 **监视连接状态** 以决定，例如 如果你向外部系统发送数据，应该等到连接恢复。

有关管理站模式所提供的功能的详细介绍，请参考 [Station Class :arrow_right:](station-class.md ) 文档。

>   I believe the minimalist sketch with `ESP8266WiFiMulti` class is a cool example what ESP8266 can do for us behind the scenes with just couple lines of code.
>
>   As shown in above example, reconnecting between access points takes time and is not seamless. Therefore, in practical applications, you will likely need to monitor connection status to decide e.g. if you can send the data to external system or should wait until connection is back.
>
>
>   For detailed review of functions provided to manage station mode please refer to the [Station Class :arrow_right:](station-class.md) documentation.
>

