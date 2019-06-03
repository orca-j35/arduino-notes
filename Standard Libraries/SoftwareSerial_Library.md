# SoftwareSerial Library

[TOC]

[SoftwareSerial Library URL](https://www.arduino.cc/en/Reference/SoftwareSerial)

可将任何数字引脚用于串口通讯


>The Arduino hardware has built-in support for serial communication on pins 0 and 1 (which also goes to the computer via the USB connection). 

Arduino 的硬件在引脚 0 和 1 内建支持串口通讯 ( 该引脚也通过 USB 连接到电脑 )。
>The native serial support happens via a piece of hardware (built into the chip) called a UART. 

原生串行通讯通过一块叫做 UART 的硬件支持产生，该硬件在芯片内部。
>This hardware allows the Atmega chip to receive serial communication even while working on other tasks, as long as there room in the 64 byte serial buffer.

该硬件允许 Atmega 芯片接受串行通讯，甚至同时处理其他任务，只要在 64 自己的串行缓冲器空间内。

>The SoftwareSerial library has been developed发展 to allow serial communication on other digital pins of the Arduino, using software to replicate复制 the functionality (hence因此 the name命名 "SoftwareSerial"). 

>It is possible可能 to have multiple software serial ports with speeds up达 to 115200 bps. 

>A parameter参数 enables inverted signaling for devices which require that protocol.

一个参数对需要该协议的设备开启反向信号。
>The version of SoftwareSerial included in 1.0 and later is based on the NewSoftSerial library by Mikal Hart.

## Limitations 限制
>The library has the following known limitations:

这个库有下列已知的局限性:
>- If using multiple software serial ports, only one can receive data at a time.

假如使用多个软件串行端口，在同一时间内仅有一个能够接受数据
>- Not all pins on the Mega and Mega 2560 support支持 change interrupts, so only the following can be used for RX: 10, 11, 12, 13, 14, 15, 50, 51, 52, 53, A8 (62), A9 (63), A10 (64), A11 (65), A12 (66), A13 (67), A14 (68), A15 (69).

不是所有 Mega 和Mega 2560 的引脚都支持改变中断，所以仅有以下可用于 RX ：10, 11, 12, 13, 14, 15, 50, 51, 52, 53, A8 (62), A9 (63), A10 (64), A11 (65), A12 (66), A13 (67), A14 (68), A15 (69).
>- Not all pins on the Leonardo and Micro support change interrupts, so only the following can be used for RX: 8, 9, 10, 11, 14 (MISO), 15 (SCK), 16 (MOSI).

不是所有 Leonardo 和 Micro的引脚都支持改变中断，所有仅有以下可用于 RX ：8, 9, 10, 11, 14 (MISO), 15 (SCK), 16 (MOSI).
>- On Arduino or Genuino 101 the current maximum RX speed is 57600bps .

在 Arduino 或 Genuino 101 上，目前最大 RX速率是  57600bps。
>- On Arduino or Genuino 101 RX doesn't work on Pin 13 .

在  Arduino 或 Genuino 101 RX 不能工作在 13 引脚。
>If your project requires simultaneous data flows, see Paul Stoffregen's AltSoftSerial library. 

假如你的项目需要同步数据流，参阅 Paul Stoffregen's AltSoftSerial library。
>AltSoftSerial overcomes客服 a number of other issues问题 with the core SoftwareSerial, but has it's own limitations. 

AltSoftSerial 客服了  SoftwareSerial 上的一些其余核心问题，但是还是有它自己的局限性。
>Refer to the AltSoftSerial site for more information.

## Examples 例子
>Software Serial Example: Use this Library... because sometimes one serial port just isn't enough!

>Two Port Receive接受: Work with multiple software serial ports.


## Functions

### SoftwareSerial(rxPin, txPin, inverse_logic)
- Description

>SoftwareSerial is used to create an instance of a SoftwareSerial object, whose name you need to provide as in the example below. 

SoftwareSerial 被用作创建 SoftwareSerial 对象的实例，你需要提供的名字如下列所示。
>The inverse_logic argument is optional and defaults to false. 

 inverse_logic 反向逻辑参数是可选的，默认值是 false。
>See below下面 for more details细节 about what it does. 

参阅下方的更多细节，有关它能做什么。
>Multiple SoftwareSerial objects may be created, however only one can be active at a given moment.

可以创建多个 SoftwareSerial 对象，但是在给定的时间内仅仅有一个是被激活的。
>You need to call SoftwareSerial.begin() to enable communication.

你需要调用 SoftwareSerial.begin() 以开启通信。

- Parameters

>rxPin: the pin on which to receive接受 serial data

>txPin: the pin on which to transmit发送 serial data

>inverse_logic: is used to invert翻转 the sense意义 of incoming bits (the default is normal正常 logic). 

>If set, SoftwareSerial treats处理 a LOW (0 volts on the pin, normally正常) on the Rx pin as a 1-bit (the idle闲置 state) and a HIGH (5 volts on the pin, normally) as a 0-bit. 

>It also affects影响 the way that it writes to the Tx pin. 

>Default value is false.

>Warning: You should not connect devices which output serial data outside the range that the Arduino can handle处理, normally 0V to 5V, for a board running at 5V, and 0V to 3.3V for a board running at 3.3V.

- Example
```
#include <SoftwareSerial.h>

const byte rxPin = 2;
const byte txPin = 3;

// set up a new serial object
SoftwareSerial mySerial (rxPin, txPin);
```

### available()

- Description

>Get the number of bytes (characters) available for reading from a software serial port. 
>This is data that's already arrived and stored in the serial receive接受 buffer.

- Syntax

>mySerial.available()

- Parameters

>none

- Returns

>the number of bytes available to read

- Example
```
// include the SoftwareSerial library so you can use its functions:
#include <SoftwareSerial.h>

#define rxPin 10
#define txPin 11

// set up a new serial port
SoftwareSerial mySerial =  SoftwareSerial(rxPin, txPin);

void setup()  {
  // define pin modes for tx, rx:
  pinMode(rxPin, INPUT);
  pinMode(txPin, OUTPUT);
  // set the data rate for the SoftwareSerial port
  mySerial.begin(9600);
}

void loop() {
  if (mySerial.available()>0){
  mySerial.read();
 }
}
```

### begin(speed)

- Description
>Sets the speed (baud rate) for the serial communication. 
>Supported baud rates are 300, 600, 1200, 2400, 4800, 9600, 14400, 19200, 28800, 31250, 38400, 57600, and 115200.

- Parameters
>speed: the baud rate (long)

- Returns
>none
- Example
```
// include the SoftwareSerial library so you can use its functions:
#include <SoftwareSerial.h>

#define rxPin 10
#define txPin 11

// set up a new serial port
SoftwareSerial mySerial =  SoftwareSerial(rxPin, txPin);

void setup()  {
  // define pin modes for tx, rx:
  pinMode(rxPin, INPUT);
  pinMode(txPin, OUTPUT);
  // set the data rate for the SoftwareSerial port
  mySerial.begin(9600);
}

void loop() {
  // ...
}
```

### isListening()
- Description

>Tests to see if requested software serial port is actively listening.

测试看看，请求的软件串行端口是否被主动监听。

- Syntax

>mySerial.isListening()

- Parameters

>none

- Returns

>boolean

- Example
```
#include <SoftwareSerial.h>

// software serial : TX = digital pin 10, RX = digital pin 11
SoftwareSerial portOne(10,11);

void setup()
{
  // Start the hardware serial port
  Serial.begin(9600);

  // Start software serial port
  portOne.begin(9600);
}

void loop()
{
  if (portOne.isListening()) {
   Serial.println("Port One is listening!"); 
}
```

### overflow()

- Description

>Tests to see if a software serial buffer overflow has occurred. 

测试以查看是否已发生一个软件串口缓冲器溢出。
>Calling this function clears the overflow flag, meaning that subsequent随后的 calls will return false unless another byte of data has been received and discarded丢弃 in the meantime期间.

调用此函数清除溢出标志，意味着后续调用将返回 false ，除非受到另一个字节的数据并在此期间丢弃。
>The software serial buffer can hold 64 bytes.

软件串口缓冲器能容纳 64 字节。

- Syntax

>mySerial.overflow()

- Parameters

>none

- Returns

>boolean

- Example
```
#include <SoftwareSerial.h>

// software serial : TX = digital pin 10, RX = digital pin 11
SoftwareSerial portOne(10,11);

void setup()
{
  // Start the hardware serial port
  Serial.begin(9600);

  // Start software serial port
  portOne.begin(9600);
}

void loop()
{
  if (portOne.overflow()) {
   Serial.println("SoftwareSerial overflow!"); 
}
```

### peek

- Description

>Return a character that was received on the RX pin of the software serial port. 

>Unlike不相同 read(), however, subsequent随后 calls to this function will return the same character.

>Note that only one SoftwareSerial instance实例 can receive incoming data at a time (select which one with the listen() function).

- Parameters

>none

- Returns

>the character read, or -1 if none is available

- Example
```
SoftwareSerial mySerial(10,11);

void setup()
{
  mySerial.begin(9600);
}

void loop()
{
  char c = mySerial.peek();
}
```

### read
- Description

>Return a character that was received on the RX pin of the software serial port. 
>Note that only one SoftwareSerial instance can receive incoming data at a time (select which one with the listen() function).

- Parameters

>none

- Returns

>the character read, or -1 if none is available

- Example
```
SoftwareSerial mySerial(10,11);

void setup()
{
  mySerial.begin(9600);
}

void loop()
{
  char c = mySerial.read();
}
```

### print(data)

- Description

>Prints data to the transmit pin of the software serial port. >Works the same as the Serial.print() function.

- Parameters

>vary变化, see Serial.print() for details细节

- Returns

>byte
>print() will return the number of bytes written, though reading that number is optional

print() 返回被写的字节数，但读该字节数是可选项。

- Example
```
SoftwareSerial serial(10,11);
int analogValue;

void setup()
{
  serial.begin(9600);
}

void loop()
{
  // read the analog input on pin 0:
  analogValue = analogRead(A0);

  // print it out in many formats:
  serial.print(analogValue);         // print as an ASCII-encoded decimal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, DEC);    // print as an ASCII-encoded decimal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, HEX);    // print as an ASCII-encoded hexadecimal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, OCT);    // print as an ASCII-encoded octal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, BIN);    // print as an ASCII-encoded binary
  serial.print("\t");                // print a tab character
  serial.print(analogValue/4, BYTE); // print as a raw byte value (divide the
                                     // value by 4 because analogRead() returns numbers
                                     // from 0 to 1023, but a byte can only hold values
                                     // up to 255)
  serial.print("\t");                // print a tab character    
  serial.println();                  // print a linefeed character

  // delay 10 milliseconds before the next reading:
  delay(10);
}
```

### println(data)

- Description

>Prints data to the transmit pin of the software serial port, followed by a carriage return 回车 and line feed 换行. 

>Works the same as the Serial.println() function.

- Parameters

>vary, see Serial.println() for details

- Returns

byte
println() will return the number of bytes written, though reading that number is optional

- Example

```
SoftwareSerial serial(10,11);
int analogValue;

void setup()
{
  serial.begin(9600);
}

void loop()
{
  // read the analog input on pin 0:
  analogValue = analogRead(A0);

  // print it out in many formats:
  serial.print(analogValue);         // print as an ASCII-encoded decimal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, DEC);    // print as an ASCII-encoded decimal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, HEX);    // print as an ASCII-encoded hexadecimal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, OCT);    // print as an ASCII-encoded octal
  serial.print("\t");                // print a tab character
  serial.print(analogValue, BIN);    // print as an ASCII-encoded binary
  serial.print("\t");                // print a tab character
  serial.print(analogValue/4, BYTE); // print as a raw byte value (divide the
                                     // value by 4 because analogRead() returns numbers
                                     // from 0 to 1023, but a byte can only hold values
                                     // up to 255)
  serial.print("\t");                // print a tab character    
  serial.println();                  // print a linefeed character

  // delay 10 milliseconds before the next reading:
  delay(10);
}
```


### listen()
- Description

>Enables the selected software serial port to listen. 

开启所选软件串口的监听。
>Only one software serial port can listen at a time; data that arrives到达 for other ports will be discarded丢弃. 

在同一时间内仅有一个软件串口可以被监听；到达其它端口的数据被丢弃。
>Any data already received is discarded during the call to listen() (unless the given instance is already listening).

在调用 listen() 期间任何已经到达的数据被丢弃( 除了这个已经被监听的给定实例 )。
- Syntax

>mySerial.listen()

- Parameters

>mySerial:the name of the instance to listen

- Returns

None

- Example

```
#include 
<SoftwareSerial.h>

// software serial : TX = digital pin 10, RX = digital pin 11
SoftwareSerial portOne(10, 11);

// software serial : TX = digital pin 8, RX = digital pin 9
SoftwareSerial portTwo(8, 9);

void setup()
{
  // Start the hardware serial port
  Serial.begin(9600);

  // Start both software serial ports
  portOne.begin(9600);
  portTwo.begin(9600);

}

void loop()
{
  portOne.listen();

  if (portOne.isListening()) {
   Serial.println("Port One is listening!"); 
}else{
   Serial.println("Port One is not listening!"); 
}

  if (portTwo.isListening()) {
   Serial.println("Port Two is listening!"); 
}else{
   Serial.println("Port Two is not listening!"); 
}

}
```
### write(data)

- Description

>Prints data to the transmit pin of the software serial port as raw原始 bytes. 

>Works the same as the Serial.write() function.

- Parameters

>Serial.write() for details

- Returns

>byte
>write() will return the number of bytes written, though reading that number is optional

- Example
```
SoftwareSerial mySerial(10, 11);

void setup()
{
  mySerial.begin(9600);
}

void loop()
{
  mySerial.write(45); // send a byte with the value 45

   int bytesSent = mySerial.write(“hello”); //send the string “hello” and return the length of the string.
}
```

