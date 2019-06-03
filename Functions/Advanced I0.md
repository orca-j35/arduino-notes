# Advanced I0
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

##  1. tone()
**Description**
在一个引脚上产生一个特定频率的方波(50% 占空比)。持续时间可以被设定，否则波形会一致产生直到调用 noTone() 函数。该引脚可以连接压电蜂鸣器或其他喇叭播放声音。

在同一时刻只能产生一个声音。如果一个引脚已经在播放音乐，那调用tone()将不会有任何效果。如果音乐在同一个引脚上播放，它会自动调整频率。

使用tone()函数会与3脚和11脚的PWM产生干扰（Mega板除外）。

| Broad                                    |     Min frequency (Hz) | Max frequency (Hz) |
| :--------------------------------------- | ---------------------: | :----------------- |
| Uno, Mega, Leonardo and other AVR boards |                     31 | 65535              |
| Gemma                                    | Not implemented 不执行 | Not implemented    |
| Zero                                     |                     41 | 275000             |
| Due                                      |        Not implemented | Not implemented    |
For technical details, see Brett Hagman's notes.
技术细节，请看  Brett Hagman's 的笔记
https://code.google.com/archive/p/rogue-code/wikis/ToneLibraryDocumentation.wiki#Ugly_Details

注意：如果你要在多个引脚上产生不同的音调，你要在对下一个引脚使用tone()函数前对此引脚调用noTone()函数。

**Syntax**
tone(pin, frequency) 
tone(pin, frequency, duration)

**Parameters**
pin: the pin on which to generate the tone
frequency: the frequency of the tone in hertz - unsigned int
duration: the duration of the tone in milliseconds (optional) - unsigned long  
tone 以毫秒为单位的持续时间（可选）unsigned long

Returns
nothing

See also
noTone()
analogWrite()
Tutorial:Tone
Tutorial:Pitch follower
Tutorial:Simple Keyboard
Tutorial: multiple tones
Tutorial: PWM

>Generates a square wave of the specified frequency (and 50% duty cycle) on a pin. A duration can be specified, otherwise the wave continues until a call to noTone(). The pin can be connected to a piezo buzzer or other speaker to play tones.

>Only one tone can be generated at a time. If a tone is already playing on a different pin, the call to tone() will have no effect. If the tone is playing on the same pin, the call will set its frequency.

>Use of the tone() function will interfere with PWM output on pins 3 and 11 (on boards other than the Mega).

>Board	Min frequency (Hz)	Max frequency (Hz)
>Uno, Mega, Leonardo and other AVR boards	31	65535
>Gemma	Not implemented	Not implemented
>Zero	41	275000
>Due	Not implemented	Not implemented
>For technical details, see Brett Hagman's notes.

>NOTE: if you want to play different pitches on multiple pins, you need to call noTone() on one pin before calling tone() on the next pin.

## 2. noTone()
**Description**
Stops the generation of a square wave triggered by tone(). Has no effect if no tone is being generated.
停止由 tone() 产生的方波。
如果没有使用 tone() 将不会有效果。

NOTE: if you want to play different pitches on multiple pins, you need to call noTone() on one pin before calling tone() on the next pin.
注意：如果你想在多个引脚上产生不同的声音，你要在对下个引脚使用 tone() 前对刚才的引脚调用noTone().

**Syntax**
noTone(pin)

**Parameters**
pin: the pin on which to stop generating the tone

**Returns**
nothing

## 3. shiftOut()
**描述：**
将一个数据的一个字节一位一位的移除。
从最高有效位（最左边）或最低有效位（最右边）开始。
依次向数据脚写入每一位，之后时钟脚被拉高或拉低，指示刚才的数据有效。

注意：如果你所连接的设备时钟类型为上升沿，你需要确定在调用 shiftOut() 前时钟脚为低电平，如调用digitalWrite(clockPin, LOW)。

这是一个软件实现；Arduino提供了一个硬件实现的 SPI 库，它速度更快但只在特定脚有效。
https://www.arduino.cc/en/Reference/SPI

**Syntax**
shiftOut(dataPin, clockPin, bitOrder, value)

**Parameters**
dataPin: the pin on which to output each bit (int)
输出每一位数据的引脚(int) 
clockPin: the pin to toggle once the dataPin has been set to the correct value (int)
dataPin 被设置为正确的值时，该引脚切换一次。(int) 
bitOrder: which order to shift out the bits; either MSBFIRST or LSBFIRST.(Most Significant Bit First, or, Least Significant Bit First)
输出位比特位的顺序； MSBFIRST 最高位优先或最低位优先。
value: the data to shift out. (byte)
需要位移输出的数据 byte
**Returns**
None

**注意**
dataPin和clockPin要用pinMode()配置为输出。 
shiftOut目前只能输出1个字节（8位），所以如果输出值大于255需要分两步。
```
// Do this for MSBFIRST serial
int data = 500;
// shift out highbyte
shiftOut(dataPin, clock, MSBFIRST, (data >> 8));  
// shift out lowbyte
shiftOut(dataPin, clock, MSBFIRST, data);

// Or do this for LSBFIRST serial
data = 500;
// shift out lowbyte
shiftOut(dataPin, clock, LSBFIRST, data);  
// shift out highbyte 
shiftOut(dataPin, clock, LSBFIRST, (data >> 8)); 
```

**Example**
For accompanying circuit, see the tutorial on controlling a 74HC595 shift register.
https://www.arduino.cc/en/Tutorial/ShiftOut
相应电路，查看tutorial on controlling a 74HC595 shift register

