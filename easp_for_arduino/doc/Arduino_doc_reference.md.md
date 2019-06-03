# [Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/**reference.md**



[TOC]

## Digital IO 数字IO

Arduino 中的引脚编号直接对应于 ESP8266 GPIO 引脚编号。`pinMode` , `degitalRead` 和 `digitalWrite` 还是以同样的方式工作，因此要读取 GPIO2 ，可调用 `digitalRead(2)`。

数字引脚 0 -15 可以使用  `INPUT`, `OUTPUT`, 或 `INPUT_PULLUP`。 引脚 16 可使用  `INPUT`, `OUTPUT` or `INPUT_PULLDOWN_16`。 在启动时，引脚被配置为 `INPUT`。

引脚还可以提供其他功能，如 Serial, I2C, SPI。这些功能通常由相应的库激活。下图展示了常用 ESP-12 模块的引脚映射关系。

![esp12](esp12.png)

数字引脚 6 - 11 在此图中未显示，因为它们在大多数 ESP 模块中被用于连接闪存芯片。尝试使用这些引脚作为 IOs 可能会导致程序崩溃。

注意，一些点了版和模块 (ESP-12ED, NodeMCU 1.0)还会断开引脚 9 和 11。如果闪存芯片工作在 DIO 模式时，这两个引脚被用作 IO(与 QIO 相同，这是默认模式)。

引脚中断通过  `attachInterrupt`, `detachInterrupt` 函数提供支持。中断可以连接到任何 GPIO 引脚，但 GPIO 16 除外。支持标准的 Arduino 中断类型： `CHANGE`, `RISING`, `FALLING`.

下面是 DIO 和 QIO 的区别：

What is the difference between QIO and DIO flash mode? - See more at: http://www.esp8266.com/viewtopic.php?f=33&t=3838

Quad IO uses 4 lines for data for up to 4 times the speed of standard.
Dual IO uses 2 lines for data
Standard uses a single line for data

You need to use the mode that the flash part supports. The datasheet for the part should describe the modes that it supports. The HW design also has to have the lines connected. - See more at: http://www.esp8266.com/viewtopic.php?f=33&t=3838#sthash.xDF4zyKw.dpuf



>   Pin numbers in Arduino correspond directly to the ESP8266 GPIO pin numbers. `pinMode`, `digitalRead`, and `digitalWrite`functions work as usual, so to read GPIO2, call `digitalRead(2)`.
>
>   Digital pins 0—15 can be `INPUT`, `OUTPUT`, or `INPUT_PULLUP`. Pin 16 can be `INPUT`, `OUTPUT` or `INPUT_PULLDOWN_16`. At startup, pins are configured as `INPUT`.
>
>   Pins may also serve other functions, like Serial, I2C, SPI. These functions are normally activated by the corresponding library. The diagram below shows pin mapping for the popular ESP-12 module.
>
>   
>
>   Digital pins 6—11 are not shown on this diagram because they are used to connect flash memory chip on most modules. Trying to use these pins as IOs will likely cause the program to crash.
>
>   Note that some boards and modules (ESP-12ED, NodeMCU 1.0) also break out pins 9 and 11. These may be used as IO if flash chip works in DIO mode (as opposed to QIO, which is the default one).
>
>   Pin interrupts are supported through `attachInterrupt`, `detachInterrupt` functions. Interrupts may be attached to any GPIO pin, except GPIO16. Standard Arduino interrupt types are supported: `CHANGE`, `RISING`, `FALLING`.

## Analog input  模拟输入

ESP8266 具有单个 ADC 通道可供用户使用。 它可用于读取 ADC 引脚的电压或读取模块电源电压 (VCC)。

要读取施加到ADC引脚的外部电压，请使用 `analogRead(A0)`。 输入电压范围为 0 — 1.0V。

要读取VCC电压，使用 `ESP.getVcc()` 和 ADC 引脚必须保持未连接。 此外，必须将以下行添加到 sketch 中：

```
ADC_MODE(ADC_VCC);
```

这行必须出现在任何函数之外，例如在你的草图的 `#include` 行之后。

>   ESP8266 has a single ADC channel available to users. It may be used either to read voltage at ADC pin, or to read module supply voltage (VCC).
>
>   To read external voltage applied to ADC pin, use `analogRead(A0)`. Input voltage range is 0 — 1.0V.
>
>   To read VCC voltage, use `ESP.getVcc()` and ADC pin must be kept unconnected. Additionally, the following line has to be added to the sketch:
>
>   This line has to appear outside of any functions, for instance right after the `#include` lines of your sketch.

## Analog output 模拟输出

`analogWrite(pin, value)`  在给定引脚上使能 software PWM 。 PWM可以在引脚0至16上使用。调用`analogWrite(pin, 0)`以禁止引脚上的 PWM。 `value` 可以在0 到 `PWMRANGE` 的范围内，默认情况下等于1023。 可以通过调用 `analogWriteRange(new_range)` 来更改 PWM 范围。

