# Wire_Library
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

Wire Library 双总线接口库

Two Wire Interface (TWI/I2C) for sending and receiving data over a net of devices or sensors.

TWI/I2C 两线接口，用于在设备或传感器网络中发送和接收数据。

[TOC]

https://www.arduino.cc/en/Reference/Wire

该库允许你和 I2C / TWI 设备进行通讯。
在 R3 布局方式(1.0 pinout)的 Arduino 板卡中，SDA (数据线)和 SCL (时钟线)的引脚接头紧挨 AREF 引脚。
Arduino Due 有两组 I2C / TWI 接口，SDA1 和 SCL1 靠近 AREF 引脚，另一组接口在 20 和 21 引脚上。

下方的参考列表，展示了不同版本的板卡中 TWI 所在的位置。
板卡名 I2C / TWI引脚
Uno, Ethernet	A4 (SDA), A5 (SCL)
Mega2560	20 (SDA), 21 (SCL)
Leonardo	2 (SDA), 3 (SCL)
Due	20 (SDA), 21 (SCL), SDA1, SCL1

截止 Arduino 1.0，此库继承自 Stream 函数，使其与其他的读/写库保持一致。
因此，send() 和 receive() 被替换为 read() 和 write()。

### 注意
存在 7 位和 8 位两个版本的 I2C 地址。
7 位用于识别设备，第 8 位决定了读写状态。
Wire 库始终使用 7 位地址。
假如你有 datasheet 或 示例代码使用 8 位地址，你要丢弃最低位(将该值向右移动一位)，产生一个介于 0~127 之间的地址。
不过地址位 0~7 不能被使用，因为这 7 位是保留位。所以第一个可用地址是第 8 位。

### 示例
数字电位器：控制 Analog Devices 公司的 AD5171 数字电位器。
https://www.arduino.cc/en/Tutorial/DigitalPotentiometer
主机读/从机发送：编程使两个 Arduino 板卡中的一个和另一个通讯，使用 I2C 的 Master Reader/Slave Sender 配置。
https://www.arduino.cc/en/Tutorial/MasterReader
主机写/从机接受：编程使两个 Arduino 板卡中的一个和另一个通讯，使用 I2C 的 Master Writer/Slave Receiver 配置。
SFR Ranger Reader: 通过 I2C 接口读超声波测距的距离。
https://www.arduino.cc/en/Tutorial/SFRRangerReader

### 也可以看看
Master Writer
http://arduino.cc/en/Tutorial/MasterWriter
Master Reader
https://www.arduino.cc/en/Tutorial/MasterReader
SFR Ranger Reader
https://www.arduino.cc/en/Tutorial/SFRRangerReader
Digital Potentiometer
https://www.arduino.cc/en/Tutorial/DigitalPotentiometer

## Functions
### 1. Wire.begin() 初始化

Wire.begin()
Wire.begin(address)
初始化 Wire 库，并加入 I2C 总线作为一个主机或从机。
该函数通常只被调用一次。
**参数**：
address: 7 位从机地址(可选)；假如不指定，会作为主机加入到总线中。
**返回值**：
无

Wire.begin()
Wire.begin(address)
Description
Initiate the Wire library and join the I2C bus as a master or slave. This should normally be called only once.
Parameters
address: the 7-bit slave address (optional); if not specified, join the bus as a master.
Returns
None

### 2. Wire.requestFrom() 数据请求
**描述**：
用于主机向从机请求 bytes。该 bytes 随后可使用 available() 和 read() 函数进行检索。
截止 Arduino 1.0.1，requestFrom() 接收一个布尔参数，并改变其行为，以兼容某些 I2C 设备。
假如为真，requestFrom() 在请求后发送一个停止消息，以释放总线。
如果为假，requestFrom() 在请求后发送一个重启指令。此时总线不会被释放，这样可以防止其它主设备在两个请求指令之间发送指令。允许在一个主设备进行控制时发送多个请求。
默认值是真。
**语法**：
Wire.requestFrom(address, quantity)
Wire.requestFrom(address, quantity, stop)
**参数**：
address: 被请求 bytes 的设备的 7-bit 地址。
quantity: 请求的字节数。
stop : 布尔值。真，将在请求后发送停止指令并释放总线。假，将在请求后发送重新启动的指令，保持连接状态。
**返回值**：
byte : 由从设备返回的字节数目。
**参考**
Wire.available()
Wire.read()

