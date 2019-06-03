# Compound Operators 复合运算
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



++ (increment)
-- (decrement)
+= (compound addition)
-= (compound subtraction)
*= (compound multiplication)
/= (compound division)
%= (compound modulo)
&= (compound bitwise and)
|= (compound bitwise or)

## 1. ++ (increment) / -- (decrement)

描述 Description
Increment or decrement a variable
递增或递减一个变量

语法 Syntax

x++;  // increment x by one and returns the old value of x
		//x自增1返回x的旧值
++x;  // increment x by one and returns the new value of x
		// x自增1返回x的新值

x-- ;   // decrement x by one and returns the old value of x 
		 // x自减1返回x的旧值
--x ;   // decrement x by one and returns the new value of x  
		 //x自减1返回x的新值
参数 Parameters
x: an integer or long (possibly unsigned)

返回值 Returns
The original or newly incremented / decremented value of the variable
变量进行自增/自减操作后的原值或新值。

示例 Examples
```
x = 2;
y = ++x;      // x now contains 3, y contains 3
y = x--;      // x contains 2 again, y still contains 3 
```
## 2. += , -= , *= , /= , %=
描述 Description
执行常量或变量与另一个变量的数学运算。
+= 等运算符是以下扩展语法的速记。

Perform a mathematical operation on a variable with another constant or variable. The += (et al) operators are just a convenient shorthand for the expanded syntax, listed below.

语法 Syntax
x += y;   // equivalent to the expression x = x + y;
x -= y;   // equivalent to the expression x = x - y; 
x *= y;   // equivalent to the expression x = x * y; 
x /= y;   // equivalent to the expression x = x / y; 
x %= y;  // equivalent to the expression x = x % y; 

参数 Parameters

x: any variable type
y: any variable type or constant

Examples
```
x = 2;
x += 4;      // x now contains 6
x -= 3;      // x now contains 3
x *= 10;     // x now contains 30
x /= 2;      // x now contains 15
x %= 5;      // x now contains 0
```

## 3. compound bitwise AND (&=)

描述 Description

符合运算按位与运算符（＆=）经常被用来将一个变量和常量进行运算，强迫使变量某些位变为0。
这通常被称为“清除”或“复位”位编程指南。

The compound bitwise AND operator (&=) is often used with a variable and a constant to force particular bits in a variable to the LOW state (to 0). This is often referred to in programming guides as "clearing" or "resetting" bits.

语法 Syntax:

x &= y;   // equivalent to x = x & y; 

参数  Parameters
x: a char, int or long variable
y: an integer constant or char, int, or long

Example:
首先，回顾一下按位与（＆）运算符
First, a review of the Bitwise AND (&) operator
```
   0  0  1  1    operand1
   0  1  0  1    operand2
   ----------
   0  0  0  1    (operand1 & operand2) - returned result
```
任何位与0进行“按位与”操作后被清零，如果myBite是变量
Bits that are "bitwise ANDed" with 0 are cleared to 0 so, if myByte is a byte variable,
```
myByte & B00000000 = 0;
```
因此，任何位与1进行“按位与运算”后保持不变
Bits that are "bitwise ANDed" with 1 are unchanged so,
``` 
myByte & B11111111 = myByte;
```

注意：因为我们用位操作符来操作位 - 所以使用二进制的常量会很方便。这些数值在其它进制的表示形式中，任然是同样的值，只是不容易理解。同样，B00000000是为了标示清楚，0在任何进制中都是0（恩。。有些哲学的味道） 

因此 - 清除（置零）就要用 0 & 变量在该位的 1，而保持其余的位不变，可与常量B11111100进行复合运算按位与（＆=）
```
   1  0  1  0  1  0  1  0    variable  
   1  1  1  1  1  1  0  0    mask
   ----------------------
   1  0  1  0  1  0  0  0
             变量不变
                     位清零
```

将变量替换为x可得到同样结果 

```
   x  x  x  x  x  x  x  x    variable
   1  1  1  1  1  1  0  0    mask
   ----------------------
   x  x  x  x  x  x  0  0
 variable unchanged
                     bits cleared
```


因此:
myByte =  B10101010;
myByte &= B11111100 == B10101000;


Note: because we are dealing处理 with bits in a bitwise operator - it is convenient to use the binary formatter with constants. The numbers are still the same value in other representations, they are just not as easy to understand. Also, B00000000 is shown for clarity, but zero in any number format is zero (hmmm something philosophical there?)

Consequently - to clear (set to zero) bits 0 & 1 of a variable, while leaving the rest of the variable unchanged, use the compound bitwise AND operator (&=) with the constant B11111100

 variable unchanged
                     bits cleared

Here is the same representation with the variable's bits replaced with the symbol x

 variable unchanged
                     bits cleared

## 4. compound bitwise OR (|=)

Description
复合操作符（| =）经常用于变量和常量“设置”（设置为1），尤其是变量中的某一位。

Syntax:
x |= y;   // equivalent to x = x | y; 

Parameters

x: a char, int or long variable
y: an integer constant or char, int, or long

Example:
First, a review of the Bitwise OR (|) operator
首先，回顾一下OR（|）运算符
```
   0  0  1  1    operand1
   0  1  0  1    operand2
   ----------
   0  1  1  1    (operand1 | operand2) - returned result
```
如果变量 myByte 中某一位与0经过"bitwise ORed"运算后不变。
myByte | B00000000 = myByte;

与1经过"bitwise ORed"的位将变为1.
myByte | B11111111 = B11111111;

因此 - 设置变量的某些位为0和1，而变量的其他位不变，可与常量B00000011进行按位与运算（| =）
```
 1  0  1  0  1  0  1  0    variable
   0  0  0  0  0  0  1  1    mask
   ----------------------
   1  0  1  0  1  0  1  1

 variable unchanged
                     bits set
```

接下来的操作相同，只是将变量用x代替
```
 x  x  x  x  x  x  x  x    variable
   0  0  0  0  0  0  1  1    mask
   ----------------------
   x  x  x  x  x  x  1  1

 variable unchanged
                     bits set
```
因此：
```
myByte =  B10101010;
myByte |= B00000011 == B10101011;
```

The compound bitwise OR operator (|=) is often used with a variable and a constant to "set" (set to 1) particular bits in a variable.

Bits that are "bitwise ORed" with 0 are unchanged, so if myByte is a byte variable,

Bits that are "bitwise ORed" with 1 are set to 1 so:

Consequently - to set bits 0 & 1 of a variable, while leaving the rest of the variable unchanged, use the compound bitwise OR operator (|=) with the constant B00000011

Here is the same representation with the variables bits replaced with the symbol x