默认PWM频率为1kHz。 调用 `analogWriteFreq(new_frequency)` 来改变频率。

>   `analogWrite(pin, value)` enables software PWM on the given pin. PWM may be used on pins 0 to 16. Call `analogWrite(pin, 0)` to disable PWM on the pin. `value` may be in range from 0 to `PWMRANGE`, which is equal to 1023 by default. PWM range may be changed by calling `analogWriteRange(new_range)`.
>
>   PWM frequency is 1kHz by default. Call `analogWriteFreq(new_frequency)` to change the frequency.

## Timing and delays 时间和延迟

`millis()` 和`micros()` 分别返回上次复位后到现在为止所经过的毫秒数和微秒数。

`delay(ms)` 将在给定的毫秒数内暂停 sketch 的执行，并在此时间内允许 WIFI 和 TCP/IP 任务的执行。`delayMicroseconds(us)` 在给定的微秒数内暂停程序的执行。

记住，当 WIFI 连接时，除 sketch 内的程序之外，还有很多代码需要在芯片上运行。WiFi 和 TCP/IP 库有机会在每次 `loop()` 函数完成时处理任何挂起的事件，或者当调用 delay 时。如果你的 sketch 中有一个 loop 需要花费大量时间(>50ms)，并且期间没有调用 delay，你可以考虑添加一个 delay 调用，以保持 WiFi 堆栈平稳运行。

还有一个 `yield()` 函数等效于 `delay(0)` 。另一方面，`delayMicroseconds` 函数不会产生其他任务，因此不推荐使用它超过20毫秒的延迟。

>   `millis()` and `micros()` return the number of milliseconds and microseconds elapsed after reset, respectively.
>
>   `delay(ms)` pauses the sketch for a given number of milliseconds and allows WiFi and TCP/IP tasks to run.`delayMicroseconds(us)` pauses for a given number of microseconds.
>
>   Remember that there is a lot of code that needs to run on the chip besides the sketch when WiFi is connected. WiFi and TCP/IP libraries get a chance to handle any pending events each time the `loop()` function completes, OR when `delay` is called. If you have a loop somewhere in your sketch that takes a lot of time (>50ms) without calling `delay`, you might consider adding a call to `delay` function to keep the WiFi stack running smoothly.
>
>   There is also a `yield()` function which is equivalent to `delay(0)`. The `delayMicroseconds` function, on the other hand, does not yield to other tasks, so using it for delays more than 20 milliseconds is not recommended.

## Serial 串行输出

`Serial` 对象的工作方式与常规的 Arduino 非常相似。除了硬件  FIFO (128 bytes for TX and RX) HardwareSerial 具有额外的  256-byte TX and RX 缓冲区。发送和接受都是 interrupt-driven 。当 相应的 FIFO/buffers 为 full/empty 时，写入和读取功能仅阻止草图执行。

`Serial` 使用UART0，它映射到引脚  GPIO1 (TX) 和 GPIO3 (RX)。通过在 `Serial.begin` 之后调用 `Serial.swap()` ，可以将串行重新映射到 GPIO15 (TX) and GPIO13 (RX) 。再次调用 `swap` 将 UART0 映射回 GPIO1和 GPIO3。

`Serial1` 使用 UART1，TX 引脚为 GPIO2。 UART1 不能用于接收数据，因为通常它的 RX 引脚被占用，并用做闪存芯片连接。要使用 `Serial1` ，请调用  `Serial1.begin(baudrate)`。

如果不使用 `Serial1` 并且不交换 `Serial`  - 可以将 UART0 的 TX 映射到GPIO2，通过在 `Serial.begin` 之后调用`Serial.set_tx(2)` 或直接使用 `Serial.begin(baud, config, mode, 2)` 。

默认情况下，当调用 `Serial.begin` 时，禁用 WiFi 库的诊断输出。要再次启用 debug 输出，请调用 `Serial.setDebugOutput(true)`。要将调试输出重定向到 `Serial1` ，请调用 `Serial1.setDebugOutput(true)`。

您还需要使用 `Serial.setDebugOutput(true)` 已从 `printf()` 函数启用输出。

 `Serial` and `Serial1` 对象都支持5, 6, 7, 8 个数据位，奇odd (O)，偶even (E) (E) 和 no (N) 奇偶校验，以及1或2个停止位。要设置所需的模式，请调用l `Serial.begin(baudrate, SERIAL_8N1)`, `Serial.begin(baudrate, SERIAL_6E2)` 等。

在 `Serial` and `Serial1` 上都实现了一种新的方法来获得当前的波特率设置。要获取当前的波特率，请调用 `Serial.baudRate()`, `Serial1.baudRate()`。返回当前速度的整数。例如

