# Bitwise Operators 位运算符
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

Bitwise AND (&), Bitwise OR (|), Bitwise XOR (^)


Bitwise AND (&)
按位操作符对变量进行位级别的计算。它们能解决很多常见的编程问题。下面的材料大多来自这个非常棒的按位运算指导。
**描述和语法**
下面是所有的运算符的说明和语法。
进一步的详细资料，可参考教程 referenced tutorial。

>The bitwise operators perform their calculations at the bit level of variables. They help solve a wide range of common programming problems. Much of the material below is from an excellent tutorial on bitwise math which may be found here.

>Description and Syntax
>Below are descriptions and syntax for all of the operators. Further details may be found in the referenced tutorial.


## 1. 按位与（&）
Bitwise AND (&)
位操作符与在C + +中是一个&符，用在两个整型表达式之间。按位与运算符对两侧的表达式的每一位独立进行运算，规则是：如果两个运算元都是1，则结果为1，否则输出0。另一种表达方式：
 0  0  1  1    operand1 操作数1
 0  1  0  1    operand2 操作数2
  ----------
 0  0  0  1    (operand1 & operand2) - returned result

在Arduino中，int类型为16位，所以在两个int表达式之间使用&会进行16个并行按位与计算。代码片段就像这样：
```
int a =  92;    // in binary: 0000000001011100
int b = 101;    // in binary: 0000000001100101
int c = a & b;  // result:    0000000001000100, or 68 in decimal.
```
a和b的16位每位都进行按位与计算，计算结果存在c中，二进制结果是01000100，十进制结果是68.

按位与最常见的作用是从整型变量中选取特定的位，也就是屏蔽。见下方的例子。

The bitwise AND operator in C++ is a single ampersand, &, used between two other integer expressions. Bitwise AND operates on each bit position of the surrounding expressions independently, according to this rule: if both input bits are 1, the resulting output is 1, otherwise the output is 0. Another way of expressing this is:

In Arduino, the type int is a 16-bit value, so using & between two int expressions causes 16 simultaneous AND operations to occur. In a code fragment like:

Each of the 16 bits in a and b are processed by using the bitwise AND, and all 16 resulting bits are stored in c, resulting in the value 01000100 in binary, which is 68 in decimal.

One of the most common uses of bitwise AND is to select a particular bit (or bits) from an integer value, often called masking. See below for an example

## 2. 按位或（|）
Bitwise OR (|)
按位或操作符在C++中是|。和&操作符类似，|操作符对两个整型表达式的每一位都进行运算，只是运算规则不同。按位或规则：只要两个位有一个为1则结果为1，否则为0。换句话说：
0  0  1  1    operand1
0  1  0  1    operand2
 ----------
0  1  1  1    (operand1 | operand2) - returned result

这里是一个按位或运算在C + +代码片段：
```
int a =  92;    // in binary: 0000000001011100
int b = 101;    // in binary: 0000000001100101
int c = a | b;  // result:    0000000001111101, or 125 in decimal.
```
The bitwise OR operator in C++ is the vertical bar symbol, |. Like the & operator, | operates independently each bit in its two surrounding integer expressions, but what it does is different (of course). The bitwise OR of two bits is 1 if either or both of the input bits is 1, otherwise it is 0. In other words:

Here is an example of the bitwise OR used in a snippet of C++ code:

### 2.1 Arduino Uno 的示例程序
Example Program for Arduino Uno
按位与和按位或运算常用于端口的读取-修改-写入。在微控制器中，一个端口是一个8位数字，它用于表示引脚状态。对端口进行写入能同时操作所有引脚。

PORTD 是一个内置的常数，是指0,1,2,3,4,5,6,7数字引脚的输出状态。如果某一位为1，着对应管脚为HIGH。（此引脚需要先用pinMode()命令设置为输出）因此如果我们这样写，PORTD=B00110001；则引脚0、4、5状态为HIGH。这里有个小陷阱，我们可能同时更改了引脚0、1的状态，引脚0、1是Arduino串行通信端口，因此我们可能会干扰通信。

我们的算法的程序是：

- 读取 PORT 并用按位 AND 仅清除我们想要控制的引脚。
- 结合控制引脚的新值，使用按位or 修改 PORTD。

