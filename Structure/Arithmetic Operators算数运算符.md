# Arithmetic Operators算数运算符
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

## 1. = 赋值运算
assignment operator (single equal sign)
赋值运算(单等号)

将等号右边的数值赋值给等号左边的变量。

在C语言中，单等号被称为赋值运算符。
它与代数中的等号含义不同，在代数中它表示一个方程或相等。
赋值运算符告诉单片机，将等号的右边的数值或计算表达式的结果，存储在等号左边的变量中。

**Example**
int sensVal;                 // declare声明 an integer variable named sensVal
sensVal = analogRead(0);       // store the (digitized数字化) input voltage at analog pin 0 in SensVal

**编程提示**
要确保赋值运算符（=符号）左侧的变量能够储存右边的数值。如果没有大到足以容纳右边的值，存储在变量中的值将会发生错误。

不要混淆赋值运算符[=]（单等号）与比较运算符[==]（双等号），认为这两个表达式是相等的。
>Stores the value to the right of the equal sign in the variable to the left of the equal sign.

>The single equal sign in the C programming language is called the assignment operator. It has a different meaning than in algebra class where it indicated an equation or equality. The assignment operator tells the microcontroller to evaluate whatever value or expression is on the right side of the equal sign, and store it in the variable to the left of the equal sign.

>Programming Tips

>The variable on the left side of the assignment operator ( = sign ) needs to be able to hold the value stored in it. If it is not large enough to hold a value, the value stored in the variable will be incorrect.

>Don't confuse the assignment operator [ = ] (single equal sign) with the comparison operator [ == ] (double equal signs), which evaluates whether two expressions are equal.

## 2. + - * / 
Addition, Subtraction, Multiplication, & Division
描述：
这些运算符分别返回两个操作数的和、差、积、商。
这些运算是根据操作数的数据类型来计算的，比如 9 和 4 都是 int 类型，所以9 / 4 结果是 2。
这也就代表如果运算结果比数据类型所能容纳的范围要大的话,就会出现溢出.(例如. 1加上一个整数 int类型 32,767 结果变成-32,768)。

如果操作数是不同类型的，结果是“更大”的那种数据类型。

如果操作数中的其中一个是 float类型或者double类型, 就变成了浮点数运算。

Examples
```
y = y + 3;
x = x - 7;
i = j * 6;
r = r / 5;
```
Syntax
```
result = value1 + value2;
result = value1 - value2;
result = value1 * value2;
result = value1 / value2;
```
Parameters:
value1: any variable or constant 任何常量或者变量
value2: any variable or constant 任何常量或者变量

编程提示：
- 整型常量的默认值是int类型，所以一些整型常量的计算会导致溢出。(比如: 60 * 1000 会得到一个负数结果。那么if(60*1000 > 0) ，if得到的是一个false值。)
- 在选择变量的数据类型时,一定要保证变量类型的范围要足够大,以至于能容纳下你的运算结果.
- 要知道你的变量在哪个点会“翻转”,两个方向上都得注意.如: (0 - 1) 或 (0 - - 32768)
- 一些数学运算需要分数，要使用浮点值，但要注意其缺点：占用字节长度大,运算速度慢。
- 使用类型转换符,例如 (int)myFloat 将一个变量强制转换为int类型..


Description

These operators return the sum, difference, product, or quotient (respectively) of the two operands. The operation is conducted using the data type of the operands, so, for example, 9 / 4 gives 2 since 9 and 4 are ints. This also means that the operation can overflow if the result is larger than that which can be stored in the data type (e.g. adding 1 to an int with the value 32,767 gives -32,768). If the operands are of different types, the "larger" type is used for the calculation.

If one of the numbers (operands) are of the type float or of type double, floating point math will be used for the calculation.

Programming Tips:
Know that integer constants default to int, so some constant calculations may overflow (e.g. 60 * 1000 will yield a negative result).
Choose variable sizes that are large enough to hold the largest results from your calculations
Know at what point your variable will "roll over" and also what happens in the other direction e.g. (0 - 1) OR (0 - - 32768)
For math that requires fractions, use float variables, but be aware of their drawbacks: large size, slow computation speeds
Use the cast operator e.g. (int)myFloat to convert one variable type to another on the fly.

## 3. % 取模
(modulo)
Description 描述
Calculates the remainder when one integer is divided by another. It is useful for keeping a variable within a particular range (e.g. the size of an array).

一个整数除以另一个数，其余数称为模。
它有助于保持一个变量在一个特定的范围(例如数组的大小)。

Syntax
```
result = dividend % divisor
```
Parameters

dividend: the number to be divided
divisor: the number to divide by

Returns

the remainder

Examples
```
x = 7 % 5;   // x now contains 2
x = 9 % 5;   // x now contains 4
x = 5 % 5;   // x now contains 0
x = 4 % 5;   // x now contains 4
```
Example Code
```
/* update one value in an array each time through a loop */

int values[10];
int i = 0;

void setup() {}

void loop()
{
  values[i] = analogRead(0);
  i = (i + 1) % 10;   // modulo operator rolls over翻滚 variable  
}
```
Tip 提示
The modulo operator does not work on floats.
模运算符对浮点数不起作用。