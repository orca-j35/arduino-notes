---
title: ESP8266WiFi Soft Access Point Class - Sketch Examples
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**soft-access-point-examples.md**

[ESP8266WiFi Library :back:](readme.md#soft-access-point)

| title                                    |
| ---------------------------------------- |
| ESP8266WiFi Soft Access Point Class - Sketch Examples |



## Soft Access Point

下面的示例介绍如何配置 ESP8266 以软接入点模式运行，以便 Wi-Fi 站可以连接到它。 由软AP 建立的 Wi-Fi 网络将在配置期间使用设置的SSID进行标识。 网络可以用密码保护。 如果在配置期间未设置密码，则网络也可以打开。

>   Example below presents how to configure ESP8266 to run in soft access point mode so Wi-Fi stations can connect to it. The Wi-Fi network established by the soft-AP will be identified with the SSID set during configuration. The network may be protected with a password. The network may be also open, if no password is set during configuration.



[TOC]

### The Sketch

>   Setting up soft-AP with ESP8266 can be done with just couple lines of code.
>

```
#include <ESP8266WiFi.h>

void setup()
{
  Serial.begin(115200);
  Serial.println();

  Serial.print("Setting soft-AP ... ");
  boolean result = WiFi.softAP("ESPsoftAP_01", "pass-to-soft-AP");
  if(result == true)
  {
    Serial.println("Ready");
  }
  else
  {
    Serial.println("Failed!");
  }
}

void loop()
{
  Serial.printf("Stations connected = %d\n", WiFi.softAPgetStationNum());
  delay(3000);
}
```

### How to Use It?

将 `boolean result = WiFi.softAP("ESPsoftAP_01", "pass-to-soft-AP")` 行中的 `pass-to-soft-AP` 修改为有意义的密码，并上传 sketch。打开串口监视器，你将看到如下信息。

>   In line `boolean result = WiFi.softAP("ESPsoftAP_01", "pass-to-soft-AP")` change `pass-to-soft-AP` to some meaningful password and upload sketch. Open serial monitor and you should see:

```
Setting soft-AP ... Ready
Stations connected = 0
Stations connected = 0
...
```

拿出你的移动电话或 PC，打开可用接入点列表，找到 `ESPsoftAP_01` 接入点进行连接。当一个新的站点连接到 AP 时，串口监视器会有所反应。

>   Then take your mobile phone or a PC, open the list of available access points, find ` ESPsoftAP_01 ` and connect to it. This should be reflected on serial monitor as a new station connected:

```
Stations connected = 1
Stations connected = 1
...
```

如果您有另一个Wi-Fi 站可用，然后连接它。 再次检查串口监视器，您现在应该看到两个站报告。

>   If you have another Wi-Fi station available then connect it as well. Check serial monitor again where you should now see two stations reported.


### How Does it Work?

skecth 很小，所以分析起来不应该很困难。在第一行，我们包含了 `ESP8266WiFi` 库。

>   Sketch is small so analysis shouldn't be difficult. In first line we are including ` ESP8266WiFi ` library:

```
#include <ESP8266WiFi.h>
```

通过执行以下命令完成接入点 `ESPsoftAP_01` 的设置。

>   Setting up of the access point `ESPsoftAP_01` is done by executing:

```
 boolean result = WiFi.softAP("ESPsoftAP_01", "pass-to-soft-AP");
```

如果此操作成功，则结果将为真，否则为false。 基于就绪或失败！ 将通过以下 `if - else` 条件语句打印出来。

>   If this operation is successful then `result` will be `true` or `false` if otherwise. Basing on that either `Ready` or `Failed!` will be printed out by the following `if - else` conditional statement. 

### 简化程序

Can we Make it Simpler?

Can we make this sketch even simpler? Yes, we can! 
We can do it by using alternate 替换`if - else` statement as below:

```
WiFi.softAP("ESPsoftAP_01", "pass-to-soft-AP") ? "Ready" : "Failed!"
```

Such statement will return either `Ready` or `Failed!` depending on result of `WiFi.softAP(...)`. This way we can considerably shorten our sketch without any changes to functionality: 这样我们可以大大缩短我们的sketch，而没有任何功能的更改：

```
#include <ESP8266WiFi.h>

void setup()
{
  Serial.begin(115200);
  Serial.println();

  Serial.print("Setting soft-AP ... ");
  Serial.println(WiFi.softAP("ESPsoftAP_01", "pass-to-soft-AP") ? "Ready" : "Failed!");
}

void loop()
{
  Serial.printf("Stations connected = %d\n", WiFi.softAPgetStationNum());
  delay(3000);
}
```

我相信这是非常整洁的代码。 如果 `? :` 条件运算符对你而言是第一次接触，我建议开始使用它，使你的代码更短，更优雅。

>   I believe this is very neat piece of code. If `? :` conditional operator is new to you, I recommend to start using it and make your code shorter and more elegant.

### 总结

Conclusion

一旦你尝试过上面的 sketch，便可将  [WiFiAccessPoint.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiAccessPoint/WiFiAccessPoint.ino) 作为更进一步的示例。它演示了如何从 web 浏览器访问运行在 soft-AP 模式的 ESP。

>   [ESP8266WiFi](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) library makes it easy to turn ESP8266 into soft access point. 
>
>   Once you try above sketch check out [WiFiAccessPoint.ino](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/WiFiAccessPoint/WiFiAccessPoint.ino) as a next step. It demonstrates how to access ESP operating in soft-AP mode from a web browser. 
>
>
>   For the list of functions to manage ESP module in soft-AP mode please refer to the [Soft Access Point Class :arrow_right:](soft-access-point-class.md) documentation.
>