Description
Used by the master to request bytes from a slave device. The bytes may then be retrieved with the available() and read() functions.
As of Arduino 1.0.1, requestFrom() accepts a boolean argument changing its behavior for compatibility with certain I2C devices.
If true, requestFrom() sends a stop message after the request, releasing the I2C bus.
If false, requestFrom() sends a restart message after the request. The bus will not be released, which prevents another master device from requesting between messages. This allows one master device to send multiple requests while in control.
The default value is true.

Syntax
Wire.requestFrom(address, quantity)
Wire.requestFrom(address, quantity, stop)

Parameters
address: the 7-bit address of the device to request bytes from
quantity: the number of bytes to request
stop : boolean. true will send a stop message after the request, releasing the bus. false will continually send a restart after the request, keeping the connection active.

Returns
byte : the number of bytes returned from the slave device

See Also
Wire.available()
Wire.read()

### 3. Wire.beginTransmission() 开始传输
**描述**：
根据已给地址，开始向I2C的从机进行传输。
随后，调用函数 write() 对传输的字节进行排列，调用函数 endTransmission() 进行传输 。
**参数**：
address: 传输所指向的设备的 7 位地址。
**返回值**
无
**参考**
Wire.write()
Wire.endTransmission()

Description
Begin a transmission to the I2C slave device with the given address. Subsequently, queue bytes for transmission with the write() function and transmit them by calling endTransmission().

Parameters
address: the 7-bit address of the device to transmit to

Returns
None

See Also
Wire.write()
Wire.endTransmission()

### 4. Wire.endTransmission() 传输结束
**描述**
停止对从机的传输，传输开始时调用 beginTransmission()，传输的字节由 write() 排列。

截止 Arduino 1.0.1，endTransmission() 接收一个布尔参数，并改变其行为，以兼容某些 I2C 设备。
假如为真，endTransmission() 在传输后发送一个停止指令，以释放总线。
如果为假，endTransmission() 在传输后发送一个重启指令。此时总线不会被释放，这样可以防止其它主设备在两个请求指令之间发送指令。允许在一个主设备进行控制时发送多个传输。
默认值是真。
**语法**
Wire.endTransmission()
Wire.endTransmission(stop)
**参数**
stop : 布尔值。为真（true）时将在传输后发送停止指令并释放总线。 为假（false）时将在请求后发送重新启动的指令，保持连接状态。
**返回值**
byte, 表示传输的状态:
0: 成功
1:数据太长以致超出了缓冲区，数据在缓冲区中溢出。
2:在传送地址时接收到NACK（非应答信号）
3:传送数据时接收到NACK（非应答信号）
4:其他错误
**参考**
Wire.beginTransmission()
Wire.write()

Description
Ends a transmission to a slave device that was begun by beginTransmission() and transmits the bytes that were queued by write().

As of Arduino 1.0.1, endTransmission() accepts a boolean argument changing its behavior for compatibility with certain I2C devices.

If true, endTransmission() sends a stop message after transmission, releasing the I2C bus.

If false, endTransmission() sends a restart message after transmission. The bus will not be released, which prevents another master device from transmitting between messages. This allows one master device to send multiple transmissions while in control.

The default value is true.

Syntax
Wire.endTransmission()
Wire.endTransmission(stop)

Parameters
stop : boolean. true will send a stop message, releasing the bus after transmission. false will send a restart, keeping the connection active.

Returns
byte, which indicates the status of the transmission:
0:success
1:data too long to fit in transmit buffer
2:received NACK on transmit of address
3:received NACK on transmit of data
4:other error

