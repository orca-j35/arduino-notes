---
title: ESP8266WiFi Scan Class - Sketch Examples
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**scan-examples.md**

[ESP8266WiFi Library :back:](readme.md#scan)



### Scan

>   To connect a mobile phone to a hot spot, you typically open Wi-Fi settings app, list available networks and then pick选择 the hot spot you need. You can also list the networks with ESP8266 and here is how.

[TOC]

### 简单的扫描方式

Simple Scan 

此示例显示了我们需要检查可用网络列表的最低限度代码。

>   This example shows the bare minimum code we need to check for the list of available networks.


#### Disconnect 断开连接

To start with, enable module in station mode and then disconnect.

```
WiFi.mode(WIFI_STA);
WiFi.disconnect();
```

Running `WiFi.disconnect()` is to shut down a connection to an access point that module may have automatically made using previously saved credentials.


#### Scan for Networks

After some **delay** to let the module disconnect, go to scanning for available networks:

```
int n = WiFi.scanNetworks();
```

Now just check if returned `n` if greater than 0 and list found networks:

```
for (int i = 0; i < n; i++)
{
  Serial.println(WiFi.SSID(i));
}
```

This is that simple.


#### Complete Example

The sketch should have obligatory `#include <ESP8266WiFi.h>` and looks as follows:

```
#include "ESP8266WiFi.h"

void setup()
{
  Serial.begin(115200);
  Serial.println();

  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);
}

void loop()
{
  Serial.print("Scan start ... ");
  int n = WiFi.scanNetworks();
  Serial.print(n);
  Serial.println(" network(s) found");
  for (int i = 0; i < n; i++)
  {
    Serial.println(WiFi.SSID(i));
  }
  Serial.println();

  delay(5000);
}
```

#### 操作示

Example in Action

Upload this sketch to ESP module and open a serial monitor. If there are access points around (sure there are) you will see a similar list repeatedly重复 printed out:

```
Scan start ... 5 network(s) found
Tech_D005107
HP-Print-A2-Photosmart 7520
ESP_0B09E3
Hack-4-fun-net
UPC Wi-Free
```

当查询文本 `scan start ... ` 被显示时，你会注意到，你将会注意到之后的  `n network(s) found` 文本内容的显示会有一个明显的时间间隔。这是因为 `WiFi.scanNetworks()` 的执行需要时间，我们的程序会在等待它完成后，再执行后面的代码。如果我们同时希望 ESP 运行时间的关键过程( 如，动画 ) 不被打断，该怎么办？

事实证明，这是相当容易做的通过扫描网络在异步模式。

在下面的示例中检查它，也将演示打印除SSID之外的可用网络的其他参数。

下面的示例会验证这个方法，同时还会演示打印可用网络中除 SSID 之外的参数。

>   When looking for the text `scan start ...` displayed, you will notice that it takes noticeable time for the following text `n network(s) found` to show up. This is because execution of `WiFi.scanNetworks()` takes time and our program is waiting for it to complete before moving to the next line of code. What if at the same time we would like ESP to run time critical process (e.g. animation) that should not be disturbed?
>
>   It turns out that this is fairly easy to do by scanning networks in async mode.
>
>   Check it out in next example below that will also demonstrate printing out other parameters of available networks besides SSID.
>

### 异步扫描

Async Scan

我们喜欢的方式是触发扫描网络的过程，然后返回到 `loop()` 中执行代码。一旦扫描完成，在方便的时候，我们将检查网络列表。“time critical process" 将通过一个以 250ms 为周期闪烁的 LED 灯模拟。

我们希望闪烁的模式不会在任何时候受到干扰。

>   What we like to do, is to trigger process of scanning for networks and then return to executing code inside the `loop()`. Once scanning is complete, at a convenient time, we will check the list of networks. The "time critical process" will be simulated by a blinking LED at 250ms period.
>
>   We would like the blinking pattern not be disturbed at any time.

#### 没有 delay() 延迟

No delay()

要实现这样的功能，我们应该避免在 `loop()` 函数内部使用任何 `delay()` 函数。相反，当触发特定操作时，我们会定义周期。在 `loop()` 内部，我们会检查 `millis()` ( 对毫秒进行计数的内部时钟 )，如果周期到期，则触发操作。

请查看 [BlinkWithoutDelay.ino](BlinkWithoutDelay.ino) 示例 sketch 如何完成该操作。相同的技术可用于周期性的触发 Wi-Fi 网络的扫描。

>   To implement such functionality we should refrain from using any `delay()` inside the `loop()`. Instead we will define period when to trigger particular action. Then inside `loop()` we will check `millis()` (internal clock that counts milliseconds) and fire the action if the period expires. 
>
>   Please check how this is done in [BlinkWithoutDelay.ino](BlinkWithoutDelay.ino) example sketch. Identical technique can be used to periodically trigger scanning for Wi-Fi networks. 
>


#### 设置 Setup

首先，我们应该定义扫描周期和内部变量 `lastScanMillis` ，它将在最后一次扫描时保持时间。

>   First we should define scanning period and internal variable `lastScanMillis` that will hold time when the last scan has been made.

```
#define SCAN_PERIOD 5000
long lastScanMillis;
```

#### 何时开始扫描

When to Start

Then inside the `loop()` we will check if `SCAN_PERIOD` expired到期, so it is time to fire next scan:

```
if (currentMillis - lastScanMillis > SCAN_PERIOD)
{
  WiFi.scanNetworks(true);
  Serial.print("\nScan start ... ");
  lastScanMillis = currentMillis;
}
```

请注意，`WiFi.scanNetworks(true)`有一个额外的参数 `true` ，在上面的例子中不存在。 这是在异步模式下扫描的指令，即触发扫描过程，不等待结果（处理将在后台完成），并继续执行下一行代码。 我们需要使用异步模式否则 250ms LED 闪烁模式将被扰乱，因为扫描花费的时间超过250ms。

>   Please note that `WiFi.scanNetworks(true)` has an extra parameter `true` that was not present in [previous example](#simple-scan) above. This is an instruction to scan in asynchronous mode, i.e. trigger scanning process, do not wait for result (processing will be done in background) and move to the next line of code. We need to use asynchronous mode otherwise 250ms LED blinking pattern would be disturbed as scanning takes longer than 250ms.

#### 完成扫描后检查

Check When Done

最后，我们定期检查扫描是否结束，一旦完成便打印出结果。为此，我们将使用 `WiFi.scanComplete()` 函数，它会在扫描完成后返回找到的网络的数量。如果扫描仍在进行中，会返回-1。如果尚未触发扫描，则返回 -2 。

>   Finally we should periodically check for scan completion to print out the result once ready. To do so, we will use function `WiFi.scanComplete()`, that upon completion returns the number of found networks. If scanning is still in progress it returns -1. If scanning has not been triggered yet, it would return -2.

```
int n = WiFi.scanComplete();
if(n >= 0)
{
  Serial.printf("%d network(s) found\n", n);
  for (int i = 0; i < n; i++)
  {
    Serial.printf("%d: %s, Ch:%d (%ddBm) %s\n", i+1, WiFi.SSID(i).c_str(), WiFi.channel(i), WiFi.RSSI(i), WiFi.encryptionType(i) == ENC_TYPE_NONE ? "open" : "");
  }
  WiFi.scanDelete();
}
```

注意 `WiFi.scanDelete()` 函数，该函数从内存中删除扫描结果，因此在每个 `loop()` 运行时不会冲重复打印。

>   Please note function `WiFi.scanDelete()` that is deleting scanning result from memory, so it is not printed out over and over again on each `loop()` run.


#### Complete Example

完整的 sketch 如下。`setup()` 中的代码和之前示例中的描述相同，除了额外的 `pinMode()` 函数，用于为 LED 设置输出引脚。

>   Complete sketch is below. The code inside `setup()` is the same as described in [previous example](#simple-scan) except for an additional `pinMode()` to configure the output pin for LED.

```
#include "ESP8266WiFi.h"

#define BLINK_PERIOD 250
long lastBlinkMillis;
boolean ledState;

#define SCAN_PERIOD 5000
long lastScanMillis;


void setup()
 {
  Serial.begin(115200);
  Serial.println();

  pinMode(LED_BUILTIN, OUTPUT);

  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);
}

void loop()
{
  long currentMillis = millis();

  // blink LED
  if (currentMillis - lastBlinkMillis > BLINK_PERIOD)
  {
    digitalWrite(LED_BUILTIN, ledState);
    ledState = !ledState;
    lastBlinkMillis = currentMillis;
  }

  // trigger Wi-Fi network scan
  if (currentMillis - lastScanMillis > SCAN_PERIOD)
  {
    WiFi.scanNetworks(true);
    Serial.print("\nScan start ... ");
    lastScanMillis = currentMillis;
  }

  // print out Wi-Fi network scan result uppon completion
  int n = WiFi.scanComplete();
  if(n >= 0)
  {
    Serial.printf("%d network(s) found\n", n);
    for (int i = 0; i < n; i++)
    {
      Serial.printf("%d: %s, Ch:%d (%ddBm) %s\n", i+1, WiFi.SSID(i).c_str(), WiFi.channel(i), WiFi.RSSI(i), WiFi.encryptionType(i) == ENC_TYPE_NONE ? "open" : "");
    }
    WiFi.scanDelete();
  }
}
```

#### 运行示例

Example in Action

将上述草图上传到ESP模块并打开串行监视器。 你会看到类似的列表每5秒打印一次：

>   Upload above sketch to ESP module and open a serial monitor. You should see similar list printed out every 5 seconds:

```
Scan start ... 5 network(s) found
1: Tech_D005107, Ch:6 (-72dBm)
2: HP-Print-A2-Photosmart 7520, Ch:6 (-79dBm)
3: ESP_0B09E3, Ch:9 (-89dBm) open
4: Hack-4-fun-net, Ch:9 (-91dBm)
5: UPC Wi-Free, Ch:11 (-79dBm)
```

检查LED。 它应该每秒闪烁4次。

>   Check the LED. It should be blinking undisturbed four times per second.


### 总结 Conclusion

扫描类API提供了一套全面的方法来在同步和异步模式下进行扫描。 因此，我们可以轻松实现在后台进行扫描的代码，而不会影响ESP8266模块上运行的其他进程。

有关管理扫描模式的函数列表，请参阅扫描类：arrow_right：文档。

>   The scan class API provides comprehensive set of methods to do scanning in both synchronous as well as in asynchronous mode. Therefore we can easy implement code that is doing scanning in background without disturbing other processes running on ESP8266 module. 
>
>   For the list of functions provided to manage scan mode please refer to the [Scan Class :arrow_right:](scan-class.md) documentation.

