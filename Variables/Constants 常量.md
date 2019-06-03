# Constants 常量
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

常量是在 Arduino 语言中预定义的表达式。
它们被用来使程序更易阅读。
我们把常量进行分组：

Constants are predefined expressions in the Arduino language. 
They are used to make the programs easier to read. 
We classify constants in groups:

##  1. 定义逻辑层次：true and false(布尔常量)
Defining Logical Levels: true and false (Boolean Constants)

在 Arduino 中有两个常量用来表示真和假：true和 false。
There are two constants used to represent代表 truth and falsity in the Arduino language: true, and false.

### 1.1 false
在这两个常量中 false 更容易被定义。
false 被定义为0。

>false is the easier of the two to define. 
>false is defined as 0 (zero).

###  1.2 true
true 通来说被定义为 1，这是正确的，但是 true 有更宽泛的定义。
在boolean意义上，任何非 0 的整数都是ture
因此在boolean意义上，-1, 2 和 -200都被都被定义为真。

注意：true 和 false 常数是输入小写字母，不像HIGH, LOW, INPUT, and OUTPUT。

>true is often said to be defined as 1, which is correct, but true has a wider definition. 
>Any integer which is non-zero is true, in a Boolean sense. 
>So -1, 2 and -200 are all defined as true, too, in a Boolean sense.

>Note that the true and false constants are typed in lowercase unlike HIGH, LOW, INPUT, and OUTPUT.

## 2. 定义引脚层次：HIGH and LOW
Defining Pin Levels: HIGH and LOW
当读取（read）或写入（write）数字引脚时只有两个可能的值： HIGH 和 LOW 。
>When reading or writing to a digital pin there are only two possible values a pin can take/be-set-to: HIGH and LOW.

###  2.1 HIGH
HIGH（参考一个引脚）的意义稍有不同，取决于一个引脚被设置为 INPUT 或 OUTPUT 两个中的哪一个。
当引脚通过pinMode()被配置为INPUT，并用digitalRead()读取，Arduino (Atmega) 将报告HIGH，假如：
5V 电源的板卡上，大于3V的电压出现在该引脚。
3.3V 电源的板卡上，大于2V的电压出现在该引脚。

一个引脚通过 pinMode() 设置为 INPUT ，并且随后通过 diditalWrite() 设置为高。
这样会开启内部 20K 上拉电阻，这将上拉输入引脚到一个HIGH读数，除非通过外部电路拉LOW。
这是 INPUT_PULLUP 如何工作的，更多细节描述查看后面的内容。

当一个引脚通过pinMode()被设置为OUTPUT，通过 digitalWrite() 设置为HIGH时，引脚的电压如下：
5 volts (5V boards);
3.3 volts (3.3V boards);
在这种状态下，它可以输出电流。例如连接 LED 灯并通过串联电阻连接到地。

>The meaning of HIGH (in reference to a pin) is somewhat different depending on取决于 whether a pin is set to an INPUT or OUTPUT. 
>When a pin is configured配置 as an INPUT with pinMode(), and read with digitalRead(), the Arduino (Atmega) will report HIGH if:
>a voltage greater大于 than 3 volts is present出现 at the pin (5V boards);
>a voltage greater大于 than 2 volts is present出现 at the pin (3.3V boards);

>A pin may also be configured as an INPUT with pinMode(), and subsequently随后 made HIGH with digitalWrite(). 
>This will enable the internal 20K pullup resistors, which will pull up the input pin to a HIGH reading unless it is pulled LOW by external circuitry. 
>This is how INPUT_PULLUP works and is described描述 below in more detail.

>When a pin is configured to OUTPUT with pinMode(), and set to HIGH with digitalWrite(), the pin is at:
>5 volts (5V boards);
>3.3 volts (3.3V boards);
>In this state it can source current, e.g. light an LED that is connected through a series resistor to ground.



###  2.2 LOW
LOW 的含义有不同的意义，取决于该引脚被设置为 INPUT 或 OUTPUT 中的哪一个。
当引脚通过 pinMode() 被设置为 INPUT ，并使用 digitalRead() 进行读， Arduino (Atmega) 将在下列情况报告 LOW：
a voltage less than 3 volts is present at the pin (5V boards);
a voltage less than 2 volts is present at the pin (3.3V boards);

