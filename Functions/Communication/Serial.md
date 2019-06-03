# Serial 串行通讯 USART / URAT

[TOC]

## 1. 简介

[Serial Communication URL](https://www.arduino.cc/reference/en/language/functions/communication/serial/)

Arduino 板卡使用 Serial 与电脑或其它设备进行通讯。所有 Arduino 板卡至少拥有一个串行端口（也称URAT 或USART）：Serial。Serial 工作于数字引脚 0 (RX) 和 1 (TX) ，同时也通过 USB 与电脑通讯。因此，如果需要使用 Serial 功能，便不能同时将引脚 0 和 1 用作数字输入和输出。
我们可以通过 Arduino IDE 内置的串口监视器与 Arduino 板卡进行通讯。在 IDE 的工具栏中点击串口监视器的图标，并将波特率设置为 `begin()` 的实参值，便可与 Arduino 进行串口通讯。

位于 TX/RX 引脚上的串行通讯采用 TTL 电平（根据电路板的设计方式，可能采用 5V 或 3.3V）。请勿将 TX/RX 引脚直接连接到 RS232 串行端口；RS232 的工作电压是 +/- 12V，可能会对 Arduino 造成损坏。

[**Arduino Mega**](https://store.arduino.cc/usa/arduino-mega-2560-rev3)  拥有三个额外的串行端口： **Serial1** 位于 19 (RX) 和 18 (TX) 引脚；**Serial2** 位于 17 (RX) 和 16 (TX) 引脚；**Serial3** 位于 15 (RX) 和 14 (TX) 引脚。如果想使用这些引脚与 PC 进行通讯，则需要自行提供 USB-to-serial 适配器 —— 因为这些额外的串行端口并没有被连接到 Mega 自带的 USB-to-serial 适配器上。如果想要通过这些额外的串行端口与外部 TTL 串行设备进行通讯，那么需要将外部设备的 TX 和 RX 引脚分别连接至 Mega 的 RX 和 TX 引脚，同时将 Mega 的地线与外部设备的地线直接相连。

[**Arduino Due**](https://store.arduino.cc/usa/arduino-due) 拥有三个额外的 3.3V TTL 串行端口：**Serial1** 位于 19 (RX) 和 18 (TX) 引脚；**Serial2** 位于 17 (RX) 和 16 (TX) 引脚；**Serial3** 位于 15 (RX) 和 14 (TX) 引脚。引脚 0 和 1 同样也被连接到了 ATmega16U2 USB-to-TTL 串口芯片的对应引脚，用于连接 USB 调试端口。此外，SAM3X 芯片还拥有一个原生 USB-serial 端口——SerialUSB' 。


[**Arduino Leonardo**](https://store.arduino.cc/usa/arduino-leonardo-with-headers) 在引脚 0 (RX) 和 1 (TX) 上使用 **Serial1** 进行通讯，**Serial1** 也是 5V TTL。**Serial** 用作 USB CDC 通讯。欲了解更多信息，请参阅 Leonardo 的相关页面。

## 2. 函数

### if (Serial) 

- 描述

  用于指示指定的串口是否已准备就绪。
  在 Leonardo 上，`if(Serial)` 用于指示 USB CDC 串行连接是否开放。对于所有其他情况，包括 Leonardo 的 `if (Serial1)` ，将始终返回 `true` 。

- 语法

  *All boards:*

  `if (Serial)`

  *Arduino Leonardo specific:*

  `if (Serial1)`

  *Arduino Mega specific:*

  `if (Serial1)`
  `if (Serial2)`
  `if (Serial3)`

- 参数

  Nothing 

- 返回值

  `boolean`：如果指定串口可用，将返回 `true` 。仅在 Leonardo 的 USB CDC 串行连接准备好之前进行查询时，会返回 `false`。

- 示例代码

  ```c
  void setup() {
   //初始化串行端口，并等待端口开启：
    Serial.begin(9600);
    while (!Serial) {
      ; // 等待串口连接。需要原生USB
    }
  }
  
  void loop() {
   //proceed normally
  }
  ```

### Serial.available()

- 描述

  获取可以从串行端口读取的可用字节（字符）数。该数据表示已经到达并存储在串行接受缓冲区中的字节数——接受缓冲区可存储 64 字节的数据。

  `available()` 继承自 Stream utility class。

- 语法

  `Serial.available()` 

  *Arduino Mega only:*

  `Serial1.available()` 

  `Serial2.available()` 

  `Serial3.available()`

- 参数

  Nothing 

- 返回值

  返回可供读取的字节数

- 示例代码

  以下代码将返回通过串口接受的字符

  ```c
  int incomingByte = 0;	// for incoming serial data
  
  void setup() {
  	Serial.begin(9600);	// opens serial port, sets data rate to 9600 bps
  }
  
  void loop() {
  
  	// reply only when you receive data:
  	if (Serial.available() > 0) {
  		// read the incoming byte:
  		incomingByte = Serial.read();
  
  		// say what you got:
  		Serial.print("I received: ");
  		Serial.println(incomingByte, DEC);
  	}
  }
  ```

  Arduino Mega 的示例：该代码可以将从 Arduino Mega 的某个串口接受到的数据发送到另一个串口。利用这段代码可将串行设备通过 Arduino 连接到电脑。

  ```c
  void setup() {
    Serial.begin(9600);
    Serial1.begin(9600);
  
  }
  
  void loop() {
    // read from port 0, send to port 1:
    if (Serial.available()) {
      int inByte = Serial.read();
      Serial1.print(inByte, DEC);
  
    }
    // read from port 1, send to port 0:
    if (Serial1.available()) {
      int inByte = Serial1.read();
      Serial.print(inByte, DEC);
    }
  }
  ```

### Serial.availableForWrite()

- 描述

  获取可用于写入串行缓冲区的字节（字符）数，并且不会阻塞写入操作。 

- 语法

  `Serial.availableForWrite()`

  *Arduino Mega only:*

  `Serial1.availableForWrite()`
  `Serial2.availableForWrite()`
  `Serial3.availableForWrite()`

- 参数

  Nothing 

- 返回值

  可用于写操作的字节数

### Serial.begin()

- 描述

  设置串行数据传输的速率，单位是 bits/秒 (baud 波特)。与计算机进行通讯时，可使用以下速率中的一个：300, 600, 1200, 2400, 4800, 9600, 14400, 19200, 28800, 38400, 57600, 115200。当然，你可以指定其它速率 —— 例如，通过引脚 0 和 1 连接一个需求特定波特率的元件时。

  可选的第二参数用于配置数据位数、奇偶校验和停止位。默认是 8 数据位、无奇偶校验、1 位停止位。

- 语法

  `Serial.begin(speed)` `Serial.begin(speed, config)`

  *Arduino Mega only:*

  `Serial1.begin(speed)`
  `Serial2.begin(speed)`
  `Serial3.begin(speed)`
  `Serial1.begin(speed, config)`
  `Serial2.begin(speed, config)`
  `Serial3.begin(speed, config)`

- 参数

  `speed`: bits/秒 (baud)  - `long`

  `config`: 设置数据位数、奇偶校验和停止位。 有效值如下

  `SERIAL_5N1`
  `SERIAL_6N1`
  `SERIAL_7N1`
  `SERIAL_8N1` (the default)
  `SERIAL_5N2`
  `SERIAL_6N2`
  `SERIAL_7N2`
  `SERIAL_8N2`
  `SERIAL_5E1`
  `SERIAL_6E1`
  `SERIAL_7E1`
  `SERIAL_8E1`
  `SERIAL_5E2`
  `SERIAL_6E2`
  `SERIAL_7E2`
  `SERIAL_8E2`
  `SERIAL_5O1`
  `SERIAL_6O1`
  `SERIAL_7O1`
  `SERIAL_8O1`
  `SERIAL_5O2`
  `SERIAL_6O2`
  `SERIAL_7O2`
  `SERIAL_8O2`

- 返回值

  Nothing 

- 示例代码

  ```c
  void setup() {
      Serial.begin(9600); // opens serial port, sets data rate to 9600 bps
  }
  
  void loop() {}
  ```

  **Arduino Mega example:** 

  ```c
  // Arduino Mega using all four of its Serial ports
  // (Serial, Serial1, Serial2, Serial3),
  // with different baud rates:
  
  void setup(){
    Serial.begin(9600);
    Serial1.begin(38400);
    Serial2.begin(19200);
    Serial3.begin(4800);
  
    Serial.println("Hello Computer");
    Serial1.println("Hello Serial 1");
    Serial2.println("Hello Serial 2");
    Serial3.println("Hello Serial 3");
  }
  void loop() {}
  ```

### Serial.end()

- 描述

  禁用串行通讯，并允许 RX 和 TX 引脚被用作通用输入和输出。再次调用 `Serial.begin()` 可重新启用串行通讯，启用后 RX 和 TX 便不能再被用作通用输入和输出。

- 语法

  `Serial.end()`

  *Arduino Mega only:*

  `Serial1.end()`
  `Serial2.end()`
  `Serial3.end()`

- 参数

  Nothing

- 返回值

  Nothing

### Serial.find()

- 描述

  需要注意，流解析只过一遍，没有办法折返回去试图发现或得到更多东西。

  `Serial.find( )` 从串口缓冲区中读取数据直到给定长度的目标字符串被发现为止。如果发现目标字符串将返回 `true`；如果超时则返回 `false`。
  假如目标字符串被发现，该函数返回 true，如果超时则为 false。

  `Serial.flush()` 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class。

- 语法

  `Serial.find(target)` 

- 参数

  `target` : 要搜索的字符串(char)

- 返回值

  `boolean`

### Serial.findUntil()

- 描述

  `Serial.findUntil()` 从串口缓冲区中读取数据直到给定长度的目标字符串，或终止字符串被发现为止。
  如果发现目标字符串将返回 `true`；如果超时则返回 `false`。

  `Serial.findUntil()` 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class. 

- 语法

   `Serial.findUntil(target, terminal)` 

- 参数

  `target` : 要搜索的字符串(char)
  `terminal` : 终止搜索的字符串(char)

- 返回值

  `boolean`

### Serial.flush()

- 描述

  等待传出的串行数据完成传输（在 Arduino 1.0 之前，这反而会移除任何已缓存的传入数据 ）。如果需要丢弃接受缓冲区中的所有数据，可使用 `while(Serial.read( ) >= 0);` 

  `flush()` 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/serial/flush) utility class. 

- 语法

  `Serial.flush()`

  *Arduino Mega only:*

  `Serial1.flush()`
  `Serial2.flush()`
  `Serial3.flush()`

- 参数

  Nothing 

- 返回值

  Nothing 

### Serial.parseFloat() 

- 描述

  `Serial.parseFloat()` 返回串行缓冲区中第一个有效的浮点数。解析时，非数值（或减号）字符会被跳过。`parseFloat()`  会在第一个非浮点数值处终止。

  `Serial.parseFloat()` 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class. 

- 语法

  `Serial.parseFloat()` 

- 参数

  Nothing

- 返回值

  `float`

### Serial.parseInt()

- 描述

  在输入串行数据中查找下一个有效的整型。

  `stream.parseInt()` 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class。

  特点：

  - 非数值或负号的初始字符会被跳过；
  - 如果在（可配置）超时值内没有读取到字符，或是读取到了一个非数值字符，解析将停止。
  - 如果发生超时（ `Serial.setTimeout()` ）时，并没有读取到有效数值，将会返回 0；

- 语法

  `Serial.parseInt()` 
  `Serial.parseInt(char skipChar)`

  *Arduino Mega only:*

  `Serial1.parseInt()`
  `Serial2.parseInt()`
  `Serial3.parseInt()`

- 参数

  `skipChar`：在搜索时，用于跳过指定字符。例如用来跳过千位分隔符，如 32,767 将被解析为 32767。

- 返回值

  `long` ：下一个有效整型

### Serial.peek()

- 描述

  返回输入串行数据的下一个字节(字符)，但不会从内部串行缓冲区移除该字节。也就是说，在下一次调用 `read()`之前，如果连续调用 `peek()` 的话，将返回相同的字符。

  `peek()` 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class. 

- 语法

  `Serial.peek()`

  *Arduino Mega only:*

  `Serial1.peek()`
  `Serial2.peek()`
  `Serial3.peek()`

- 参数

  Nothing

- 返回值

  输入串行数据的第一个可用字节（如果没有数据可用，则返回 -1 )- `int` 

### Serial.print()

- 描述

  将数据以人类可读的 ASCII 文本打印到串口。该命令有多重形式。对于数值，会使用 ASCII 字符打印其每个数位。对于浮点数，同样会使用 ASCII 打印其每个数位，默认包含两位小数。Bytes 会以单个字符发送。字符和字符串按原样发送。例如 - 

  - `Serial.print(78) gives "78"`
  - `Serial.print(1.23456) gives "1.23"`
  - `Serial.print('N') gives "N"`
  - `Serial.print("Hello world.") gives "Hello world."` 

  可选的第二参数用于指定底数（格式），允许的值有：`BIN(binary, or base 2)`, `OCT(octal, or base 8)`, `DEC(decimal, or base 10)`, `HEX(hexadecimal, or base 16)`。对于浮点数，第二个参数用于指定小数的位数。例如 -  

  - `Serial.print(78, BIN) gives "1001110"`
  - `Serial.print(78, OCT) gives "116"`
  - `Serial.print(78, DEC) gives "78"`
  - `Serial.print(78, HEX) gives "4E"`
  - `Serial.println(1.23456, 0) gives "1"`
  - `Serial.println(1.23456, 2) gives "1.23"`
  - `Serial.println(1.23456, 4) gives "1.2346"`

  可以将基于 flash-memory 的字符串通过 `F()` 包装(wrapping)后，传递到 `Serial.print()` 中。例如

   `Serial.print(F(“Hello World”))` 

  发送单个 byte，需使用 [Serial.write()](https://www.arduino.cc/reference/en/language/functions/communication/serial/write)。

- 语法

  `Serial.print(val)` 

  `Serial.print(val, format)` 

- 参数

  `val`:  用于打印输出的值 - 任何类型的数据。

- 返回值

  `size_t`: `print()` 返回向外写出的字节数，但可选择是否需获取该返回值。

- 示例代码

  ```cassandra
  /*
  Uses a FOR loop for data and prints a number in various formats.
  */
  int x = 0;    // variable
  
  void setup() {
    Serial.begin(9600);      // open the serial port at 9600 bps:
  }
  
  void loop() {
    // print labels
    Serial.print("NO FORMAT");       // prints a label
    Serial.print("\t");              // prints a tab
  
    Serial.print("DEC");
    Serial.print("\t");
  
    Serial.print("HEX");
    Serial.print("\t");
  
    Serial.print("OCT");
    Serial.print("\t");
  
    Serial.print("BIN");
    Serial.println("\t");           // carriage return after the last label
  
    for(x=0; x< 64; x++){    // only part of the ASCII chart, change to suit
  
      // print it out in many formats:
      Serial.print(x);       // print as an ASCII-encoded decimal - same as "DEC"
      Serial.print("\t\t");  // prints two tabs to accomodate the label lenght
  
      Serial.print(x, DEC);  // print as an ASCII-encoded decimal
      Serial.print("\t");    // prints a tab
  
      Serial.print(x, HEX);  // print as an ASCII-encoded hexadecimal
      Serial.print("\t");    // prints a tab
  
      Serial.print(x, OCT);  // print as an ASCII-encoded octal
      Serial.print("\t");    // prints a tab
  
      Serial.println(x, BIN);  // print as an ASCII-encoded binary
      //                             then adds the carriage return with "println"
      delay(200);            // delay 200 milliseconds
    }
    Serial.println("");      // prints another carriage return
  }
  ```

- 注意和警告

  从 1.0 版本开始，串行传输是异步的；`Serial.print()` 将在任何字符被发送前返回。
  `Serial.print()` 和 `Serial.write()` 不会阻塞程序。在 1.0 之前，代码会等到所有的字符发送之后才返回。在 1.0 及其以后，`Serial.write()` 会在后台中进行（使用了终端处理程序），这使得程序会理解恢复，并执行后续代码。这种方式通常可使程序更加快捷，但是入股需要等待所有字符发送完毕。可以通过在 ` Serial.write()` 之后调用 `Serial.flush()` 来实现。 

### Serial.println()

- 描述

  将数据以人类可读的 ASCII 文本打印到串口，并在文本结尾添加回车字符 (ASCII 13, or '\r')  和换行字符 (ASCII 10, 或 '\n')。该命令采用与 [Serial.print() ](https://www.arduino.cc/reference/en/language/functions/communication/serial/print)相同的格式。 

- 语法

  `Serial.println(val)` 

  `Serial.println(val, format)` 

- 参数

  `val`:  用于打印输出的值 - 任何类型的数据。

  `format`: 指定底数（对于整形数据类型）或小数位数(对于浮点类型)

- 返回值

  `size_t`: `println()`返回向外写出的字节数，但可选择是否需获取该返回值

- 示例代码

  ```c
  /*
   Analog input reads an analog input on analog in 0, prints the value out.
   created 24 March 2006
   by Tom Igoe
   */
  
  int analogValue = 0;    // variable to hold the analog value
  
  void setup() {
    // open the serial port at 9600 bps:
    Serial.begin(9600);
  }
  
  void loop() {
    // read the analog input on pin 0:
    analogValue = analogRead(0);
  
    // print it out in many formats:
    Serial.println(analogValue);       // print as an ASCII-encoded decimal
    Serial.println(analogValue, DEC);  // print as an ASCII-encoded decimal
    Serial.println(analogValue, HEX);  // print as an ASCII-encoded hexadecimal
    Serial.println(analogValue, OCT);  // print as an ASCII-encoded octal
    Serial.println(analogValue, BIN);  // print as an ASCII-encoded binary
  
    // delay 10 milliseconds before the next reading:
    delay(10);
  ```

### Serial.read()

- 描述

  读取传入的串口的数据。

  read() 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class. 

- 语法

  `Serial.read()`

  *Arduino Mega only:*

  `Serial1.read()`
  `Serial2.read()`
  `Serial3.read()`

- 参数

  Nothing

- 返回值

  串行输入数据的第一个可用字节（如果数据不可用，则返回 -1）- `int` 。

- 示例代码

  ```c
  int incomingByte = 0;   // for incoming serial data
  
  void setup() {
          Serial.begin(9600);     // opens serial port, sets data rate to 9600 bps
  }
  
  void loop() {
  
          // send data only when you receive data:
          if (Serial.available() > 0) {
                  // read the incoming byte:
                  incomingByte = Serial.read();
  
                  // say what you got:
                  Serial.print("I received: ");
                  Serial.println(incomingByte, DEC);
          }
  }
  ```

  

###Serial.readBytes()

- 描述

  `Serial.readBytes()` 会从串行端口读取多个字符，并将它们存放到一个缓冲器中。如果已读取了指定数量的字符，或发生了超时 ([Serial.setTimeout()](https://www.arduino.cc/reference/en/language/functions/communication/serial/settimeout))，函数都将终止执行。
  `Serial.readBytes()` 会返回被存放到缓冲器中的字符数。0 意味着没有找到有效数据。

  `Serial.readBytes()` 继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class。

- 语法

  `Serial.readBytes(buffer, length)` 

- 参数

  `buffer`: 用于存储字节的缓冲器 (`char[]` or `byte[]`)

  `length` : 指定需要读取的字节数 (`int`)

- 返回值

  放入缓冲区的字节数 (`size_t`) 。

### Serial.readBytesUntil()
- 描述

  `Serial.readBytesUntil()` 用于从串口缓冲区读取多个字符到一个数组中。如果检测到终止字符，或是已读取了指定数量的字符，又或是发生了超时 ([Serial.setTimeout()](https://www.arduino.cc/reference/en/language/functions/communication/serial/settimeout))，函数都将终止执行。该函数读取的字符不包含终止字符，仅到终止字符的前一个字符为止。终止字符会被保留在串口缓冲区中。

  `Serial.readBytesUntil()` 会返回被存放到缓冲器中的字符数。0 意味着没有找到有效数据。

  `Serial.readBytesUntil()`继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class. 

- 语法

  `Serial.readBytesUntil(character, buffer, length)` 

- 参数

  `character` : 终止字符 (`char`)

  `buffer`: 用于存储字节的缓冲器 (`char[]` or `byte[]`)

  `length` : 指定需要读取的字节数 (`int`)

- 返回值

  `size_t`

### Serial.setTimeout()

- 描述

  `Serial.setTimeout()` 用于设置等待串行数据的最大毫秒数，对于  [serial.readBytesUntil()](https://www.arduino.cc/reference/en/language/functions/communication/serial/readbytesuntil) 或 [serial.readBytes()](https://www.arduino.cc/reference/en/language/functions/communication/serial/readbytes) 有效。默认值是 1000 毫秒。

  `Serial.setTimeout()`继承自 [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream) utility class. 

- 语法

  `Serial.setTimeout(time)` 

- 参数

  `time` : 超时持续时间，单位是毫秒 (`long`) 。

- 返回值

- Nothing

### Serial.write()

- 描述

  向串行端口写入二进制数据。数据会以单个字节，或是以一系列字节被发送。如果发送的 characters 表示的一个数值的不同数位则应用 `print()` 函数代替。

  直接将变量的值以数值形式发送，发送时不会转换为ASCII，如整型值 65，会以65发送，若接收方以 ASCII 显示，则会显示A；若以 hex 显示，则显示41。

- 语法

  `Serial.write(val)`
  `Serial.write(str)`
  `Serial.write(buf, len)`

  *Arduino Mega also supports:*

  `Serial1`, `Serial2`, `Serial3` (in place of `Serial`)

- 参数

  `val`: 以单个字节的形式发送值

  `str`: 以一系列字节的形式发送字符串

  `buf`: 以一系列字节的形式发送一个数组

  `len`: 缓冲器的长度

- 返回值

  `size_t` : `write()` 将返回向外写出的字节数，但可选择是否需获取该返回值。

- 示例代码

```c
void setup(){
  Serial.begin(9600);
}

void loop(){
  Serial.write(45); // send a byte with the value 45

   int bytesSent = Serial.write(“hello”); //send the string “hello” and return the length of the string.
}
```

Description
>Writes binary data to the serial port. 
>This data is sent as a byte or series系列 of bytes; to send the characters representing表示的 the digits of a number use the print() function instead.

### Serial.serialEvent() 

串口数据可用

- 描述

  当数据可用时调用该函数。使用 `Serial.read()`  便可俘获可用数据。

  NB :  目前 `serialEvent()` 不兼容 Esplora, Leonardo, 以及 Micro 。

- 语法

  ```c
  void serialEvent(){
  //statements
  }
  ```

  Arduino Mega only:

  ```c
  void serialEvent1(){
  //statements
  }
  
  void serialEvent2(){
  //statements
  }
  
  void serialEvent3(){
  //statements
  }
  ```

- 参数

  `statements` :  任何有效语句

- 返回值

  Nothing

## 