```
//**************************************************************//
//  Name    : shiftOutCode, Hello World                         //
//  Author  : Carlyn Maw,Tom Igoe                               //
//  Date    : 25 Oct, 2006                                      //
//  Version : 1.0                                               //
//  Notes   : Code for using a 74HC595 Shift Register           //
//          : to count from 0 to 255                            //
//****************************************************************

//Pin connected to ST_CP of 74HC595
int latchPin = 8;
//Pin connected to SH_CP of 74HC595
int clockPin = 12;
////Pin connected to DS of 74HC595
int dataPin = 11;

void setup() {
  //set pins to output because they are addressed in the main loop
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(dataPin, OUTPUT);
}

void loop() {
  //count up routine
  for (int j = 0; j < 256; j++) {
    //ground latchPin and hold low for as long as you are transmitting
    digitalWrite(latchPin, LOW);
    shiftOut(dataPin, clockPin, LSBFIRST, j);   
    //return the latch pin high to signal chip that it 
    //no longer needs to listen for information
    digitalWrite(latchPin, HIGH);
    delay(1000);
  }
} 
```

Description
Shifts out a byte of data one bit at a time. Starts from either the most (i.e. the leftmost) or least (rightmost) significant bit. Each bit is written in turn to a data pin, after which a clock pin is pulsed (taken high, then low) to indicate that the bit is available.

Note: if you're interfacing with a device that's clocked by rising edges, you'll need to make sure that the clock pin is low before the call to shiftOut(), e.g. with a call to digitalWrite(clockPin, LOW).

This is a software implementation; see also the SPI library, which provides a hardware implementation that is faster but works only on specific pins.

Note
The dataPin and clockPin must already be configured as outputs by a call to pinMode().
shiftOut is currently written to output 1 byte (8 bits) so it requires a two step operation to output values larger than 255.

## 4. shiftIn()
**描述：**
将一个数据的一个字节一位一位的移入。
从最高有效位（最左边）或最低有效位（最右边）开始。
对于每个位，先拉高时钟clock 电平，再从数据传输线中读取一位，再将时钟线拉低。

如果你所连接的设备时钟类型为上升沿，你需要确定在调用 shiftIn() 前时钟脚为低电平，如调用digitalWrite(clockPin, LOW)。

注意：这是一个软件实现；Arduino提供了一个硬件实现的SPI库，它速度更快但只在特定脚有效。
https://www.arduino.cc/en/Reference/SPI

**Syntax**
byte incoming = shiftIn(dataPin, clockPin, bitOrder)

**Parameters**
dataPin: the pin on which to input each bit (int)
clockPin: the pin to toggle to signal a read from dataPin
从 dataPin 读取一位是，该引脚切换信号。
bitOrder: which order to shift in the bits; either MSBFIRST or LSBFIRST.
(Most Significant Bit First, or, Least Significant Bit First)
输出位的顺序，最高位优先或最低位优先

**Returns**
the value read (byte)


Description
Shifts in a byte of data one bit at a time. Starts from either the most (i.e. the leftmost) or least (rightmost) significant bit. For each bit, the clock pin is pulled high, the next bit is read from the data line, and then the clock pin is taken low.

If you're interfacing with a device that's clocked by rising edges, you'll need to make sure that the clock pin is low before the first call to shiftIn(), e.g. with a call to digitalWrite(clockPin, LOW).

Note: this is a software implementation; Arduino also provides an SPI library that uses the hardware implementation, which is faster but only works on specific pins.

## 5. pulseIn() 读取输入脉冲宽度
**描述**
读取一个引脚上的脉冲宽度(HIGH 或 LOW)。
例如：假如值是 HIGH，pulseIn() 等待引脚变为高电平，开始计时，然后等待引脚变为 LOW 并停止计时。
返回 pulse 的长度，单位是微秒。
假如在超时范围内，没有完整 pulse 被接受，将返回 0 。

该函数的计时功能由经验决定，在短脉冲下可能会显示错误。
计时范围从10微秒至3分钟。（1秒=1000毫秒=1000000微秒）

同样请注意，当函数被调用时该引脚是否已经是 high，在函数开始继续之前，它将等待引脚变为 LOW，再变HIGH。
该例程仅在中断被激活时可以被使用。
此外，最高的分辨率在最短的间隔下获得。

**Syntax**
pulseIn(pin, value) 
pulseIn(pin, value, timeout)

**Parameters**
pin: the number of the pin on which you want to read the pulse. (int)
value: type of pulse to read: either HIGH or LOW. (int)
timeout (optional): the number of microseconds to wait for the pulse to be completed: the function returns 0 if no complete pulse was received within the timeout. Default is one second (unsigned long).
指定等待脉冲计数的完成时间，单位为微秒，默认值是1秒（unsigned long）：假如在超时范围内，没有完整 pulse 被接受，将返回 0 。默认是 1 秒 (unsigned long)。

**Returns**
the length of the pulse (in microseconds) or 0 if no pulse is completed before the timeout (unsigned long)

脉冲长度(微秒)；如果在超时之前没有完整脉冲，将返回 0 (unsigned long)

**Example**
```
int pin = 7;
unsigned long duration;

void setup()
{
  pinMode(pin, INPUT);
}

void loop()
{
  duration = pulseIn(pin, HIGH);
}
```


Description
Reads a pulse (either HIGH or LOW) on a pin. For example, if value is HIGH, pulseIn() waits for the pin to go HIGH, starts timing, then waits for the pin to go LOW and stops timing. Returns the length of the pulse in microseconds or 0 if no complete pulse was received within the timeout.

The timing of this function has been determined empirically and will probably show errors in shorter pulses. Works on pulses from 10 microseconds to 3 minutes in length. Please also note that if the pin is already high when the function is called, it will wait for the pin to go LOW and then HIGH before it starts counting. This routine can be used only if interrupts are activated. Furthermore the highest resolution is obtained with short intervals.