当引脚通过 pinMode() 被配置为 OUTPUT，并通过 digitalWrite() 设置为 LOW ，那么该引脚的电压为 0 V(both 5V and 3.3V boards).。
这种状态会吸入电流，例如 点亮 LED ( LED 通过串联电阻连接到 5 volts (or +3.3 volts))。

>The meaning of LOW also has a different meaning depending on取决于 whether a pin is set to INPUT or OUTPUT. 
>When a pin is configured as an INPUT with pinMode(), and read with digitalRead(), the Arduino (Atmega) will report报告 LOW if:
>a voltage less than 3 volts is present at the pin (5V boards);
>a voltage less than 2 volts is present at the pin (3.3V boards);

>When a pin is configured配置 to OUTPUT with pinMode(), and set to LOW with digitalWrite(), the pin is at 0 volts (both 5V and 3.3V boards). 
>In this state状态 it can sink吸收 current, e.g. light an LED that is connected through a series resistor to +5 volts (or +3.3 volts).


## 3. 定义数字引脚模式：INPUT, INPUT_PULLUP, and OUTPUT
Defining Digital Pins modes: INPUT, INPUT_PULLUP, and OUTPUT

数字引脚可被用作 INPUT, INPUT_PULLUP, 或 OUTPUT。
改变引脚的 pinMode() 会改变引脚的电气特性。

>Digital pins can be used as INPUT, INPUT_PULLUP, or OUTPUT. Changing a pin with pinMode() changes the electrical behavior of the pin.

### 3.1 配置引脚为 INPUT
Pins Configured as INPUT

使用 pinMode() 将 Arduino (Atmega) 的引脚配置为 INPUT，意味着该引脚被配置为高阻抗状态。
引脚被配置为 INPUT 时，意味着引脚取样时，对电路有极小的需求，即等效于在引脚前串联一个100兆欧的电阻。
这种配置在读取传感器状态时非常有用。

如果你将引脚配置为 INPUT，并用其阅读一个开关量， 当开关处于打开状态时，输入引脚处于"floating"状态，将导致不可预测的结果。
在开关被打开时，为了确保一个恰当的值被读取，必须使用一个上拉或下拉电阻。
这个电阻的作用是当开关被打开时，拉动引脚到一个已知状态。
通常会选择一个 10K 欧的电阻，因为它是一个足够低的值，以可靠的防止 floating 输入；同时当开关闭合时不至于产生过高的电流值。
参考  Digital Read Serial 以获得更多信息。
https://www.arduino.cc/en/Tutorial/DigitalReadSerial

假如使用了下拉电阻，在开关打开时 input 引脚将被设置为LOW ；当开关闭合时，引脚被设置为 HIGH。
假如使用了上拉电阻，在开关断开时 input 引脚将被设置为HIGH；当开关闭合时，引脚被设置为 LOW。

>Arduino (Atmega) pins configured as INPUT with pinMode() are said to be in a high-impedance state. Pins configured as INPUT make extremely small demands on the circuit that they are sampling, equivalent to a series resistor of 100 Megohms in front of the pin. This makes them useful for reading a sensor.

>If you have your pin configured as an INPUT, and are reading a switch, when the switch is in the open state the input pin will be "floating", resulting in unpredictable results. In order to assure a proper reading when the switch is open, a pull-up or pull-down resistor must be used. The purpose of this resistor is to pull the pin to a known state when the switch is open. A 10 K ohm resistor is usually chosen, as it is a low enough value to reliably prevent a floating input, and at the same time a high enough value to not not draw too much current when the switch is closed. See the Digital Read Serial tutorial for more information.

>If a pull-down resistor is used, the input pin will be LOW when the switch is open and HIGH when the switch is closed.

>If a pull-up resistor is used, the input pin will be HIGH when the switch is open and LOW when the switch is closed.

### 3.2 配置引脚为 INPUT_PULLUP
Pins Configured as INPUT_PULLUP

