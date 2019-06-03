# External Interrupts 外部中断
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. attachInterrupt()
**描述**
中断数字引脚
当发生外部中断时，调用一个指定函数。当中断发生时，该函数会取代正在执行的程序。
attachInterrupt() 中的第一个参数是中断号。
通常你会使用 digitalPinToInterrupt(pin) 以将实际的数字引脚转化为特定中断号。
例如，假如你连接引脚 3，使用 digitalPinToInterrupt(3) 作为
attachInterrupt() 的第一个参数。

| Board                             | Digital Pins Usable For Interrupts |
| :-------------------------------- | :--------------------------------- |
| Uno, Nano, Mini, other 328-based  | 2, 3                               |
| Mega, Mega2560, MegaADK           | 2, 3, 18, 19, 20, 21               |
| Micro, Leonardo, other 32u4-based | 0, 1, 2, 3, 7                      |
| Zero                              | all digital pins, except除了 4     |
| MKR1000 Rev.1                     | 0, 1, 4, 5, 6, 7, 8, 9, A1, A2     |
| Due                               | all digital pins                   |
| 101                               | all digital pins                   |

**注意**
在 attached 函数内部，delay() 将不工作，millis() 的返回值将不会增加。
在函数中同时接受串行数据，可能会丢失。
 在 attached 函数内你修改的任意变量，应该声明为`volatile`易失性。
更多信息请看后 ISRs 一节。

**使用中断**
在单片机自动化程序中当突发事件发生时，中断是非常有用的，它可以帮助解决时序问题。一个使用中断任务的程序可能会读一个旋转编码器，监视用户的输入。

如果你想以确保程序始终抓住一个旋转编码器的脉冲，从来不缺少一个脉冲，这将使写一个程序做别的任何事情，变的非常棘手，因为该程序需要不断地传感器在线编码器，以便脉冲发生时能够赶上。
其它传感器也有相似的动态接口，例如尝试读取声音的传感器，尝试捕捉单击声；或红外线槽传感器（照片灭弧室），试图抓住一个硬币下降。在所有这些情况下，使用一个中断可以释放的微控制器来完成其他一些工作。

**关于中断服务例程**
ISRs 是特殊类型的函数，具有一些独特的限制，但大多数其它函数并没有这样的限制。
ISR 不能有任何参数，并且他们不返回任何内容。

一般来说，ISR 应该尽可能的短而快。
假如你的 sketch 使用多个 ISRs，每次仅有一个能运行，其余的中断依照秩序，在当前这个被执行完后接着执行。

millis() 依赖于中断计数，因此在 ISR 内，它将永远不会增加。
由于 delay() 需要中断工作，如果在 ISR 内调用delay，它将不会工作。
micros() 期初可以工作一小会儿，在开始 1-2ms 后会行为不正常。
delayMicroseconds() 不使用任何计数器，所以它将能正常工作。

通常在 ISR 和 主程序之间使用全局变量传递数据。
确保 ISR 和主程序之间共享的变量正确更新，将变量声明为volatile。

关于中断的详细信息，请参阅 Nick Gammon's 的笔记。
http://gammon.com.au/interrupts

**Syntax**
attachInterrupt(digitalPinToInterrupt(pin), ISR, mode);	(recommended推荐)

attachInterrupt(interrupt, ISR, mode);	(not recommended不推荐)

attachInterrupt(pin, ISR, mode) ;	(not recommended Arduino Due, Zero, MKR1000, 101 only)


**Parameters**
interrupt:	the number of the interrupt (int) 中断引脚号 

pin:	the pin number	(Arduino Due, Zero, MKR1000 only)

ISR:	the ISR to call when the interrupt occurs; this function must take no parameters and return nothing. This function is sometimes referred to as an interrupt service routine.
中断发生时调用 ISR ;
该函数必须不带参数和不返回任何值。
该函数有时被称为中断服务例程 ISR。 

mode:	
defines when the interrupt should be triggered. Four constants are predefined as valid values:
LOW to trigger the interrupt whenever the pin is low,
CHANGE to trigger the interrupt whenever the pin changes value
RISING to trigger when the pin goes from low to high,
FALLING for when the pin goes from high to low.

定义何时触发中断，以下四个contstants预定有效值：
LOW 当引脚为低电平时，触发中断
CHANGE 当引脚电平发生改变时，触发中断
RISING 当引脚由低电平变为高电平时，触发中断
FALLING 当引脚由高电平变为低电平时，触发中断.

The Due board allows also:
HIGH to trigger the interrupt whenever the pin is high.
(Arduino Due, Zero, MKR1000 only)
HIGH 无论何时为高电平时，触发中断。
(Arduino Due, Zero, MKR1000 only)

**Returns**
none

**Example**
```
const byte ledPin = 13;
const byte interruptPin = 2;
volatile byte state = LOW;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(interruptPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(interruptPin), blink, CHANGE);
}

void loop() {
  digitalWrite(ledPin, state);
}

void blink() {
  state = !state;
}
```


