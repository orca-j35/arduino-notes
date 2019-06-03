# Communication - Stream
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



Stream
Stream 是字符和二进制数据流的基础的类。
并没有直接声明，但是每当你使用依赖它的函数时，它就会被调用。

Stream 定义了arduino的读取功能。
当使用arduino的任何核心功能中的read（）或相似方法的时候，你可以假设它安全的调用了Stream类。
像print（） 这种，Stream 就在print 类中被引用。

下面这些库，依赖 Stream ：

Serial
https://www.arduino.cc/en/Reference/Serial
Wire
https://www.arduino.cc/en/Reference/Wire
Ethernet Client
https://www.arduino.cc/en/Reference/Ethernet
Ethernet Server
https://www.arduino.cc/en/Reference/Ethernet
SD
https://www.arduino.cc/en/Reference/SD


>Stream is the base class for character and binary based streams. It is not called directly, but invoked whenever you use a function that relies on it.

>Stream defines the reading functions in Arduino. When using any core functionality that uses a read() or similar method, you can safely assume it calls on the Stream class. For functions like print(), Stream inherits from the Print class.

## 1. available()
**Description**
available() 获取 stream 中可用的字节数。
仅指已经到达的字节数。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream
**Syntax**
stream.available()
**Parameters**
stream : an instance of a class that inherits from Stream. 继承于stream 的实例 class
**Returns**
int : the number of bytes available to read
可供读取的字节数。

>available() gets the number of bytes available in the stream. This is only for bytes that have already arrived.
>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 2. read()
**Description**
read() 从输入 stream 中读取字符，到缓冲器中。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.read()
**Parameters**
stream : an instance of a class that inherits from Stream.
**Returns**
the first byte of incoming data available (or -1 if no data is available)
输入数据的第一个可用字节(假如没有数据可用，返回 -1 )

>read() reads characters from an incoming stream to the buffer.

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 3. flush()
**Description**
一旦所有输出字符已发送，清理缓冲区。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.flush()
**Parameters**
stream : an instance of a class that inherits from Stream.
**Returns**
boolean

>flush() clears the buffer once all outgoing characters have been sent.

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 4. find()
**Description**
从 stream 中读取数据，直到给定长度的目标字符串被发现。
如果目标字符串被发现，返回 true；超时返回 flase

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.find(target)
**Parameters**
stream : an instance of a class that inherits from Stream.
target : the string to search for (char)
**Returns**
boolean

>find() reads data from the stream until the target string of given length is found. The function returns true if target string is found, false if timed out.

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 5. findUntil()
**Description**
findUntil() 从stream 中读取数据，直到给定长度的目标字符串被发现，或 终止字符串被发现。
假如目标字符串被发现，函数返回真；如果超时返回 假。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.findUntil(target, terminal)
**Parameters**
stream : an instance of a class that inherits from Stream.
target : the string to search for (char)
terminal : the terminal终止 string in the search (char)
**Returns**
boolean

>findUntil() reads data from the stream until the target string of given length or terminator string is found.
>The function returns true if target string is found, false if timed out
>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 6. peek()
从文件中读取一个字节，但是不会向前进入下一个字节。
这意味着，连续调用 peek() 将会返回相同的值，如果调用 read() 将返回下一个字节。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.peek()
**Parameters**
stream : an instance of a class that inherits from Stream.
Returns
The next byte (or character), or -1 if none is available.
下一个字节(或字符)，如果没有可用字节，将返回 -1。

>Read a byte from the file without advancing to the next one. That is, successive calls to peek() will return the same value, as will the next call to read().

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 7. readBytes()
**Description**
readByte() 从 stream 中读取字符到缓冲区。
如果指定长度的字符被读取，函数将结束；如果超时，函数也将结束 (see setTimeout())
https://www.arduino.cc/en/Reference/StreamSetTimeout
readBytes() 返回的字节数被放置在缓冲区。
0 意味着没有有效数据被发现。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.readBytes(buffer, length)

**Parameters**
stream : an instance of a class that inherits from Stream.
buffer: the buffer to store the bytes in (char[] or byte[])
length : the number of bytes to read (int)

**Returns**
The number of bytes placed in the buffer

>readBytes() read characters from a stream into a buffer. The function terminates if the determined length has been read, or it times out (see setTimeout()).