See Also
Wire.beginTransmission()
Wire.write()
### 5. Wire.write() 写数据
**描述**：
由从机中写入数据，回应主机的请求。
或排列将要从主机传输给从机的字节（在beginTransimission()和endTransmission()中调用）。
**函数**
Wire.write(value) 
Wire.write(string) 
Wire.write(data, length)
**参数**
value:  以单个字节形式发送的值 
string: 以一串字节的形式发送的字符串 
data: 以字节形式发送的数组 
length: 传输的字节数
**返回值**
byte:write()将返回写入的字节数，但是否读取这个返回值是可选的
Example
```
#include <Wire.h>
byte val = 0;
void setup()
{
  Wire.begin(); // join i2c bus
}
void loop()
{
  Wire.beginTransmission(44); // transmit to device #44 (0x2c)
                              // device address is specified in datasheet
  Wire.write(val);             // sends value byte  
  Wire.endTransmission();     // stop transmitting

  val++;        // increment value
  if(val == 64) // if reached 64th position (max)
  {
    val = 0;    // start over from lowest value
  }
  delay(500);
}

```


Description
Writes data from a slave device in response to a request from a master, or queues bytes for transmission from a master to slave device (in-between calls to beginTransmission() and endTransmission()).

Syntax
Wire.write(value) 
Wire.write(string) 
Wire.write(data, length)

Parameters
value: a value to send as a single byte
string: a string to send as a series of bytes
data: an array of data to send as bytes
length: the number of bytes to transmit

Returns
byte: write() will return the number of bytes written, though reading that number is optional

See also
WireRead()
WireBeginTransmission()
WireEndTransmission()
Serial.write()

### 6. Wire.available() 可用字节数
**描述**
调用 read() 时，返回可用于检索的字节数 。
在主设备中调用函数 requestFrom()后，可在主机中调用此函数。或者从机内部 onReceive() 函数的 handler 参数中调用。
在 onReceive() 函数内部的从机中调用。
available()继承Stream 类.
**参数**
无
**返回值**
可读取的字节数。
**参考**
Wire.read()
Stream.available()

Description
Returns the number of bytes available for retrieval with read(). This should be called on a master device after a call to requestFrom() or on a slave inside the onReceive() handler.
available() inherits from the Stream utility class.

Parameters
None

Returns
The number of bytes available for reading.

See Also
Wire.read()
Stream.available()


### 7. Wire.read() 读数据
**描述**
在调用 requestFrom() 函数后，读取由从设备发送到主设备的一个字节的数据；或由主机发送给从机的字节。
read()从 Stream 类中继承
**函数**
Wire.read() 
**参数**
无
**返回值**
返回下一个接收到的字节
**示例**
```
#include <Wire.h>

void setup()
{
  Wire.begin();        // join i2c bus (address optional for master)加入I2C总线（地址可选为主机）
  Serial.begin(9600);  // start serial for output初始化串口输出
}

void loop()
{
  Wire.requestFrom(2, 6);    // request 6 bytes from slave device #2 向从机 #2请求6个字节

  while(Wire.available())    // slave may send less than requested 从机发送的数据可以少于所请求的
  { 
    char c = Wire.read();    // receive a byte as character 接收一个字节，并设置为字符类型
    Serial.print(c);         // print the character
  }

  delay(500);
}
```
Description
Reads a byte that was transmitted from a slave device to a master after a call to requestFrom() or was transmitted from a master to a slave. read() inherits from the Stream utility class.

Syntax
Wire.read() 

Parameters
none

Returns
The next byte received

See also
WireWrite()
WireAvailable()
WireRequestFrom()
Stream.read()

