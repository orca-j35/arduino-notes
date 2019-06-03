---
title: ESP8266WiFi Generic Class - Sketch Examples
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**generic-examples.md**

[ESP8266WiFi Library :back:](readme.md#generic)

## Generic 通用

在 ESP8266 库文档的第一个 [example](readme.md#quick-start) ，我们讨论了如何检查模块何时连接到了 WiFi 网络。我们持续等待，直到连接被建立。如果网络不可用，模块可能会这样永远等待下去，不会做任何其它事情。WiFi 异步扫描模式中的另一个 [example](scan-examples.md#async-scan) 演示了如何在等待扫描结果的同时并行执行其它操作 - 不会影响一个闪烁的 LED 的闪光模式。当模块连接到接入点时，我们应用类似的功能。

>   In the first [example](readme.md#quick-start) of the ESP8266WiFi library documentation we have discussed how to check when module connects to the Wi-Fi network. We were waiting until connection is established. If network is not available, the module could wait like that for ever doing nothing else. Another [example](scan-examples.md#async-scan) on the Wi-Fi asynchronous scan mode demonstrated how to wait for scan result and do in parallel something else - blink a LED not disturbing the blink pattern. Let's apply similar functionality when connecting the module to an access point.

[TOC]

### Introduction

在下面的示例中，我们将展示另一个很酷的示例，让 ESP 同时执行几个任务，并且只用编写少量的代码。

>   In example below we will show another cool example of getting ESP perform couple of tasks at the same time and with very little programming. 


### What are the Tasks?

我们想编写一个代码，通知我们 Wi-Fi 网络的连接已经建立或丢失。 同时我们要执行一些时间关键任务。 我们将用闪烁的 LED 模拟这些关键任务。 通用类提供特定的事件驱动方法，将异步执行，具体取决于连接状态等，而我们已经在做其他任务。

>   We would like to write a code that will inform us that connection to Wi-Fi network has been established or lost. At the same time we want to perform some time critical task. We will simulate it with a blinking LED. Generic class provides specific, event driven methods, that will be executed asynchronously, depending on e.g. connection status, while we are already doing other tasks.

### 事件驱动方法

Event Driven Methods

所有这些方法的列表在  [Generic Class](generic-class.md) 文档中提供。

我们想使用其中两个：

- 当 station 被分配了IP地址时，`onStationModeGotIP` 被调用。 此分配过程可以由 DHCP 客户端或通过执行 `WiFi.config(...)` 来完成。
- 当 station 与 Wi-Fi 网络断开连接时，`onStationModeDisconnected`  被调用。 网络连接断开的原因并不重要。 由于 Wi-Fi 信号较弱或因为接入点被关闭，因此从代码中执行`WiFi.disconnect()` 以断开连接，将触发事件。

>   The list of all such methods is provided in [Generic Class](generic-class.md) documentation.
>
>   We would like to use two of them:
>   * `onStationModeGotIP` called when station is assigned IP address. This assignment may be done  by DHCP client or by executing `WiFi.config(...)`.
>   * `onStationModeDisconnected` called when station is disconnected from Wi-Fi network. The reason of disconnection does not matter. Event will be triggered both if disconnection is done from the code by executing `WiFi.disconnect()`, because the Wi-Fi signal is weak, or because the access point is switched off. 
>
>

### Register the Events

要让事件工作，我们需要完成两个步骤：

>   To get events to work we need to complete just two steps:

1. Declare the event handler: 

  声明事件处理程序

  ```
  WiFiEventHandler disconnectedEventHandler;
  ```

2. Select particular event (in this case `onStationModeDisconnected`) and add the code to be executed when event is fired.

  选择特定的事件( 在本示例中选择 `onStationModeDisconnected` )，并添加在事件触发时要执行的代码。

  ```
  disconnectedEventHandler = WiFi.onStationModeDisconnected([](const WiFiEventStationModeDisconnected& event)
  {
    Serial.println("Station disconnected");
  });
  ```
  如果此事件被触发，代码将打印站已经断开的信息。

  >   If this event is fired the code will print out information that station has been disconnected.

以上就是我们需要去做的。

>   That's it. It is all we need to do. 




### The Code

下面提供了完整的代码，包括开头讨论的两种方法。

>   The complete code, including both methods discussed at the beginning, is provided below.

```
#include <ESP8266WiFi.h>

const char* ssid = "********";
const char* password = "********";

WiFiEventHandler gotIpEventHandler, disconnectedEventHandler;

bool ledState;


void setup()
{
  Serial.begin(115200);
  Serial.println();

  pinMode(LED_BUILTIN, OUTPUT);

  gotIpEventHandler = WiFi.onStationModeGotIP([](const WiFiEventStationModeGotIP& event)
  {
    Serial.print("Station connected, IP: ");
    Serial.println(WiFi.localIP());
  });

  disconnectedEventHandler = WiFi.onStationModeDisconnected([](const WiFiEventStationModeDisconnected& event)
  {
    Serial.println("Station disconnected");
  });

  Serial.printf("Connecting to %s ...\n", ssid);
  WiFi.begin(ssid, password);
}


void loop()
{
  digitalWrite(LED_BUILTIN, ledState);
  ledState = !ledState;
  delay(250);
}
```

注意：下面这个是 C++ 中lambda 表达式。详细内容参考笔记 [C++ Lambda表达式用法 - coderland - 博客园](https://app.yinxiang.com/shard/s10/nl/119459/9cefb799-1102-490e-b309-90c8b6f9eb76)

```
[](const WiFiEventStationModeGotIP& event)
  {
    Serial.print("Station connected, IP: ");
    Serial.println(WiFi.localIP());
  }
```



### Check the Code

上传上面的 sketch 后，打开串口监视器，我们应该看到一个类似的日志：

>   After uploading above sketch and opening a serial monitor we should see a similar log:

```
Connecting to sensor-net ...
Station connected, IP: 192.168.1.10
```

如果你将接入点关断后重启，你将看到如下类容：

>   If you switch off the access point, and put it back on, you will see the following:

```
Station disconnected
Station disconnected
Station disconnected
Station connected, IP: 192.168.1.10
```

连接，断开和打印消息的过程是在负责闪烁LED的循环（）的背景下完成的。 因此，blink 模式始终保持不受干扰。

>   The process of connection, disconnection and printing messages is done in background of the `loop()` that is responsible for blinking the LED. Therefore the blink pattern all the time remains undisturbed.


### Conclusion

检查通用类的事件。 他们将帮助您编写更紧凑的代码。 使用它们来实践将代码拆分为异步执行的单独任务。

对于泛型类中包含的函数的回顾，请参考 [Generic Class :arrow_right:](generic-class.md) ddocumentation。

>   Check out events from generic class. They will help you to write more compact code. Use them to practice splitting your code into separate tasks that are executed asynchronously.
>
>   For review of functions included in generic class, please refer to the [Generic Class :arrow_right:](generic-class.md) documentation.

### 另一个示例

```
/*
    This sketch shows how to use WiFi event handlers.

    In this example, ESP8266 works in AP mode.作为AP
    Three event handlers are demonstrated:
    - station connects to the ESP8266 AP
    - station disconnects from the ESP8266 AP
    - ESP8266 AP receives a probe request from a station ESP8266 AP从站接收到探测请求

    Written by Markus Sattler, 2015-12-29.
    Updated for new event handlers by Ivan Grokhotkov, 2017-02-02.
    This example is released into public domain,
    or, at your option, CC0 licensed.
*/

#include <ESP8266WiFi.h>
#include <stdio.h>

const char* ssid     = "ap-ssid";
const char* password = "ap-password";
//声明事件
WiFiEventHandler stationConnectedHandler;
WiFiEventHandler stationDisconnectedHandler;
WiFiEventHandler probeRequestPrintHandler;
WiFiEventHandler probeRequestBlinkHandler;

bool blinkFlag;

void setup() {
  Serial.begin(115200);
  pinMode(LED_BUILTIN, OUTPUT);
  digitalWrite(LED_BUILTIN, HIGH);

  // Don't save WiFi configuration in flash - optional
  WiFi.persistent(false);

  // Set up an access point
  WiFi.mode(WIFI_AP);
  WiFi.softAP(ssid, password);

  // Register event handlers.注册事件处理程序
  // Callback functions will be called as long as these handler objects exist.
  // 只要这些处理程序的对象存在，就会调用回调函数
  // Call "onStationConnected" each time a station connects
  // 每当有站点连接时，调用 "onStationConnected"
  stationConnectedHandler = WiFi.onSoftAPModeStationConnected(&onStationConnected);
  // Call "onStationDisconnected" each time a station disconnects
  // 
  stationDisconnectedHandler = WiFi.onSoftAPModeStationDisconnected(&onStationDisconnected);
  // Call "onProbeRequestPrint" and "onProbeRequestBlink" each time
  // a probe探测 request is received.
  // Former will print MAC address of the station and RSSI to Serial,
  // 前者将打印站和RSSI的MAC地址串行，
  // latter will blink an LED.
  // 后者会闪烁一个 LED
  probeRequestPrintHandler = WiFi.onSoftAPModeProbeRequestReceived(&onProbeRequestPrint);
  probeRequestBlinkHandler = WiFi.onSoftAPModeProbeRequestReceived(&onProbeRequestBlink);
}

void onStationConnected(const WiFiEventSoftAPModeStationConnected& evt) {
  Serial.print("Station connected: ");
  Serial.println(macToString(evt.mac));
}

void onStationDisconnected(const WiFiEventSoftAPModeStationDisconnected& evt) {
  Serial.print("Station disconnected: ");
  Serial.println(macToString(evt.mac));
}

void onProbeRequestPrint(const WiFiEventSoftAPModeProbeRequestReceived& evt) {
  Serial.print("Probe request from: ");
  Serial.print(macToString(evt.mac));
  Serial.print(" RSSI: ");
  Serial.println(evt.rssi);
}

void onProbeRequestBlink(const WiFiEventSoftAPModeProbeRequestReceived&) {
  // We can't use "delay" or other blocking functions in the event handler.
  // Therefore we set a flag here and then check it inside "loop" function.
  blinkFlag = true;
}

void loop() {
  if (millis() > 10000 && probeRequestPrintHandler) {
    // After 10 seconds, disable the probe request event handler which prints.
    // 10秒钟后，禁用打印的探针请求事件处理程序。
    // Other three event handlers remain active.
    // 其他三个事件处理程序保持活动。
    Serial.println("Not printing probe requests any more (LED should still blink)");
    probeRequestPrintHandler = WiFiEventHandler();
  }
  if (blinkFlag) {
    blinkFlag = false;
    digitalWrite(LED_BUILTIN, LOW);
    delay(100);
    digitalWrite(LED_BUILTIN, HIGH);
  }
  delay(10);
}

String macToString(const unsigned char* mac) {
  char buf[20];
  snprintf(buf, sizeof(buf), "%02x:%02x:%02x:%02x:%02x:%02x",
           mac[0], mac[1], mac[2], mac[3], mac[4], mac[5]);
  return String(buf);
}
```



c++中函数名字前有&是什么意思？

一般出现在类里边的，比如说Complex类里：
Complex &operator=(Complex A1)
{........}
加&和不加&好像都出现过，有什么区别么？

&的意思是，返回类型为Complex 的一个引用。
不加&的时候表示，返回类型为Complex 的一个拷贝。
就类似于函数参数传递时，按值传递和按引用传递的区别。