Description
Digital Pins With Interrupts
The first parameter to attachInterrupt is an interrupt number. Normally you should use digitalPinToInterrupt(pin) to translate the actual digital pin to the specific interrupt number. For example, if you connect to pin 3, use digitalPinToInterrupt(3) as the first parameter to attachInterrupt.

Note
Inside the attached function, delay() won't work and the value returned by millis() will not increment. Serial data received while in the function may be lost. You should declare as volatile any variables that you modify within the attached function. See the section on ISRs below for more information.

Using Interrupts
Interrupts are useful for making things happen automatically in microcontroller programs, and can help solve timing problems. Good tasks for using an interrupt may include reading a rotary encoder, or monitoring user input.

If you wanted to insure that a program always caught the pulses from a rotary encoder, so that it never misses a pulse, it would make it very tricky to write a program to do anything else, because the program would need to constantly poll the sensor lines for the encoder, in order to catch pulses when they occurred. Other sensors have a similar interface dynamic too, such as trying to read a sound sensor that is trying to catch a click, or an infrared slot sensor (photo-interrupter) trying to catch a coin drop. In all of these situations, using an interrupt can free the microcontroller to get some other work done while not missing the input.

About Interrupt Service Routines
ISRs are special kinds of functions that have some unique limitations most other functions do not have. An ISR cannot have any parameters, and they shouldn't return anything.

Generally, an ISR should be as short and fast as possible. If your sketch uses multiple ISRs, only one can run at a time, other interrupts will be executed after the current one finishes in an order that depends on the priority they have. millis() relies on interrupts to count, so it will never increment inside an ISR. Since delay() requires interrupts to work, it will not work if called inside an ISR. micros() works initially, but will start behaving erratically after 1-2 ms. delayMicroseconds() does not use any counter, so it will work as normal.

Typically global variables are used to pass data between an ISR and the main program. To make sure variables shared between an ISR and the main program are updated correctly, declare them as volatile.

For more information on interrupts, see Nick Gammon's notes.

**中断号**
通常你应该使用
Interrupt numbers digitalPinToInterrupt(pin)，而不是在 sketch 中直接使用中断号，
特定的中断引脚，以及他们所映射的中断号在不同类型的板卡上会变化。
直接使用中断号可能看似简单，但是当你在不同的板子上运行你的 sketch 时，可能导致兼容性故障。

不过，以前的 sketch 常常直接使用中断号。
常使用数字 0 ( 数字引脚 2 )，或数字 1 (数字引脚 3)。
下表显示了在不同的板子上可用的中断引脚。

不过像这些新的，具有高性能处理器的主板：
Due;
Zero;
MKR1000;
101;
在引脚和中断号之间存在一对一的映射，语法如下
attachInterrupt(digitalPinToInterrupt(interruptPin), blink, CHANGE);
attachInterrupt(interruptPin, blink, CHANGE);
是相等的。

| Board                            | int.0 | int.1 | int.2 | int.3  | int.4 | int.5 |
| :------------------------------- | :---: | :---: | :---: | :----: | :---: | :---: |
| Uno, Ethernet                    |   2   |   3   |       |        |       |       |
| Mega2560                         |   2   |   3   |  21   |   20   |  19   |  18   |
| 32u4 based (e.g Leonardo, Micro) |   3   |   2   |   0   | 1	7 |       |       |
Due, Zero, MKR1000, 101 interrupt number = pin number

---
Normally you should use digitalPinToInterrupt(pin), rather than place an interrupt number directly into your sketch. The specific pins with interrupts, and their mapping to interrupt number varies on each type of board. Direct use of interrupt numbers may seem simple, but it can cause compatibility trouble when your sketch is run on a different board.

However, older sketches often have direct interrupt numbers. Often number 0 (for digital pin 2) or number 1 (for digital pin 3) were used. The table below shows the available interrupt pins on various boards.

However on newer boards with high performances processors like:

Due;
Zero;
MKR1000;
101;
a one-to-one mapping between the pin and the interrupt number exist, so the syntaxes:

attachInterrupt(digitalPinToInterrupt(interruptPin), blink, CHANGE);
attachInterrupt(interruptPin, blink, CHANGE);
are equivalent.

Board	int.0	int.1	int.2	int.3	int.4	int.5
Uno, Ethernet	2	3	 	 	 	 
Mega2560	2	3	21	20	19	18
32u4 based (e.g Leonardo, Micro)	3	2	0	1	7	 
Due, Zero, MKR1000, 101	interrupt number = pin number

---

## 2. detachInterrupt()
**Description**
Turns off the given interrupt.
关闭给定的中断。

**Syntax**
detachInterrupt(interrupt)
detachInterrupt(digitalPinToInterrupt(pin));
detachInterrupt(pin)	(Arduino Due, Zero only)

Parameters
interrupt: the number of the interrupt to disable (see attachInterrupt() for more details).
pin: the pin number of the interrupt to disable (Arduino Due only)