```
// Set Baud rate to 57600
Serial.begin(57600);

// Get current baud rate
int br = Serial.baudRate();

// Will print "Serial is 57600 bps"
Serial.printf("Serial is %d bps", br);
```

我也为官方ESP8266 [Software Serial](https://github.com/esp8266/Arduino/blob/master/doc/libraries.md#softwareserial) library 提供这样的支持，看 [pull request](https://github.com/plerup/espsoftwareserial/pull/22)。

注意，这个实现仅用于基于ESP8266的板，并且不能与其他 Arduino 板一起工作

>   `Serial` object works much the same way as on a regular Arduino. Apart from hardware FIFO (128 bytes for TX and RX) HardwareSerial has additional 256-byte TX and RX buffers. Both transmit and receive is interrupt-driven. Write and read functions only block the sketch execution when the respective FIFO/buffers are full/empty.
>
>   `Serial` uses UART0, which is mapped to pins GPIO1 (TX) and GPIO3 (RX). Serial may be remapped to GPIO15 (TX) and GPIO13 (RX) by calling `Serial.swap()` after `Serial.begin`. Calling `swap` again maps UART0 back to GPIO1 and GPIO3.
>
>   `Serial1` uses UART1, TX pin is GPIO2. UART1 can not be used to receive data because normally it's RX pin is occupied for flash chip connection. To use `Serial1`, call `Serial1.begin(baudrate)`.
>
>   If `Serial1` is not used and `Serial` is not swapped - TX for UART0 can be mapped to GPIO2 instead by calling `Serial.set_tx(2)` after `Serial.begin` or directly with `Serial.begin(baud, config, mode, 2)`.
>
>   By default the diagnostic output from WiFi libraries is disabled when you call `Serial.begin`. To enable debug output again, call `Serial.setDebugOutput(true)`. To redirect debug output to `Serial1` instead, call  `Serial1.setDebugOutput(true)`.
>
>   You also need to use `Serial.setDebugOutput(true)` to enable output from `printf()` function.
>
>   Both `Serial` and `Serial1` objects support 5, 6, 7, 8 data bits, odd (O), even (E), and no (N) parity, and 1 or 2 stop bits. To set the desired mode, call `Serial.begin(baudrate, SERIAL_8N1)`, `Serial.begin(baudrate, SERIAL_6E2)`, etc.
>
>   A new method has been implemented on both `Serial` and `Serial1` to get current baud rate setting. To get the current baud rate, call `Serial.baudRate()`, `Serial1.baudRate()`. Return a `int` of current speed. For example
>
>   
>
>   I've done this also for official ESP8266 [Software Serial](https://github.com/esp8266/Arduino/blob/master/doc/libraries.md#softwareserial) library, see this [pull request](https://github.com/plerup/espsoftwareserial/pull/22).
>   Note that this implementation is **only for ESP8266 based boards**, and will not works with other Arduino boards.

## Progmem 程序存储器

程序存储器功能的工作方式与常规 Arduino 相同，将只读数据和字符串放在只读内存中，并为您的应用程序释放堆。 重要的区别是，在ESP8266上，文字字符串不会被合并。 这意味着在 `F("")` 和/或 `PSTR("")` 内定义的相同文字字符串将在代码的中的每个不同实例下都占用相应的空间。所以你需要自己管理重复的字符串。

还有一个额外的助手宏，使得传递 `const PROGMEM` 所创的字符串到一个名为 `FPSTR()` 的 `__FlashStringHelper` 的方法更加便利。使用这将有助于更容易地合并字符串。 

>   Not pooling strings... 没有进行字符串合并的示例：

```
String response1;
response1 += F("http:");//两处 F("http:") 并不会自动合并。
...
String response2;
response2 += F("http:");
```

>   using FPSTR would become... 使用 FPSTR 后，可以轻松合并：

```
const char HTTP[] PROGMEM = "http:";
...
{
    String response1;
    response1 += FPSTR(HTTP);
    ...
    String response2;
    response2 += FPSTR(HTTP);
}
```
如果不理解 String Literal Pool 可参考这个连接 [What is String literal pool?](http://www.xyzws.com/javafaq/what-is-string-literal-pool/3)

>   The Program memory features work much the same way as on a regular Arduino; placing read only data and strings in read only memory and freeing heap for your application. The important difference is that on the ESP8266 the literal strings are not pooled. This means that the same literal string defined inside a `F("")` and/or `PSTR("")` will take up space for each instance in the code. So you will need to manage the duplicate strings yourself.
>
>   There is one additional helper macro to make it easier to pass `const PROGMEM` strings to methods that take a `__FlashStringHelper` called `FPSTR()`. The use of this will help make it easier to pool strings. 