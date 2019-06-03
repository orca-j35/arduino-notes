# [Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/**libraries.md**

[TOC]

## WiFi(ESP8266WiFi library)

 [ESP8266 的 Wi-Fi 库](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) 是基于 [ESP8266 SDK](http://bbs.espressif.com/viewtopic.php?f=51&t=1023) 开发的，使用  [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) 库的命名约定和总体功能理念。 随着时间的推移，丰富的 Wi-Fi 功能从 ESP9266 SDK 被移植到了[esp8266 / Adruino](https://github.com/esp8266/Arduino) ，对 [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) 进行了扩展，因此我们明显需要提供新的文档，以展示新的功能和额外的扩展。

[ESP8266WiFi library documentation](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md) : [Quick Start](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#quick-start) | [Who is Who](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#who-is-who) | [Station](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#station) | [Soft Access Point](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#soft-access-point) | [Scan](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#scan) | [Client](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#client) | [Client Secure](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#client-secure) | [Server](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#server) | [UDP](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#udp) | [Generic](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#generic) | [Diagnostics](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#diagnostics)

>   The [Wi-Fi library for ESP8266](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi) has been developed basing on [ESP8266 SDK](http://bbs.espressif.com/viewtopic.php?f=51&t=1023), using naming convention and overall functionality philosophy of [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi). Over time the wealth Wi-Fi features ported from ESP9266 SDK to [esp8266 / Adruino](https://github.com/esp8266/Arduino) out grow [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) and it became apparent that we need to provide separate documentation on what is new and extra.
>
>   [ESP8266WiFi library documentation](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md) : [Quick Start](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#quick-start) | [Who is Who](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#who-is-who) | [Station](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#station) | [Soft Access Point](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#soft-access-point) | [Scan](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#scan) | [Client](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#client) | [Client Secure](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#client-secure) | [Server](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#server) | [UDP](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#udp) | [Generic](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#generic) | [Diagnostics](https://github.com/esp8266/Arduino/blob/master/doc/esp8266wifi/readme.md#diagnostics)

## Ticker

用于以一个给定周期重复调用函数的库。包括两个例子。

目前不建议从 Ticker 回调函数中阻塞 IO 操作 (network, serial, file) 。相反，在 ticker 回调中设置一个标志，并在 loop 函数内检查该标志。

这是库以简化 `Ticher` 的使用，并避免 WDT 重置： [TickerScheduler](https://github.com/Toshik/TickerScheduler)

>   Library for calling functions repeatedly with a certain period. Two examples included.
>
>   It is currently not recommended to do blocking IO operations (network, serial, file) from Ticker callback functions. Instead, set a flag inside the ticker callback and check for that flag inside the loop function.
>
>   Here is library to simplificate `Ticker` usage and avoid WDT reset: [TickerScheduler](https://github.com/Toshik/TickerScheduler)

## EEPROM

这与标准 EEPROM 类有点不同。在开始读取或写入之前，你需要调用 `EEPROM.begin(size)` ，大小是要使用的字节数。 大小可以是 4 到 4096 字节之间的任何值。

`EEPROM.write` 不会立即写入闪存，而是您必须在希望将更改保存到闪存时调用 `EEPROM.commit()` 。 EEPROM.end（）也将提交，并将释放EEPROM内容的RAM副本。 `EEPROM.end()` 也将提交，并将释放 EEPROM 内容的 RAM 副本。

EEPROM 库使用位于 SPIFFS 之后的一个闪存扇区。

包括三个例子。

>   This is a bit different from standard EEPROM class. You need to call `EEPROM.begin(size)` before you start reading or writing, size being the number of bytes you want to use. Size can be anywhere between 4 and 4096 bytes.
>
>   `EEPROM.write` does not write to flash immediately, instead you must call `EEPROM.commit()` whenever you wish to save changes to flash. `EEPROM.end()` will also commit, and will release the RAM copy of EEPROM contents.
>
>   EEPROM library uses one sector of flash located just after the SPIFFS.
>
>   Three examples included.



## I2C (Wire library)

Wire 库目前支持高达约 450KHz 的主机模式。 在使用 I2C 之前，需要通过调用 `Wire.begin(int sda, int scl)`，即 ESP-01上的`Wire.begin(0, 2)` 设置SDA和SCL的引脚，否则默认为引脚4（SDA）和 5（SCL）。

>   Wire library currently supports master mode up to approximately 450KHz. Before using I2C, pins for SDA and SCL need to be set by calling `Wire.begin(int sda, int scl)`, i.e. `Wire.begin(0, 2)` on ESP-01, else they default to pins 4(SDA) and 5(SCL).

## SPI

SPI库支持整个Arduino SPI API，包括 transactions，包括设置阶段 (CPHA)。 但不支持设置时钟极性 (CPOL)，(SPI_MODE2 and SPI_MODE3 not working)。

>   SPI library supports the entire Arduino SPI API including transactions, including setting phase (CPHA). Setting the Clock polarity (CPOL) is not supported, yet (SPI_MODE2 and SPI_MODE3 not working).

## SoftwareSerial

由 Peter Lerup (@plerup) 完成的 ESP8266 端口的 SoftwareSerial 库支持波特率高达 115200 和多个 SoftwareSerial 实例。 如果您想要建议改进或打开与 SoftwareSeria 相关的问题，请参见https://github.com/plerup/espsoftwareserial

>   An ESP8266 port of SoftwareSerial library done by Peter Lerup (@plerup) supports baud rate up to 115200 and multiples SoftwareSerial instances. See [https://github.com/plerup/espsoftwareserial](https://github.com/plerup/espsoftwareserial) if you want to suggest an improvement or open an issue related to SoftwareSerial.

注意：该库当前只有8位，无奇偶校验和一个停止位可用。

Only 8-bit, no parity and one stop bit is available.

### EspSoftwareSerial

https://github.com/plerup/espsoftwareserial

为 ESP8266 实现 Arduino 软件串行库

与相应的AVR库具有相同的功能，但几个实例可以同时处于活动状态。 支持速度高达115200波特。 构造函数还具有可选的输入缓冲区大小。

请注意，由于ESP总是有其他活动正在进行，在中断时序中会有一些不精确。 当在高波特率下有大量数据流量时，这可能导致位错误。

>   Implementation of the Arduino software serial library for the ESP8266
>
>   Same functionality as the corresponding AVR library but several instances can be active at the same time. Speed up to 115200 baud is supported. The constructor also has an optional input buffer size.
>
>   Please note that due to the fact that the ESP always have other activities ongoing, there will be some inexactness in interrupt timings. This may lead to bit errors when having heavy data traffic in high baud rates.

## ESP-specific APIs

与深度睡眠和看门狗定时器相关的 APIs 在 `ESP` 对象中可用，但仅在Alpha版本中可用。

`ESP.deepSleep(microseconds, mode)` 会使芯片进入深度睡眠。 `mode` 可选 `WAKE_RF_DEFAULT`, `WAKE_RFCAL`, `WAKE_NO_RFCAL`, `WAKE_RF_DISABLED` 中的一个 。(GPIO16 需要连接到 RST 以从深度睡眠中唤醒。

`ESP.rtcUserMemoryWrite(offset, &data, sizeof(data))` 和 `ESP.rtcUserMemoryRead(offset, &data, sizeof(data))` 允许数据数据在 RTC 用户各自芯片的存储器中被存储和取回。RTC 用户存储器的总尺寸是 512 字节，因此  offset + sizeof(data) 不应超过 512 。数据应为4字节对齐。所存储的数据可以在深睡眠周期之间保持。然而，芯片上电后，数据可能会丢失。

`ESP.restart()` 重新启动 CPU。

`ESP.getResetReason()` 返回包含人类可读格式的最后一个复位原因的字符串。

`ESP.getFreeHeap()` 返回可用堆的大小。

`ESP.getChipId()` 返回 ESP8266 芯片ID作为 32 位整数。

可以使用多个API来获取闪存芯片信息：

`ESP.getFlashChipId()` 返回一个32位整数的闪存芯片ID。

`ESP.getFlashChipSize()` 返回Flash芯片的大小（以字节为单位），如SDK所示（可能小于实际大小）。

`ESP.getFlashChipRealSize()` 返回基于闪存芯片ID的实际芯片大小（以字节为单位）。

`ESP.getFlashChipSpeed(void)` 返回闪存芯片的频率，单位为Hz。

`ESP.getCycleCount()` 返回自启动以来的 cpu 指令周期计数，为无符号32位。这对于精确定时非常短的动作（例如位突变）是有用的。

`ESP.getVcc()` 可用于测量电源电压。 ESP 需要在启动时重新配置 ADC，以使此功能可用。将以下行添加到草图的顶部以使用 `getVcc`：

```
ADC_MODE(ADC_VCC);
```

在此模式下必须断开 TOUT 引脚。

注意，默认情况下，ADC 配置为使用 `analogRead(A0)` 从TOUT 引脚读取模拟量，并且 `ESP.getVCC()` 不可用。

>   APIs related to deep sleep and watchdog timer are available in the `ESP` object, only available in Alpha version.
>
>   `ESP.deepSleep(microseconds, mode)` will put the chip into deep sleep. `mode` is one of `WAKE_RF_DEFAULT`, `WAKE_RFCAL`, `WAKE_NO_RFCAL`, `WAKE_RF_DISABLED`. (GPIO16 needs to be tied to RST to wake from deepSleep.)
>
>   `ESP.rtcUserMemoryWrite(offset, &data, sizeof(data))` and `ESP.rtcUserMemoryRead(offset, &data, sizeof(data))` allow data to be stored in and retrieved from the RTC user memory of the chip respectively. Total size of RTC user memory is 512 bytes, so offset + sizeof(data) shouldn't exceed 512. Data should be 4-byte aligned. The stored data can be retained between deep sleep cycles. However, the data might be lost after power cycling the chip.
>
>   `ESP.restart()` restarts the CPU.
>
>   `ESP.getResetReason()` returns String containing the last reset reason in human readable format.
>
>   `ESP.getFreeHeap()` returns the free heap size.
>
>   `ESP.getChipId()` returns the ESP8266 chip ID as a 32-bit integer.
>
>   Several APIs may be used to get flash chip info:
>
>   `ESP.getFlashChipId()` returns the flash chip ID as a 32-bit integer.
>
>   `ESP.getFlashChipSize()` returns the flash chip size, in bytes, as seen by the SDK (may be less than actual size).
>
>   `ESP.getFlashChipRealSize()` returns the real chip size, in bytes, based on the flash chip ID.
>
>   `ESP.getFlashChipSpeed(void)` returns the flash chip frequency, in Hz.
>
>   `ESP.getCycleCount()` returns the cpu instruction cycle count since start as an unsigned 32-bit. This is useful for accurate timing of very short actions like bit banging.
>
>   `ESP.getVcc()` may be used to measure supply voltage. ESP needs to reconfigure the ADC at startup in order for this feature to be available. Add the following line to the top of your sketch to use `getVcc`:
>
>   ```
>   ADC_MODE(ADC_VCC);
>   ```
>
>   TOUT pin has to be disconnected in this mode.
>
>   Note that by default ADC is configured to read from TOUT pin using `analogRead(A0)`, and `ESP.getVCC()` is not available.

## mDNS and DNS-SD responder (ESP8266mDNS library)

允许 sketch 响应域名的多播 DNS 查询(如"foo.local"，和 DNS-SD  (service discovery)  查询)。 有关详细信息，请参阅附加示例。

>   Allows the sketch to respond to multicast DNS queries for domain names like "foo.local", and DNS-SD (service discovery) queries. See attached example for details.

## SSDP responder (ESP8266SSDP)

SSDP 是另一种服务发现协议，在 Windows 上支持开箱即用。 请参见附加示例以供参考。

>   SSDP is another service discovery protocol, supported on Windows out of the box. See attached example for reference.

## DNS server (DNSServer library)

实现一个简单的 DNS 服务器，可以在 STA 和 AP 模式下使用。 DNS 服务器当前仅支持一个域（对于所有其他域，它将使用NXDOMAIN 或自定义状态代码进行回复）。 有了它，客户端可以使用域名而不是IP地址打开在 ESP8266 上运行的 Web 服务器。 有关详细信息，请参阅附加示例。

>   Implements a simple DNS server that can be used in both STA and AP modes. The DNS server currently supports only one domain (for all other domains it will reply with NXDOMAIN or custom status code). With it, clients can open a web server running on ESP8266 using a domain name, not an IP address. See attached example for details.

## Servo 伺服电机

该库提供控制 RC (业余) 伺服电机的能力。 它将在任何可用的输出引脚上支持多达24个伺服。 默认情况下，前12个伺服器将使用Timer0 计数器，目前这不会干扰任何其他支持。 高于 12 的伺服计数将使用 Timer1，使用它的功能将受到影响。 虽然许多RC伺服电机将接受来自 ESP8266 的 3.3V IO 数据引脚，但大多数RC电机将不能运行3.3V，并且将需要另一个符合其规格的电源。 确保连接 ESP8266 和伺服电机电源之间的接地。

>   This library exposes the ability to control RC (hobby) servo motors. It will support upto 24 servos on any available output pin. By defualt the first 12 servos will use Timer0 and currently this will not interfere with any other support. Servo counts above 12 will use Timer1 and features that use it will be effected. While many RC servo motors will accept the 3.3V IO data pin from a ESP8266, most will not be able to run off 3.3v and will require another power source that matches their specifications. Make sure to connect the grounds between the ESP8266 and the servo motor power supply.

## 其它库(这些库没有被包含在 IDE 中)

Other libraries (not included with the IDE)

不依赖于 low-level 访问 AVR 寄存器的库应该工作得很好。 这里有一些已验证工作的库：

Libraries that don't rely on low-level access to AVR registers should work well. Here are a few libraries that were verified to work:

-   [Adafruit_ILI9341](https://github.com/Links2004/Adafruit_ILI9341) - Port of the Adafruit ILI9341 for the ESP8266  
-   [arduinoVNC](https://github.com/Links2004/arduinoVNC) - VNC Client for Arduino **Arduino 的 VPN 客户端**
-   [arduinoWebSockets](https://github.com/Links2004/arduinoWebSockets) - WebSocket Server and Client compatible with ESP8266 (RFC6455) **与ESP8266（RFC6455）兼容的WebSocket服务器和客户端**
-   [aREST](https://github.com/marcoschwartz/aREST) - REST API handler library. **REST API处理程序库。**
-   [Blynk](https://github.com/blynkkk/blynk-library) - easy IoT framework for Makers (check out the [Kickstarter page](http://tiny.cc/blynk-kick)). 为 makers 提供简单的IoT框架（查看Kickstarter页面）。
-   [DallasTemperature](https://github.com/milesburton/Arduino-Temperature-Control-Library.git) 达拉斯温度
-   [DHT-sensor-library](https://github.com/adafruit/DHT-sensor-library) - Arduino library for the DHT11/DHT22 temperature and humidity sensors. Download latest v1.1.1 library and no changes are necessary. Older versions should initialize DHT as follows: `DHT dht(DHTPIN, DHTTYPE, 15)`
    **用于DHT11 / DHT22 温度和湿度传感器的Arduino库。下载最新的v1.1.1库，不需要更改。旧版本应该初始化DHT如下：DHT dht（DHTPIN，DHTTYPE，15）**
-   [DimSwitch](https://github.com/krzychb/DimSwitch) - Control electronic dimmable ballasts for fluorescent light tubes remotely as if using a wall switch. **远程控制荧光灯管的电子调光镇流器，就像使用墙壁开关一样。**
-   [Encoder](https://github.com/PaulStoffregen/Encoder) - Arduino library for rotary encoders. Version 1.4 supports ESP8266.**用于旋转编码器的Arduino库。版本1.4支持ESP8266。**
-   [esp8266_mdns](https://github.com/mrdunk/esp8266_mdns) - mDNS queries and responses on esp8266. Or to describe it another way: An mDNS Client or Bonjour Client library for the esp8266.**esp8266上的 mDNS 查询和响应。或者以另一种方式来描述它：用于esp8266的mDNS客户端或Bonjour客户端库。**
-   [ESPAsyncTCP](https://github.com/me-no-dev/ESPAsyncTCP) - Asynchronous TCP Library for ESP8266 and ESP32/31B **ESP8266和ESP32 / 31B的异步TCP库**
-   [ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer) - Asynchronous Web Server Library for ESP8266 and ESP32/31B **ESP8266和ESP32/31B的异步Web服务器库**
-   [Homie for ESP8266](https://github.com/marvinroger/homie-esp8266) - Arduino framework for ESP8266 implementing Homie, an MQTT convention for the IoT.**实现Homie的ESP8266的Arduino框架，物联网的MQTT约定**
-   [NeoPixel](https://github.com/adafruit/Adafruit_NeoPixel) - Adafruit's NeoPixel library, now with support for the ESP8266 (use version 1.0.2 or higher from Arduino's library manager).**Adafruit的NeoPixel库，现在支持ESP8266（使用版本1.0.2或更高版本的Arduino的库管理器）。**
-   [NeoPixelBus](https://github.com/Makuna/NeoPixelBus) - Arduino NeoPixel library compatible with ESP8266. Use the "DmaDriven" or "UartDriven" branches for ESP8266. Includes HSL color support and more.**Arduino NeoPixel库与ESP8266兼容。对ESP8266使用“DmaDriven”或“UartDriven”分支。包括HSL颜色支持和更多。**
-   [PubSubClient](https://github.com/Imroy/pubsubclient) - MQTT library by @Imroy.**由@Imroy的MQTT库。**
-   [RTC](https://github.com/Makuna/Rtc) - Arduino Library for Ds1307 & Ds3231 compatible with ESP8266.**Ds1307和Ds3231的Arduino库与ESP8266兼容。**
-   [Souliss, Smart Home](https://github.com/souliss/souliss) - Framework for Smart Home based on Arduino, Android and openHAB.**基于Arduino，Android和openHAB的智能家居框架。**
-   [ST7735](https://github.com/nzmichaelh/Adafruit-ST7735-Library) - Adafruit's ST7735 library modified to be compatible with ESP8266. Just make sure to modify the pins in the examples as they are still AVR specific.**Adafruit的 ST7735库已修改为与ESP8266兼容。只需确保修改示例中的引脚，因为它们仍然是AVR特定的。**
-   [Task](https://github.com/Makuna/Task) - Arduino Nonpreemptive multitasking library. While similiar to the included Ticker library in the functionality provided, this library was meant for cross Arduino compatibility.**任务 - Arduino Nonpreemptive多任务库。虽然类似于所提供的功能中包括的 Ticker 库，但此库是为了实现跨Arduino兼容性。**
-   [TickerScheduler](https://github.com/Toshik/TickerScheduler) - Library provides simple scheduler for `Ticker` to avoid WDT reset **Library为Ticker提供简单的调度程序，以避免WDT复位**
-   [Teleinfo](https://github.com/hallard/LibTeleinfo) - Generic French Power Meter library to read Teleinfo energy monitoring data such as consuption, contract, power, period, ... This library is cross platform, ESP8266, Arduino, Particle, and simple C++. French dedicated [post](https://hallard.me/libteleinfo/) on author's blog and all related information about [Teleinfo](https://hallard.me/category/tinfo/) also available.**通用法语功率计库，用于读取Teleinfo能量监测数据，如消耗，合同，功率，周期等。这个库是跨平台，ESP8266，Arduino，粒子和简单的C ++。法语专门文章作者的博客和所有相关信息关于Teleinfo也可用。**
-   [UTFT-ESP8266](https://github.com/gnulabis/UTFT-ESP8266) - UTFT display library with support for ESP8266. Only serial interface (SPI) displays are supported for now (no 8-bit parallel mode, etc). Also includes support for the hardware SPI controller of the ESP8266.**支持ESP8266的UTFT显示库。目前仅支持串行接口（SPI）显示（无8位并行模式等）。还包括对ESP8266的硬件SPI控制器的支持。**
-   [WiFiManager](https://github.com/tzapu/WiFiManager) - WiFi Connection manager with web captive portal. If it can't connect, it starts AP mode and a configuration portal so you can choose and enter WiFi credentials.**WiFi连接管理器与网络强制门户。如果它无法连接，它会启动AP模式和配置门户，以便您可以选择并输入WiFi凭据。**
-   [OneWire](https://github.com/PaulStoffregen/OneWire) - Library for Dallas/Maxim 1-Wire Chips.**达拉斯/ Maxim 1-Wire芯片库。**
-   [Adafruit-PCD8544-Nokia-5110-LCD-Library](https://github.com/WereCatf/Adafruit-PCD8544-Nokia-5110-LCD-library) - Port of the Adafruit PCD8544 - library for the ESP8266.
-   [PCF8574_ESP](https://github.com/WereCatf/PCF8574_ESP) - A very simplistic library for using the PCF8574/PCF8574A I2C 8-pin GPIO-expander.**使用PCF8574 / PCF8574A I2C 8针GPIO扩展器的非常简单的库。**
-   [Dot Matrix Display Library 2](https://github.com/freetronics/DMD2) - Freetronics DMD & Generic 16 x 32 P10 style Dot Matrix Display Library **Freetronics DMD＆Generic 16 x 32 P10风格点阵显示库**
-   [SdFat-beta](https://github.com/greiman/SdFat-beta) - SD-card library with support for long filenames, software- and hardware-based SPI and lots more. **SD卡库支持长文件名，基于软件和硬件的SPI和更多。**
-   [FastLED](https://github.com/FastLED/FastLED) - a library for easily & efficiently controlling a wide variety of LED chipsets, like the Neopixel (WS2812B), DotStar, LPD8806 and many more. Includes fading, gradient, color conversion functions.**用于轻松高效地控制各种LED芯片组的库，如Neopixel（WS2812B），DotStar，LPD8806等等。包括褪色，渐变，颜色转换功能。**
-   [OLED](https://github.com/klarsys/esp8266-OLED) - a library for controlling I2C connected OLED displays. Tested with 0.96 inch OLED graphics display.**用于控制I2C连接的OLED显示器的库。测试与0.96英寸OLED图形显示。**
-   [MFRC522](https://github.com/miguelbalboa/rfid) - A library for using the Mifare RC522 RFID-tag reader/writer.**使用Mifare RC522 RFID标签读写器的库。**
-   [Ping](https://github.com/dancol90/ESP8266Ping) - lets the ESP8266 ping a remote machine.**让ESP8266 ping 远程机器。**
-   [AsyncPing](https://github.com/akaJes/AsyncPing) - fully asynchronous Ping library (have full ping statistic and hardware MAC address).**完全异步Ping库（具有完整的ping统计信息和硬件MAC地址）。**