```
int i;     // counter variable 计数器
int j;

void setup(){
DDRD = DDRD | B11111100; // set direction bits for pins 2 to 7, leave 0 and 1 untouched (xx | 00 == xx) 设置引脚2~7的方向，0、1脚不变(xx|00==xx)
// same as pinMode(pin, OUTPUT) for pins 2 to 7
// 效果和pinMode(pin,OUTPUT)设置2~7脚为输出一样
Serial.begin(9600);
}

void loop(){
for (i=0; i<64; i++){

PORTD = PORTD & B00000011;  // clear out bits 2 - 7, leave pins 0 and 1 untouched (xx & 11 == xx)
//清除2~7位，0、1保持不变（xx & 11 == xx）
j = (i << 2);               // shift variable up to pins 2 - 7 - to avoid pins 0 and 1
// 变量左移为·2~7脚，避免0、1脚
PORTD = PORTD | j;          // combine the port information with the new information for LED pins
//将新状态和原端口状态结合以控制LED脚
Serial.println(PORTD, BIN); // debug to show masking
// 输出掩盖以便调试
delay(100);
   }
}
```

A common job for the bitwise AND and OR operators is what programmers call Read-Modify-Write on a port. On microcontrollers, a port is an 8 bit number that represents something about the condition of the pins. Writing to a port controls all of the pins at once.

PORTD is a built-in constant that refers to the output states of digital pins 0,1,2,3,4,5,6,7. If there is 1 in an bit position, then that pin is HIGH. (The pins already need to be set to outputs with the pinMode() command.) So if we write PORTD = B00110001; we have made pins 0,4 & 5 HIGH. One slight hitch here is that we may also have changeed the state of Pins 0 & 1, which are used by the Arduino for serial communications so we may have interfered with serial communication.

Our algorithm for the program is:
Get PORTD and clear out only the bits corresponding to the pins we wish to control (with bitwise AND).
Combine the modified PORTD value with the new value for the pins under control (with biwise OR).

## 3. 按位异或（^）
Bitwise XOR (^)
C++中有一个不常见的操作符叫按位异或，也叫做XOR（通常读作”eks-or“）。按位异或使用插入符‘^'表示。此操作符和按位或（|）很相似，区别是如果两个位都为1则结果为0：

    0  0  1  1    operand1
    0  1  0  1    operand2
    ----------
    0  1  1  0    (operand1 ^ operand2)-returned result

按位异或的另一种解释是如果两个位值相同则结果为0，否则为1。

下面是一个简单的代码示例：

    int x = 12;     // binary: 1100
    int y = 10;     // binary: 1010
    int z = x ^ y;  // binary: 0110, or decimal 6

^ 运算常用于整型表达式中一些位的切换(例如，将0 变为1，或将 1 变为0)。
在按位 XOR 运算中，假如掩码位是 1，该位将翻转；假如该位是 0 ，该位将不会翻转，保持原样。下面是 blink digital pin 5 程序。

```
// Blink_Pin_5
// demo for Exclusive OR 演示“异或”
void setup(){
DDRD = DDRD | B00100000; // set digital pin five as OUTPUT 设置数字脚5设置为输出
Serial.begin(9600);
}

void loop(){
PORTD = PORTD ^ B00100000;  // invert bit 5 (digital pin 5), leave others untouched 反转第5位（数字脚5），其他保持不变
delay(100);
}
```

There is a somewhat unusual operator in C++ called bitwise EXCLUSIVE OR, also known as bitwise XOR. (In English this is usually pronounced发音 "eks-or".) The bitwise XOR operator is written using the caret symbol ^. This operator is very similar to the bitwise OR operator |, only it evaluates to 0 for a given bit position when both of the input bits for that position are 1:

Another way to look at bitwise XOR is that each bit in the result is a 1 if the input bits are different, or 0 if they are the same.

Here is a simple code example:

The ^ operator is often used to toggle (i.e. change from 0 to 1, or 1 to 0) some of the bits in an integer expression. In a bitwise XOR operation if there is a 1 in the mask bit, that bit is inverted; if there is a 0, the bit is not inverted and stays the same. Below is a program to blink digital pin 5.

## 4. 按位取反 (~)
Bitwise NOT (~)
按位取反在C+ +语言中是波浪号~。与＆（按位与）和|（按位或）不同，按位取反使用在一个操作数的右侧。按位取反将操作数改变为它的“反面”：0变为1，1变成0。例如：

```
    0  1    operand1
   ----------
    1  0   ~ operand1
    
    int a = 103;    // binary:  0000000001100111
    int b = ~a;     // binary:  1111111110011000 = -104
```
你可能会惊讶地看到结果为像-104这样的负数。
这是因为整数型变量的最高位，即所谓的符号位。
如果最高位是1，这个数字将变为负数。
这个正数和负数的编码被称为补。
想了解更多信息，请参考Wikipedia文章two's complement.
http://en.wikipedia.org/wiki/Twos_complement