>readBytes() returns the number of bytes placed in the buffer. A 0 means no valid data was found.

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.


## 8. readBytesUntil()
**Description**
readBytesUntil() 从 stream 中读取字符到缓冲器。
如果终止字符被发现，或指定的长度的字符被读取，又或超时时，函数将终止。
https://www.arduino.cc/en/Reference/StreamSetTimeout

readBytesUntil() 返回的字节被放置在缓冲器中。
0 意味着没有可用的字节被发现。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.readBytesUntil(character, buffer, length)
**Parameters**
stream : an instance of a class that inherits from Stream.
character : the character to search for (char)
buffer: the buffer to store the bytes in (char[] or byte[])
length : the number of bytes to read (int)
**Returns**
The number of bytes placed in the buffer

readBytesUntil() reads characters from a stream into a buffer. The function terminates if the terminator character is detected, the determined length has been read, or it times out (see setTimeout()).

readBytesUntil() returns the number of bytes placed in the buffer. A 0 means no valid data was found.

This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.


## 9. readString()
**Description**
readString() 从 stream 读入字符放入字符串中。
如果超时，该函数将终止。
https://www.arduino.cc/en/Reference/StreamSetTimeout

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.readString()

**Parameters**
none

**Returns**
A string read from a stream

readString() reads characters from a stream into a string. The function terminates if it times out (see setTimeout()).

This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 10. readStringUntil()
**Description**
readStringUntil() 从strem 中读取字符放入字符串。
如果检测到终止字符或出现超时，函数将终止。
https://www.arduino.cc/en/Reference/StreamSetTimeout

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.readString(terminator)

**Parameters**
terminator : the character to search for (char)

**Returns**
The entire string read from a stream, until the terminator character is detected
整个字符串从 stream 中读取，直到检测到终止字符。

>readStringUntil() reads characters from a stream into a string. The function terminates if the terminator character is detected or it times out (see setTimeout()).

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 11. parseInt()
**Description**
parseInt()  从串口缓冲器返回第一个可用的 (long) integer 数。
非 integers (或非减号minus )将被跳过。

>parseInt() returns the first valid (long) integer number from the serial buffer. Characters that are not integers (or the minus sign) are skipped.

**In particular:**特别点
不是数字或减号的初始字符会被跳过。
当在设置的超时范围内，没有字符可被读取时解析停止；读取一个非数字也可使解析停止。
如果发生超时时，没有有效数字被读取，将返回 0；

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.parseInt(list)
stream.parseInt('list', 'char skipchar')

**Parameters**
stream : an instance of a class that inherits from Stream.
list : the stream to check for ints (char) 
skipChar: used to skip the indicated char in the search. 
Used for example to skip thousands divider.
用于跳过搜索指定的字符。
例如用来跳过千分位符。

**Returns**
long

>Initial characters that are not digits or a minus sign, are skipped;
>Parsing stops when no characters have been read for a configurable time-out value, or a non-digit is read;
>If no valid digits were read when the time-out (see Stream.setTimeout()) occurs, 0 is returned;

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.

## 12. parseFloat()
**Description**
parseFloat() 从当前位置返回第一个有效的浮点数。
非数字(或非符号)的初始符将被跳过。parseFloat() 终止于第一个不是浮点数的字符。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.parseFloat(list)

**Parameters**
stream : an instance of a class that inherits from Stream.
list : the stream to check for floats (char)

**Returns**
float

>parseFloat() returns the first valid floating point number from the current position. Initial characters that are not digits (or the minus sign) are skipped. parseFloat() is terminated by the first character that is not a floating point number.

>This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.


## 13. setTimeout()
**Description**
setTimeout() 设置等待 stream 数据的最大毫秒数，默认值是 1000 毫秒。

该函数时 stream 类的一部分，并且会被继承它的 class 调用(Wire,Serial 等)。参看 stream class 主页，以获取更多信息。
https://www.arduino.cc/en/Reference/Stream

**Syntax**
stream.setTimeout(time)

**Parameters**
stream : an instance of a class that inherits from Stream.
time : timeout duration in milliseconds (long).

**Parameters**
None

setTimeout() sets the maximum milliseconds to wait for stream data, it defaults to 1000 milliseconds. 

This function is part of the Stream class, and is called by any class that inherits from it (Wire, Serial, etc). See the Stream class main page for more information.