Arduino 上的 Atmega 微控器具有内部上拉电阻(电阻被连接到内部电源)，你可以访问该电阻。如果你愿意使用内部上拉电阻替代外部上拉电阻，你可以在 pinMode () 中使用 INPUT_PULLUP 参数。

参考 Input Pullup Serial 获得更多信息。
https://www.arduino.cc/en/Tutorial/InputPullupSerial

引脚被配置为 input (INPUT 或 INPUT_PULL中的一个)，假如该引脚被连接到低于 ground 的电压(负电压)，或高于正电压(5V or 3V)时，该引脚可能被损坏或烧毁。

>The Atmega microcontroller on the Arduino has internal pull-up resistors (resistors that connect to power internally) that you can access. If you prefer to use these instead of external pull-up resistors, you can use the INPUT_PULLUP argument in pinMode().

>See the Input Pullup Serial tutorial for an example of this in use.

>Pins configured as inputs with either INPUT or INPUT_PULLUP can be damaged or destroyed if they are connected to voltages below ground (negative voltages) or above the positive power rail (5V or 3V).

### 3.3 配置引脚为 Outputs
Pins Configured as Outputs

引脚通过pinMode()配置为 输出（OUTPUT） 即是将其配置在一个低阻抗的状态。这意味着它们可以为外部电路提供充足的电流。
Atmega 引脚可作为source(提供电流)或作为sink(吸收电流)，向其它设备/电路提供达40 mA (milliamps) 的电流。
这使该引脚可用于向 LED 供电，因为LED 的典型电流值小于 40mA.
负载超过 40mA 时(e.g. motors)，需要使用晶体管或其它接口电路。
output 在连接到 ground 或正电源线时，可能被损坏或烧毁。
输出（OUTPUT）引脚被短路的接地或5V电路上会受到损坏甚至烧毁。

>Pins configured as OUTPUT with pinMode() are said to be in a low-impedance state. This means that they can provide a substantial amount of current to other circuits. Atmega pins can source (provide current) or sink (absorb current) up to 40 mA (milliamps) of current to other devices/circuits. This makes them useful for powering LEDs because LEDs typically use less than 40 mA. Loads greater than 40 mA (e.g. motors) will require a transistor or other interface circuitry.

>Pins configured as outputs can be damaged or destroyed if they are connected to either the ground or positive power rails.

## 4. 定义内建 LDE：LED_BUILTIN
Defining built-ins: LED_BUILTIN

大多数 Arduino 有一个引脚连接到一个板载 LED，并且该LED 串联了一个电阻。
常量 LED_BUILTIN 是指板载 LED 所连接的引脚。
大多数板卡在13数字引脚连接一个 LED

>Most Arduino boards have a pin connected to an on-board LED in series with a resistor. The constant LED_BUILTIN is the number of the pin to which the on-board LED is connected. Most boards have this LED connected to digital pin 13.

## 5. 整型常量 Integer Constants
Integer Constants
整数常量是直接在 sketch 中使用的数字，如123。
默认情况下，这些数字被被处理为 int，但是你可以用 U 和 L修饰符进行修改(见下文)。
 通常情况下，整数常量默认为十进制(decimal)，但可以加上特殊标记 (formatters) ，以用于输入其他进制的整型常量。

| Base             |  Example | Formatter    | Comment注释                                                  |
| :--------------- | -------: | :----------- | :----------------------------------------------------------- |
| 10 (decimal)     |      123 | none         |                                                              |
| 2 (binary)       | B1111011 | leading 'B'  | only works with 8 bit values (0 to 255)   characters 0-1 valid 只适用于8位的值（0到255）字符0-1有效 |
| 8 (octal)        |     0173 | leading "0"  | characters 0-7 valid 字符0-7有效                             |
| 16 (hexadecimal) |     0x7B | leading "0x" | characters 0-9, A-F, a-f valid  字符0-9，A-F，a-f有效        |

**Decimal小数**是10进制数。这是数学常识。
如果一个常数数没有特定的前缀，则默认处理为十进制。

Example:
```
101     // same as 101 decimal   ((1 * 10^2) + (0 * 10^1) + 1)
```

