# Bits and Bytes 位和字节操作
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. lowByte() 提取低位字节
提取变量低位(最右边)字节。
语法：>lowByte(x)
函数：x: a value of any type 任意类型的值
返回值：byte 字节

Description
Extracts the low-order (rightmost) byte of a variable (e.g. a word).
Syntax
lowByte(x)
Parameters
x: a value of any type
Returns
byte

## 2. highByte() 提取高位字节
提取一个 word 的高位(最左边)字节，(或一个更长数据类型的第二低位字节)。
语法：highByte(x)
函数：x: a value of any type 任意类型的值
返回值：byte 字节

Description
Extracts the high-order (leftmost) byte of a word (or the second lowest byte of a larger data type).
Syntax
highByte(x)
Parameters
x: a value of any type
Returns
byte

## 3. bitRead() 读取位
读取一个数的位。
语法：bitRead(x, n)
函数：x : 要读取的数；
		   n : 要读取的位，最小意义的位从0开始(rightmost) 
返回值：返回读取位的 bit 值（0或1）。

Description
Reads a bit of a number.
Syntax
bitRead(x, n)
Parameters
x: the number from which to read
n: which bit to read, starting at 0 for the least-significant (rightmost) bit
Returns
the value of the bit (0 or 1).

## 4. bitWrite() 写位
写入数字变量的某一位。
语法：bitWrite(x, n, b)
参数：x: the numeric数值 variable变量 to which to write
n: which bit of the number to write, starting at 0 for the least-significant (rightmost) bit
b: the value to write to the bit (0 or 1)
返回值：无

Description
Writes a bit of a numeric variable.
Syntax
bitWrite(x, n, b)
Parameters
x: the numeric数值 variable变量 to which to write
n: which bit of the number to write, starting at 0 for the least-significant (rightmost) bit
b: the value to write to the bit (0 or 1)
Returns
none

## 5. bitSet() 设置位
区别于bitWrite()，bitSet()只能写1
设置数值变量的某一位，向该位写入 1。
语法：bitSet(x, n)
函数：x: the numeric variable whose bit to set
n: which bit to set, starting at 0 for the least-significant (rightmost) bit
返回值：无

Description
Sets (writes a 1 to) a bit of a numeric variable.
Syntax
bitSet(x, n)
Parameters
x: the numeric variable whose bit to set
n: which bit to set, starting at 0 for the least-significant (rightmost) bit
Returns
none


## 6. bitClear() 清除位
清除一个数值型数值的指定位(将此位设置成 0)
语法：bitClear(x, n)
参数：x: the numeric variable whose bit to clear
n: which bit to clear, starting at 0 for the least-significant (rightmost) bit
返回值：无

Description
Clears (writes a 0 to) a bit of a numeric variable.
Syntax
bitClear(x, n)
Parameters
x: the numeric variable whose bit to clear
n: which bit to clear, starting at 0 for the least-significant (rightmost) bit
Returns
none

## 7. bit() 位数值
计算指定比特位的数值（0位是1，1位是2，2位4，以此类推）。
语法：bit(n)
参数：n: the bit whose value to compute
返回值：比特位的值。

Description
Computes计算 the value of the specified指定 bit (bit 0 is 1, bit 1 is 2, bit 2 is 4, etc.).
Syntax
bit(n)
Parameters
n: the bit whose value to compute
Returns
the value of the bit