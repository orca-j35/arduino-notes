# Analog IO
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. analogReference()
**Description**
配置参考电压，用于模拟输入(即：输入范围的最大值)
选项如下：
- DEFAULT: 使用默认参考电压 5 volts (on 5V Arduino boards) or 3.3 volts (on 3.3V Arduino boards)
- INTERNAL: 一个内建参考电压，equal to 1.1 volts on the ATmega168 or ATmega328 and 2.56 volts on the ATmega8 (在 Arduino Mega 上该项不可用)
- INTERNAL1V1: 内建 1.1V 参考 (Arduino Mega only)
- INTERNAL2V56: 内建 2.56V 参考 (Arduino Mega only)
- EXTERNAL: 以AREF引脚（0至5V）的电压作为基准电压。

**Syntax**
analogReference(type)
**Parameters**
type: which type of reference to use (DEFAULT, INTERNAL, INTERNAL1V1, INTERNAL2V56, or EXTERNAL).
**Returns**
None.
**Note**
After changing the analog reference, the first few readings from analogRead() may not be accurate.
在改变模拟参考电压后，前几个 analogRead() 的读数可能不准确。

**Warning**
不要在AREF引脚上使用使用任何小于0V或超过5V的外部电压。如果你使用 AREF 引脚上的电压作为基准电压，你在调用 analogRead() 前必须设置参考类型为EXTERNAL。否则，你将会使内部产生的有效基准电压和 AREF 引脚间形成短路，这可能会损坏您Arduino板上的单片机。

另外，您可以在外部基准电压和AREF引脚之间连接一个5K电阻，使你可以在外部和内部基准电压之间切换。请注意，总阻值将会发生改变，因为AREF引脚内部有一个32K电阻。这两个电阻都有分压作用。所以，例如，如果输入2.5V的电压，最终在在AREF引脚上的电压将为2.5 * 32 /（32 + 5）= 2.2V。

Configures the reference voltage used for analog input (i.e. the value used as the top of the input range). The options are:

- DEFAULT: the default analog reference of 5 volts (on 5V Arduino boards) or 3.3 volts (on 3.3V Arduino boards)
- INTERNAL: an built-in reference, equal to 1.1 volts on the ATmega168 or ATmega328 and 2.56 volts on the ATmega8 (not available on the Arduino Mega)
- INTERNAL1V1: a built-in 1.1V reference (Arduino Mega only)
- INTERNAL2V56: a built-in 2.56V reference (Arduino Mega only)
- EXTERNAL: the voltage applied to the AREF pin (0 to 5V only) is used as the reference.

Warning
Don't use anything less than 0V or more than 5V for external reference voltage on the AREF pin! If you're using an external reference on the AREF pin, you must set the analog reference to EXTERNAL before calling analogRead(). Otherwise, you will short together the active reference voltage (internally generated) and the AREF pin, possibly damaging the microcontroller on your Arduino board.

Alternatively, you can connect the external reference voltage to the AREF pin through a 5K resistor, allowing you to switch between external and internal reference voltages. Note that the resistor will alter the voltage that gets used as the reference because there is an internal 32K resistor on the AREF pin. The two act as a voltage divider, so, for example, 2.5V applied through the resistor will yield 2.5 * 32 / (32 + 5) = ~2.2V at the AREF pin.

**See also**
Description of the analog input pins
https://www.arduino.cc/en/Tutorial/AnalogInputPins
analogRead()

## 2. analogRead()
**Description**
从指定的 analog 引脚读取数值。
arduino 包含一个6通道(在 Mini 和 Nano 中是8通道，在mega 中是16通道)，10-bit 的模数转换器。
这意味着它将0至5伏特之间的输入电压映射到0至1023之间的int 数值。
这将产生读数之间的分辨率：5伏特/ 1024单位，或0.0049伏特（4.9 mV）每单位。
输入范围和分辨率可以使用 analogReference() 改变。 

它需要大约100微秒（0.0001）来读取模拟输入，所以最大的阅读速度是每秒10000次。

**Syntax**
analogRead(pin)

**Parameters**
pin: the number of the analog input pin to read from (0 to 5 on most boards, 0 to 7 on the Mini and Nano, 0 to 15 on the Mega)

**Returns**
int (0 to 1023)

**注意**
如果模拟引脚没有进行任何连接，
如果模拟输入引脚没有连入电路，由analogRead()返回的值将根据多项因素产生波动。（例如其他模拟输入引脚，你的手靠近板子等因素）

**Example**
```
int analogPin = 3;     // potentiometer wiper (middle terminal) connected to analog pin 3
//电位器（中间的引脚）连接到模拟输入引脚3
                       // outside leads to ground and +5V
                        //另外两个引脚分别接地和+5 V

int val = 0;           // variable to store the value read //定义变量来存储读取的数值

void setup()
{

  Serial.begin(9600);          //  setup serial

}

void loop()

{

  val = analogRead(analogPin);    // read the input pin //从输入引脚读取数值

  Serial.println(val);             // debug value
//显示读取的数值
}
```
Reads the value from the specified analog pin. The Arduino board contains a 6 channel (8 channels on the Mini and Nano, 16 on the Mega), 10-bit analog to digital converter. This means that it will map input voltages between 0 and 5 volts into integer values between 0 and 1023. This yields a resolution between readings of: 5 volts / 1024 units or, .0049 volts (4.9 mV) per unit. The input range and resolution can be changed using analogReference().