**Binary 二进**制以2为基底，只有数字0和1是有效的。
Example:
```
B101    // same as 5 decimal   ((1 * 2^2) + (0 * 2^1) + 1)
```
二进制格式只能是8位(一个字节)，在0 (B0) 和 255 (B11111111) 之间。在二进制格式下输入int (16 bits)，可以通过以下两个步骤。
```
myInt = (B11001100 * 256) + B10101010;    // B11001100 is the high byte
```

**Octal八进制**基于8 的底数，只有0-7是有效的字符。前缀“0”（数字0）表示该值为八进制。

Example:
```
0101    // same as 65 decimal   ((1 * 8^2) + (0 * 8^1) + 1) 
```
警告：八进制数0前缀很可能无意产生很难发现的错误，因为你可能不小心在常量前加了个“0”，结果就悲剧了

**Hexadecimal (or hex)十六进制**十六进制以16为基底，有效的字符为0-9和A-F。A的值是10，B的值是11...F的值是15.十六进制数用前缀“0x”（数字0，字母爱克斯）表示。请注意，A-F不区分大小写，就是说你也可以用a-f。

Example:
```
0x101   // same as 257 decimal   ((1 * 16^2) + (0 * 16^1) + 1)
```

**U & L formatters**
默认情况下，整型常量被视作int型。要将整型常量转换为其他类型时，请遵循以下规则：

'u' or 'U' 指定一个常量为无符号型。（只能表示正数和0） 例如: 33u
'l' or 'L' 指定一个常量为长整型。（表示数的范围更广） 例如: 100000L
'ul' or 'UL' 这个你懂的，就是上面两种类型，称作无符号长整型。 例如：32767ul


>Integer constants are numbers used directly in a sketch, like 123. By default, these numbers are treated as int's but you can change this with the U and L modifiers (see below).

>Normally, integer constants are treated as base 10 (decimal) integers, but special notation (formatters) may be used to enter numbers in other bases.

>Decimal is base 10. This is the common-sense math with which you are acquainted. Constants without other prefixes are assumed to be in decimal format.

>Binary is base two. Only characters 0 and 1 are valid.

>The binary formatter only works on bytes (8 bits) between 0 (B0) and 255 (B11111111). If it is convenient to input an int (16 bits) in binary form you can do it a two-step procedure such as:

>Octal is base eight. Only characters 0 through 7 are valid. Octal values are indicated by the prefix "0"

>Warning
>It is possible to generate a hard-to-find bug by (unintentionally) including a leading zero before a constant and having the compiler unintentionally interpret your constant as octal.

>Hexadecimal (or hex) is base sixteen. Valid characters are 0 through 9 and letters A through F; A has the value 10, B is 11, up to F, which is 15. Hex values are indicated by the prefix "0x". Note that A-F may be syted in upper or lower case (a-f).

>U & L formatters

>By default, an integer constant is treated as an int with the attendant limitations in values. To specify an integer constant with another data type, follow it with:

>a 'u' or 'U' to force the constant into an unsigned data format. Example: 33u
>a 'l' or 'L' to force the constant into a long data format. Example: 100000L
>a 'ul' or 'UL' to force the constant into an unsigned long constant. Example: 32767ul

## 6. 浮点常量 floating point constants
类似于整型常量，浮点常量用于使代码更具可读性。
浮点常量在编译时被转换为其表达式所取的值。
Examples:
```
n = .005;
```
 浮点数可以用科学记数法表示。
 'E'和'e'都可以作为有效的指数标志。

| floating-point constant | evaluates to: 计算结果 | also evaluates to: |
| :---------------------- | ---------------------: | :----------------: |
| 10.0                    |                     10 |       field3       |
| 2.34E5                  |            2.34 * 10^5 |       234000       |
| 67e-12                  |          67.0 * 10^-12 |   .000000000067    |


Similar to integer constants, floating point constants are used to make code more readable. Floating point constants are swapped at compile time for the value to which the expression evaluates.

Floating point constants can also be expressed in a variety of scientific notation. 'E' and 'e' are both accepted as valid exponent indicators.