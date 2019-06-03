# Conversion 数据类型转换
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. char()
Description
Converts a value to the char data type.
转换一个值到 char 类型

Syntax
char(x)

Parameters
x: a value of any type

Returns
char

## 2. byte()
Description
Converts a value to the byte data type.

Syntax
byte(x)

Parameters
x: a value of any type

Returns
byte

## 3. int()
Description
Converts a value to the int data type.

Syntax
int(x)

Parameters
x: a value of any type

Returns
int

## 4. word()
Description
Convert a value to the word data type or create a word from two bytes.
将一个值转换为 word 数据类型，或由两个字节创建一个 word。

Syntax
word(x) 
word(h, l)

Parameters
x: a value of any type
h: the high-order (leftmost) byte of the word高阶（最左边）字节 
l: the low-order (rightmost) byte of the word低序（最右边）字节

Returns
word

## 5. long()

Description
Converts a value to the long data type.

Syntax
long(x)

Parameters
x: a value of any type

Returns
long

## 6. float()
Description
Converts a value to the float data type.

Syntax
float(x)

Parameters
x: a value of any type

Returns
float

Notes
See the reference for float for details about the precision and limitations of floating point numbers on Arduino.
见 float 中关于Arduino浮点数的精度和限制的详细信息。参考 数据类型 笔记
https://app.yinxiang.com/shard/s10/nl/119459/3bac95db-c1c4-4f98-bef5-78faf5f95c24