It takes about 100 microseconds (0.0001 s) to read an analog input, so the maximum reading rate is about 10,000 times a second.

If the analog input pin is not connected to anything, the value returned by analogRead() will fluctuate based on a number of factors (e.g. the values of the other analog inputs, how close your hand is to the board, etc.).

Example



## 3. analogWrite()
**描述**

向某个引脚写入模拟值(PWM 波形)。
https://www.arduino.cc/en/Tutorial/PWM
https://app.yinxiang.com/shard/s10/nl/119459/ba031d35-08e6-44f1-81ac-44c2097aa990
可以使用不同的亮度来点亮 LED，或以不同的速度驱动电机。
引脚在调用 analogWrite() 之后，该引脚将产生一个特定占空比的稳定方波，直到再次调用 analogWrite() 以改变该引脚的状态（也可在相同的引脚上调用 digitalRead() 或 digitalWrite() ）。
在大多数引脚中，PWM信号的频率大约是 490 Hz。
在 Uno 和类似的板卡上，引脚 5 和 6 的频率大约是 980 Hz。
在 Leonardo 上引脚 3 和 11 的频率也是 980 Hz。

在大多是 Arduino板卡上(those with the ATmega168 or ATmega328), 该函数可以用于 3, 5, 6, 9, 10,  11 引脚。
在Arduino Mega，引脚 2 - 13 和 44 - 46 可实现该功能。
使用  ATmega8 的老版本的 Arduino ，仅有9, 10, and 11支持analogWrite() 函数。

 Arduino Due 在  2 到 13 引脚提供该功能，加上引脚 DAC0 和 DAC1。与 PWM 引脚不同的是  DAC0 和 DAC1 是数字-模拟转换，会输出真正的模拟量。

在调用 analogWrite()之前，你不需要调用 pinMode()设置引脚作为输出。

analogWrite 函数和模拟引脚或是 analogRead 函数没有关系。


**语法 Syntax**
analogWrite(pin, value)

**参数 Parameters**
pin: the pin to write to.
value: the duty cycle: between 0 (always off) and 255 (always on).

**返回值 Returns**
nothing

**说明和已知问题**
引脚 5 和6 所产生的 PWM 输出的占空比会高于预期。
这是因为 millis() 和 delay() 函数的相互影响，这两个函数和 PWM 输出共享相同的内部计时器。
在大多数低占空比设置中需要注意这点，并可能导致在数值为0时，没有完全关闭引脚5和6。
（这将导致大多时候处于低占空比状态（如：0 - 10），并可能导致在数值为0时，没有完全关闭引脚5和6。）

**示例 Example**
Sets the output to the LED proportional to the value read from the potentiometer.
设置到 LED 的输出值与从电位器读取的值成正比。
```
int ledPin = 9;      // LED connected to digital pin 9

int analogPin = 3;   // potentiometer connected to analog pin 3

int val = 0;         // variable to store the read value



void setup()

{

  pinMode(ledPin, OUTPUT);   // sets the pin as output

}
void loop()
{
  val = analogRead(analogPin);   // read the input pin

  analogWrite(ledPin, val / 4);  // analogRead values go from 0 to 1023, analogWrite values from 0 to 255
}
```

**See also**
analogRead()
analogWriteResolution()
Tutorial: PWM

 Description

Writes an analog value (PWM wave) to a pin. Can be used to light a LED at varying brightnesses or drive a motor at various speeds. After a call to analogWrite(), the pin will generate a steady square wave of the specified duty cycle until the next call to analogWrite() (or a call to digitalRead() or digitalWrite() on the same pin). The frequency of the PWM signal on most pins is approximately 490 Hz. On the Uno and similar boards, pins 5 and 6 have a frequency of approximately 980 Hz. Pins 3 and 11 on the Leonardo also run at 980 Hz.

On most Arduino boards (those with the ATmega168 or ATmega328), this function works on pins 3, 5, 6, 9, 10, and 11. On the Arduino Mega, it works on pins 2 - 13 and 44 - 46. Older Arduino boards with an ATmega8 only support analogWrite() on pins 9, 10, and 11.

The Arduino Due supports analogWrite() on pins 2 through 13, plus pins DAC0 and DAC1. Unlike the PWM pins, DAC0 and DAC1 are Digital to Analog converters, and act as true analog outputs.

You do not need to call pinMode() to set the pin as an output before calling analogWrite().

The analogWrite function has nothing to do with the analog pins or the analogRead function.

Notes and Known Issues
The PWM outputs generated on pins 5 and 6 will have higher-than-expected duty cycles. This is because of interactions with the millis() and delay() functions, which share the same internal timer used to generate those PWM outputs. This will be noticed mostly on low duty-cycle settings (e.g 0 - 10) and may result in a value of 0 not fully turning off the output on pins 5 and 6.