### 8. Wire.onReceive() 注册从机接受事件
**描述**
注册一个函数，当从设备从主设备接受传输时，该函数被调用。
注册一个函数，当从机接收到主机所发送的数据时调用。
**参数**
handler: 当从设备接受到数据时，该函数被调用；应该采取单精度 int 型参数(从主设备读取的字节数)，并无任何返回值，如：void myHandler(int numBytes)
**返回值**
无
```
#include <Wire.h>

void setup() {
  Wire.begin(8);                // join i2c bus with address #8
  Wire.onReceive(receiveEvent); // register event 注册事件
  Serial.begin(9600);           // start serial for output
}

void loop() {
  delay(100);
}

// function that executes whenever data is received from master
//从主机收到数据时，执行该函数
// this function is registered as an event, see setup()
//该函数被注册为一个事件，看 setup()
void receiveEvent(int howMany) {//从主设备读取的字节数
//显示字符
  while (1 < Wire.available()) { // loop through all but the last
    char c = Wire.read(); // receive byte as a character
    Serial.print(c);         // print the character
  }
//显示 int x
  int x = Wire.read();    // receive byte as an integer
  Serial.println(x);         // print the integer
}

```

Description
Registers a function to be called when a slave device receives a transmission from a master.

Parameters
handler: the function to be called when the slave receives data; this should take a single int parameter (the number of bytes read from the master) and return nothing, e.g.: void myHandler(int numBytes)

Returns
None

See Also
Wire.onRequest()
### 9. Wire.onRequest() 注册从机发送事件
**描述**
注册一个函数,当主机向这个从机请求数据时调用该函数。
**参数**
handler：要调用的函数，不带任何参数，并且没有任何返回值，例如： void MyHandler()
**返回值**
无
```
#include <Wire.h>

void setup() {
  Wire.begin(8); // join i2c bus with address #8
  Wire.onRequest(requestEvent); // register event
}

void loop() {
  delay(100);
}

// function that executes whenever data is requested by master
// this function is registered as an event, see setup()
void requestEvent() {
  Wire.write("hello "); // respond with message of 6 bytes
  // as expected by master
}

```
Description
Register a function to be called when a master requests data from this slave device.

Parameters
handler: the function to be called, takes no parameters and returns nothing, e.g.: void myHandler()

Returns
None

See Also
Wire.onReceive()

### 10.Wire.println() 打印数据
向从设备打印数据。参考 《Arduino 权威指南》 407 页

---
This library allows you to communicate with I2C / TWI devices. 
On the Arduino boards with the R3 layout (1.0 pinout), the SDA (data line) and SCL (clock line) are on the pin headers close to the AREF pin. 
The Arduino Due has two I2C / TWI interfaces SDA1 and SCL1 are near to the AREF pin and the additional one is on pins 20 and 21.

As a reference the table below shows where TWI pins are located on various Arduino boards.
Board	I2C / TWI pins
Uno, Ethernet	A4 (SDA), A5 (SCL)
Mega2560	20 (SDA), 21 (SCL)
Leonardo	2 (SDA), 3 (SCL)
Due	20 (SDA), 21 (SCL), SDA1, SCL1

As of Arduino 1.0, the library inherits from the Stream functions, making it consistent with other read/write libraries. Because of this, send() and receive() have been replaced with read() and write().

Note

There are both 7- and 8-bit versions of I2C addresses. 7 bits identify the device, and the eighth bit determines if it's being written to or read from. The Wire library uses 7 bit addresses throughout. If you have a datasheet or sample code that uses 8 bit address, you'll want to drop the low bit (i.e. shift the value one bit to the right), yielding an address between 0 and 127. However the addresses from 0 to 7 are not used because are reserved so the first address that can be used is 8.

Examples

Digital Potentiometer: Control an Analog Devices AD5171 Digital Potentiometer.
Master Reader/Slave Writer: Program two Arduino boards to communicate with one another in a Master Reader/Slave Sender configuration via the I2C.
Master Writer/Slave receiver:Program two Arduino boards to communicate with one another in a Master Writer/Slave Receiver configuration via the I2C.
SFR Ranger Reader: Read an ultra-sonic range finder interfaced via the I2C.

See also

Master Writer
Master Reader
SFR Ranger Reader
Digital Potentiometer