# Math 数学运算
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

## 1. min(x, y)
**Description**
Calculates the minimum of two numbers.

**Parameters**
x: the first number, any data type
y: the second number, any data type
X：第一个数字，任何数据类型 
Y：第二个数字，任何数据类型

**Returns**
The smaller of the two numbers.

Examples

```
sensVal = min(sensVal, 100); // assigns sensVal to the smaller of sensVal or 100
                             // ensuring that it never gets above 100.确保它永远不会大于 100。
```

**Note**
Perhaps counter-intuitively, max() is often used to constrain the lower end of a variable's range, while min() is used to constrain the upper end of the range.

可能与直觉相反，max() 方法常被用来约束变量的下限，而 min() 常被用来约束变量的上限。

**Warning**
Because of the way the min() function is implemented, avoid using other functions inside the brackets, it may lead to incorrect results
由于 min() 函数的实现方式，应避免在括号内出现其他函数，这将导致不正确的结果。

```
min(a++, 100);   // avoid this - yields incorrect results 避免这种情况 - 会产生不正确的结果

min(a, 100);
a++;            // use this instead - keep other math outside the function 使用这种形式替代 - 将其他数学运算放在函数之外
```

## 2. max(x, y)
**Description**
Calculates the maximum of two numbers.
计算两个数的最大值。

**Parameters**
x: the first number, any data type
y: the second number, any data type
X：第一个数字，任何数据类型 
Y：第二个数字，任何数据类型
**Returns**
The larger of the two parameter values.
两个参数中较大的一个。

**Example**
```
sensVal = max(senVal, 20); // assigns sensVal to the larger of sensVal or 20
                           // (effectively ensuring that it is at least 20)（有效保障它的值至少为20）
```
**Note**
Perhaps counter-intuitively, max() is often used to constrain the lower end of a variable's range, while min() is used to constrain the upper end of the range.

可能与直觉相反，max()通常用来约束变量最小值，而min()通常用来约束变量的最大值。
**Warning**
Because of the way the max() function is implemented, avoid using other functions inside the brackets, it may lead to incorrect results
由于max()函数的实现方法，要避免在括号内嵌套其他函数，这可能会导致不正确的结果。

```
max(a--, 0);   // avoid this - yields incorrect results 避免此用法，这会导致不正确结果

max(a, 0); 
a--;           //use this instead - keep other math outside the function 用此方法代替，将其他计算放在函数外
```

## 3. abs(x) 绝对值
**Description**
Computes the absolute value of a number.
计算一个数的绝对值。

**Parameters**
x: the number

**Returns**
x: if x is greater than or equal to 0.
-x: if x is less than 0.

**Warning**
Because of the way the abs() function is implemented, avoid using other functions inside the brackets, it may lead to incorrect results.
由于实现ABS（）函数的方法，避免在括号内使用任何函数（括号内只能是数字），否则将导致不正确的结果。

```
abs(a++);   // avoid this - yields incorrect results 避免这种情况，否则它将产生不正确的结果
abs(a); 
a++;          // use this instead - keep other math outside the function 使用这段代码代替上述的错误代码，保证其他函数放在括号的外部
```

## 4. constrain(x, a, b) 范围约束
**Description**
Constrains a number to be within a range.
将一个数约束在一个范围内

**Parameters**
x: the number to constrain, all data types
a: the lower end of the range, all data types
b: the upper end of the range, all data types
x：要被约束的数字，所有的数据类型适用。 
a：该范围的最小值，所有的数据类型适用。 
b：该范围的最大值，所有的数据类型适用。

**Returns**
x: if x is between a and b
a: if x is less than a
b: if x is greater than b

**Example**
```
sensVal = constrain(sensVal, 10, 150);
//传感器返回值的范围限制在10到150之间
```

## 5. map(value, fromLow, fromHigh, toLow, toHigh) 范围映射
**描述：**
将一个数从一个范围映射到另外一个范围。
也就是说，会将 fromLow 到 fromHigh 之间的值映射到 toLow 在 toHigh 之间的值。

不限制值的范围，因为范围外的值有时是刻意的和有用的。如果需要限制的范围， constrain() 函数可以用于此函数之前或之后。

注意，两个范围中的“lower bounds下限”可以比“upper bounds上限”更大或者更小，因此 map() 函数可以用来翻转数值的范围，例如:
`y = map(x, 1, 50, 50, 1);`
这个函数同样可以处理负数，请看下面这个例子:
`y = map(x, 1, 50, 50, -100);`
是有效的并且可以很好的运行。

map() 函数使用整型数进行运算因此不会产生分数，这时运算应该表明它需要这样做。小数的余数部分会被舍去，不会四舍五入或者平均。

**Parameters**
value: the number to map
fromLow: the lower bound of the value's current range
fromHigh: the upper bound of the value's current range
toLow: the lower bound of the value's target range
toHigh: the upper bound of the value's target range
value:需要映射的值 
fromLow:当前范围值的下限 
fromHigh:当前范围值的上限 
toLow:目标范围值的下限 
toHigh:目标范围值的上限

**Returns**
The mapped value.
被映射的值
**Example**

```
/* Map an analog value to 8 bits (0 to 255) */
void setup() {}

void loop()
{
  int val = analogRead(0);
  val = map(val, 0, 1023, 0, 255);
  analogWrite(9, val);
}
```

**Appendix 附录**
For the mathematically inclined, here's the whole function
关于数学的实现，这里是完整函数
```
long map(long x, long in_min, long in_max, long out_min, long out_max)
{
  return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}
```

Description
Re-maps a number from one range to another. That is, a value of fromLow would get mapped to toLow, a value of fromHigh to toHigh, values in-between to values in-between, etc.

Does not constrain values to within the range, because out-of-range values are sometimes intended and useful. The constrain() function may be used either before or after this function, if limits to the ranges are desired.

Note that the "lower bounds" of either range may be larger or smaller than the "upper bounds" so the map() function may be used to reverse a range of numbers, for example

The function also handles negative numbers well, so that this example

is also valid and works well.

The map() function uses integer math so will not generate fractions, when the math might indicate that it should do so. Fractional remainders are truncated, and are not rounded or averaged.


## 6. pow(base, exponent) 幂次方
**Description**
Calculates the value of a number raised to a power. Pow() can be used to raise a number to a fractional power. This is useful for generating exponential mapping of values or curves.

计算一个数的幂次方。Pow()可以用来计算一个数的分数幂。
生成指数映射的值或曲线非常有用。
这用来产生指数幂的数或曲线非常方便。

**Parameters**
base底数: the number (**float**)
exponent指数: the power to which the base is raised (**float**)幂

**Returns**
The result of the exponentiation (double)
一个数的幂次方值（double）
**Example**
See the `fscale` function in the code library.
http://arduino.cc/playground/Main/Fscale

## 7. sqrt(x) 平方根
**Description**
Calculates the square root of a number.
计算一个数的平方根。
**Parameters**
x: the number, any data type
x：被开方数，任何类型
**Returns**
double, the number's square root.
此数的平方根，类型double