顺便说一句，有趣的是，要注意对于任何整数型操作数X，〜X和-X-1是相同的。

有时，对带有符号的整数型操作数进行位操作可以造成一些不必要的意外。

The bitwise NOT operator in C++ is the tilde character ~. Unlike & and |, the bitwise NOT operator is applied to a single operand to its right. Bitwise NOT changes each bit to its opposite: 0 becomes 1, and 1 becomes 0. For example:

You might be surprised to see a negative number like -104 as the result of this operation. This is because the highest bit in an int variable is the so-called sign bit. If the highest bit is 1, the number is interpreted as negative. This encoding of positive and negative numbers is referred to as two's complement. For more information, see the Wikipedia article on two's complement.

As an aside, it is interesting to note that for any integer x, ~x is the same as -x-1.

At times, the sign bit in a signed integer expression can cause some unwanted surprises.


## 5. bitshift left (<<), bitshift right (>>)
描述：
出自 Playground 的 The Bitmath Tutorial 

在C++语言中有两个移位运算符：左移位运算符（<<）和右移运算符（>>）。这些操作符会使左边的操作数向右或向左移动指定的位数，移动的位数由右边的操作数决定。

想了解有关位的更多信息可以点击 这里。
http://playground.arduino.cc/Code/BitMath

Syntax
```
variable << number_of_bits
variable >> number_of_bits
```
Parameters
```
variable - (byte, int, long) 
number_of_bits integer <= 32
```
Example:
```
    int a = 5;        // binary: 0000000000000101
    int b = a << 3;   // binary: 0000000000101000, or 40 in decimal
    int c = b >> 3;   // binary: 0000000000000101, or back to 5 like we started with
```

当你将x左移y位时（x<<y），x中最左边的y位会逐个逐个的丢失，从字面上移出存在：
```
int a = 5;        // binary: 0000000000000101
int b = a << 14;  // binary: 0100000000000000 - the first 1 in 101 was discarded
```

如果你确定位移不会引起数据溢出，你可以简单的把左移运算当做对左运算元进行2的右运算元次方的操作。例如，要产生2的次方，可使用下面的方式：
```
1 <<  0  ==    1
    1 <<  1  ==    2
    1 <<  2  ==    4
    1 <<  3  ==    8
    ...
    1 <<  8  ==  256
    1 <<  9  ==  512
    1 << 10  == 1024
```
当你将x右移y位(x>>y)，如果x最高位是1，位移结果将取决于x的数据类型。如果x是int类型，最高位为符号位，决定x是负数或不是，正如我们上面的讨论。如果x类型为int，则最高位是符号位，正如我们以前讨论过，符号位表示x是正还是负。在这种情况下，由于深奥的历史原因，符号位被复制到较低位：
```
int x = -16;     // binary: 1111111111110000
int y = x >> 3;  // binary: 1111111111111110
```

这种结果，被称为符号扩展，往往不是你想要的行为。你可能希望左边被移入的数是0。右移操作对无符号整型来说会有不同结果，你可以通过数据强制转换改变从左边移入的数据：
```
int x = -16;                   // binary: 1111111111110000
    int y = (unsigned int)x >> 3;  // binary: 0001111111111110
```

如果你能小心的避免符号扩展问题，你可以将右移操作当做对数据除2运算。例如：
```
 int x = 1000;
    int y = x >> 3;   // integer division of 1000 by 8, causing y = 125.
```

Description
From The Bitmath Tutorial in The Playground

There are two bit shift operators in C++: the left shift operator << and the right shift operator >>. These operators cause the bits in the left operand to be shifted left or right by the number of positions specified by the right operand.

More on bitwise math may be found here.

When you shift a value x by y bits (x << y), the leftmost y bits in x are lost, literally shifted out of existence:

If you are certain that none of the ones in a value are being shifted into oblivion, a simple way to think of the left-shift operator is that it multiplies the left operand by 2 raised to the right operand power. For example, to generate powers of 2, the following expressions can be employed:

When you shift x right by y bits (x >> y), and the highest bit in x is a 1, the behavior depends on the exact data type of x. If x is of type int, the highest bit is the sign bit, determining whether x is negative or not, as we have discussed above. In that case, the sign bit is copied into lower bits, for esoteric historical reasons:

If you are careful to avoid sign extension, you can use the right-shift operator >> as a way to divide by powers of 2. For example:

If you are careful to avoid sign extension, you can use the right-shift operator >> as a way to divide by powers of 2. For example: