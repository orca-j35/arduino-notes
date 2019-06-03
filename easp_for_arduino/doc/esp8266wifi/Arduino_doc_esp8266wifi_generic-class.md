---
title: ESP8266WiFi Generic Class
---

[ESP8266WiFi Library :back:](readme.md#generic)

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**generic-class.md**


## Generic Class

[TOC]

>   Methods and properties described in this section are specific to ESP8266. They are not covered in [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) documentation. Before they are fully documented please refer to information below.




### onEvent( )

```
void  onEvent (WiFiEventCb cb, WiFiEvent_t event=WIFI_EVENT_ANY) __attribute__((deprecated)) 
```

要查看如何使用 `onEvent` ，请检查示例 sketch [WiFiClientEvents.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiClientEvents/WiFiClientEvents.ino) 在ESP8266WiFi 库的examples 文件夹中。

>   To see how to use `onEvent` please check example sketch [WiFiClientEvents.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiClientEvents/WiFiClientEvents.ino) available inside examples folder of the ESP8266WiFi library. 


### WiFiEventHandler

```
WiFiEventHandler  onStationModeConnected (std::function< void(const WiFiEventStationModeConnected &)>) 
WiFiEventHandler  onStationModeDisconnected (std::function< void(const WiFiEventStationModeDisconnected &)>) 
WiFiEventHandler  onStationModeAuthModeChanged (std::function< void(const WiFiEventStationModeAuthModeChanged &)>) 
WiFiEventHandler  onStationModeGotIP (std::function< void(const WiFiEventStationModeGotIP &)>) 
WiFiEventHandler  onStationModeDHCPTimeout (std::function< void(void)>) 
WiFiEventHandler  onSoftAPModeStationConnected (std::function< void(const WiFiEventSoftAPModeStationConnected &)>) 
WiFiEventHandler  onSoftAPModeStationDisconnected (std::function< void(const WiFiEventSoftAPModeStationDisconnected &)>) 
```

要查看如何使用 `WiFiEventHandler`，请检查单独的部分与专用于通用类的示例。

>   To see a sample application with `WiFiEventHandler`, please check separate section with [examples :arrow_right:](generic-examples.md) dedicated specifically to the Generic Class.. 


### persistent( )

```
WiFi.persistent (persistent) 
```

模块能够在上电时重新连接到上次使用的 Wi-Fi 网络，或根据存储在闪存特定扇区中的设置重置。 默认情况下，每次在 `WiFi.begin(ssid, password)`等函数中使用这些设置时，这些设置都会写入闪存。 无论 SSID 或密码是否实际更改，这都会发生。

这可能导致闪存的一些磨损，这取决于这种功能被调用的频率。

将 `persistent` 设置为 `false` ，将只有当前使用的值与已经存储在闪存中的值不匹配时才会将SSID / password 写入闪存。

请注意， `WiFi.disconnect` 或 `WiFi.softAPdisconnect` 函数重置当前使用的 SSID / password 。 如果 `persistent` 设置为 `false`，则使用这些函数不会影响存储在闪存中的 SSID / password 。

要了解有关此功能的更多信息，以及为什么介绍此功能，请查看问题报告[#1054](https://github.com/esp8266/Arduino/issues/1054)。

>   Module is able to reconnect to last used Wi-Fi network on power up or reset basing on settings stored in specific sectors of flash memory. By default these settings are written to flash each time they are used in functions like `WiFi.begin(ssid, password)`. This happens no matter if SSID or password has been actually changed.
>
>   This might result in some wear of flash memory depending on how often such functions are called.
>
>   Setting `persistent` to `false` will get SSID / password written to flash only if currently used values do not match what is already stored in flash.
>
>   Please note that functions `WiFi.disconnect` or `WiFi.softAPdisconnect` reset currently used SSID / password. If `persistent` is set to `false`, then using these functions will not affect SSID / password stored in flash.
>
>   To learn more about this functionality, and why it has been introduced, check issue report [#1054](https://github.com/esp8266/Arduino/issues/1054).


### mode( ) 模式

```
WiFi.mode(m) 
WiFi.getMode() 
```

* `WiFi.mode(m)`: set mode to `WIFI_AP`, `WIFI_STA`, `WIFI_AP_STA` or `WIFI_OFF`
* `WiFi.getMode()`: return current Wi-Fi mode (one out of four modes above)


### Other Function Calls

```
int32_t  channel (void) 
bool  setSleepMode (WiFiSleepType_t type) 
WiFiSleepType_t  getSleepMode () 
bool  setPhyMode (WiFiPhyMode_t mode) 
WiFiPhyMode_t  getPhyMode () 
void  setOutputPower (float dBm) 
WiFiMode_t  getMode () 
bool  enableSTA (bool enable) 
bool  enableAP (bool enable) 
bool  forceSleepBegin (uint32 sleepUs=0) 
bool  forceSleepWake () 
int  hostByName (const char *aHostname, IPAddress &aResult)
```

上述功能的文档尚未准备好。

有关代码示例，请参阅单独的部分与 [examples :arrow_right:](generic-examples.md) 专用于通用类。

>   Documentation for the above functions is not yet prepared.
>
>
>   For code samples please refer to separate section with [examples :arrow_right:](generic-examples.md) dedicated specifically to the Generic Class.