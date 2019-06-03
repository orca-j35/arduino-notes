# Time 时间
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. millis() 返回毫秒计数
**描述：**
返回Arduino开发板从运行当前程序开始的毫秒数。这个数字将在约50天后溢出（归零）。

**Parameters**
None
**Returns**
Number of milliseconds since the program started (unsigned long)

**注意：**
注意，参数 millis 是一个无符号长整数，试图和较小的数据类型（如整型数）做数学运算可能会产生逻辑错误。
甚至 signed long 也会遭遇错误，因为其最大值是 无符号 long 的一半。
当中断函数发生时，millis()的数值将不会继续变化。

**Example**
```
unsigned long time;

void setup(){
  Serial.begin(9600);
}
void loop(){
  Serial.print("Time: ");
  time = millis();
  //prints time since program started
  Serial.println(time);
  // wait a second so as not to send massive amounts of data
  delay(1000);
}
```

Description
Returns the number of milliseconds since the Arduino board began running the current program. This number will overflow (go back to zero), after approximately 50 days.

Note:
Please note that the return value for millis() is an unsigned long, logic errors may occur if a programmer tries to do arithmetic with smaller data types such as int's. Even signed long may encounter errors as its maximum value is half that of its unsigned counterpart.

Example


## 2. micros() 返回微秒计数
**描述：**
返回 Arduino 开发板从运行当前程序开始的微秒数。
这个数字将在约70分钟后溢出（归零）。在 16MHz 的 Arduino 开发板上（比如 Duemilanove 和 Nano），这个函数的分辨率为四微秒（即返回值总是四的倍数）。在 8MHz 的 Arduino 开发板上（比如 LilyPad），这个函数的分辨率为八微秒。

注意 ：每毫秒是1,000微秒，每秒是1,000,000微秒。

**Parameters**
None

**Returns**
Number of microseconds since the program started (unsigned long)

**Example**

```
unsigned long time;

void setup(){
  Serial.begin(9600);
}
void loop(){
  Serial.print("Time: ");
  time = micros();
  //prints time since program started
  Serial.println(time);
  // wait a second so as not to send massive amounts of data
  delay(1000);
}
```

Description
Returns the number of microseconds since the Arduino board began running the current program. This number will overflow (go back to zero), after approximately 70 minutes. On 16 MHz Arduino boards (e.g. Duemilanove and Nano), this function has a resolution of four microseconds (i.e. the value returned is always a multiple of four). On 8 MHz Arduino boards (e.g. the LilyPad), this function has a resolution of eight microseconds.

Note: there are 1,000 microseconds in a millisecond and 1,000,000 microseconds in a second.


## 3. delay() 延迟微秒
**描述：**
Pauses the program for the amount of time (in miliseconds) specified as parameter. (There are 1000 milliseconds in a second.)

使程序暂定设定的时间参数（单位毫秒）。
（一秒等于1000毫秒）

**Syntax**
delay(ms)

**Parameters**
ms: the number of milliseconds to pause (unsigned long)

**Returns**
nothing

**Example**

```
int ledPin = 13;                 // LED connected to digital pin 13

void setup()
{
  pinMode(ledPin, OUTPUT);      // sets the digital pin as output
}

void loop()
{
  digitalWrite(ledPin, HIGH);   // sets the LED on
  delay(1000);                  // waits for a second
  digitalWrite(ledPin, LOW);    // sets the LED off
  delay(1000);                  // waits for a second
}
```
**警告：**
虽然创建一个使用 delay() 的闪烁LED很简单，并且许多例子将很短的delay用于消除开关抖动，delay()确实拥有很多显著的缺点。在delay函数使用的过程中，读取传感器值、计算、引脚操作均无法执行，因此，它所带来的后果就是使其他大多数活动暂停。其他操作定时的方法请参加millis()函数和它下面的例子。大多数熟练的程序员通常避免超过10毫秒的delay(),除非arduino程序非常简单。

但某些操作在delay() 函数控制 Atmega 芯片时任然能够运行，因为delay函数不会使中断失效。通信端口 RX 接收到得数据会被记录，PWM(analogWrite)值和引脚状态会保持，中断也会按设定的执行。

Caveat
While it is easy to create a blinking LED with the delay() function, and many sketches use short delays for such tasks as switch debouncing, the use of delay() in a sketch has significant drawbacks. No other reading of sensors, mathematical calculations, or pin manipulation can go on during the delay function, so in effect, it brings most other activity to a halt. For alternative approaches to controlling timing see the millis() function and the sketch sited below. More knowledgeable programmers usually avoid the use of delay() for timing of events longer than 10's of milliseconds unless the Arduino sketch is very simple.

Certain things do go on while the delay() function is controlling the Atmega chip however, because the delay function does not disable interrupts. Serial communication that appears at the RX pin is recorded, PWM (analogWrite) values and pin states are maintained, and interrupts will work as they should.


## 4. delayMicroseconds() 延迟微秒
**描述：**
使程序暂停指定的一段时间（单位：微秒）。
一秒等于1000000微秒，一毫秒等于 1000 微秒。
目前，能够产生的最大的延时准确值是16383。这可能会在未来的Arduino版本中改变。对于超过几千微秒的延迟，你应该使用delay()代替。

**Syntax**
delayMicroseconds(us)

**Parameters**
us: the number of microseconds to pause (unsigned int)
暂停的时间，单位微秒（unsigned int）
**Returns**
None

**Example**

```
int outPin = 8;                 // digital pin 8

void setup()
{
  pinMode(outPin, OUTPUT);      // sets the digital pin as output
}

void loop()
{
  digitalWrite(outPin, HIGH);   // sets the pin on
  delayMicroseconds(50);        // pauses for 50 microseconds      
  digitalWrite(outPin, LOW);    // sets the pin off
  delayMicroseconds(50);        // pauses for 50 microseconds      
}
```
将8号引脚配置为输出脚。它会发出一系列周期近似100微秒的方波。
因为同时还会在代码中执行其它的指令，所以是近似值。

**警告和已知问题**

此函数在3微秒以以上工作的非常准确。我们不能保证，delayMicroseconds在更小的时间内延时准确。

Arduino0018版本后，delayMicroseconds()不再会使中断失效。

Description
Pauses the program for the amount of time (in microseconds) specified as parameter. There are a thousand microseconds in a millisecond, and a million microseconds in a second.

Currently, the largest value that will produce an accurate delay is 16383. This could change in future Arduino releases. For delays longer than a few thousand microseconds, you should use delay() instead.

configures pin number 8 to work as an output pin. It sends a train of pulses of approximately 100 microseconds period. The approximation is due to execution of the other instructions in the code.

Caveats and Known Issues

This function works very accurately in the range 3 microseconds and up. We cannot assure that delayMicroseconds will perform precisely for smaller delay-times.

As of Arduino 0018, delayMicroseconds() no longer disables interrupts.