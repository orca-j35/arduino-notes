# Variables_变量
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

http://www.arduino.cc/en/Reference/VariableDeclaration
>A variable is a way of naming and storing a value for later use by the program, such as data from a sensor or an intermediate value used in a calculation.

变量时命名和存储数值供程序在以后使用的一种方式，例如从传感器中获取数据，或用于在计算中使用的中间值。

- Declaring Variables 声明变量

>Before they are used, all variables have to be declared. 

>Declaring a variable means defining its type, and optionally, setting an initial value (initializing the variable). 

>Variables do not have to be initialized初始化 (assigned分配 a value) when they are declared, but it is often useful.
```
int inputVariable1;
int inputVariable2 = 0; // both are correct正确的
```
>Programmers should consider考虑 the size of the numbers they wish to store in choosing variable types. 
>程序员应该考虑，他们希望在所选的变量类型中存储的数值的大小。
>Variables will roll over when the value stored exceeds the space assigned to store it. 
>变量将会翻转，当存储的值超出分配来存储它的空间时。
>See below for an example.

- Variable Scope 变量范围

>Another important choice选择 that programmers face is where to declare variables. 

程序员面对的另一个重要问题是在哪里声明变量。
>The specific place that variables are declared influences影响 how various不同 functions in a program will see the variable. 

变量声明的特定位置会影响程序中的不同函数如何看待变量。

>This is called variable scope.
>这被称为变量的作用域。

- Initializing Variables

>Variables may be initialized (assigned a starting value) when they are declared or not. 

当变量被声明时可以被初始化(指定初始值)或不初始化。
>It is always good programming编程 practice习惯 however to double check that a variable has valid data in it, before it is accessed for some other purpose用途.

无论何时在变量被存取并用于其它用途之前，仔细检查变量中有有效的数据，这始终是一个良好的编程习惯。

- Example:
```
 int calibrationVal = 17;  // declare calibrationVal and set initial value声明 calibrationVal 并设置初始值
```

- Variable Rollover 变量翻转

>When variables are made to exceed超出 their maximum capacity容量 they "roll over" back to their minimum capacity, note that this happens in both directions.
```
 int x
   x = -32,768;
   x = x - 1;       // x now contains 32,767 - rolls over in neg. direction
   x = 32,767;
   x = x + 1;       // x now contains -32,768 - rolls over
```
- Using Variables
>Once variables have been declared, they are used by setting the variable equal to the value one wishes to store with the assignment赋值 operator运算符 (single equal sign). 

一旦变量被声明，它们被设置为一个希望被存储的值，通过赋值运算(单等号)。
>The assignment operator tells the program to put whatever is on the right side of the equal sign into the variable on the left side.

赋值运算符告诉程序，放任何等号右侧的值到等号左侧的变量中。
```
inputVariable1 = 7;             // sets the variable named inputVariable1 to 7
inputVariable2 = analogRead(2); // sets the variable named inputVariable2 to the 
                                // (digitized) input voltage read from analog pin #2
```

- Examples
```
 int lightSensVal;
   char currentLetter;
   unsigned long speedOfLight = 186000UL;
   char errorMessage = {"choose another option"}; // see string 
```
>Once a variable has been set (assigned a value), you can test its value to see if it meets certain conditions, or you can use its value directly.

一旦变量已被设置 （分配一个值），您可以测试其值，看看是否它满足一定的条件，或你可以直接使用它的价值。
>For instance, the following code tests whether是否 the inputVariable2 is less than 100, then sets a delay based on inputVariable2 which is a minimum of 100:

例如，下面的代码测试 inputVariable2 是否小于 100，然后设置基于 inputVariable2 延迟时间，延迟时间最小是100；
```
if (inputVariable2 < 100)
{
  inputVariable2 = 100;
}
delay(inputVariable2);
```
This example shows all three useful operations with variables. 

本示例显示变量的所有三种有用操作。
It tests the variable ( `if (inputVariable2 < 100)` ), it sets the variable if it passes通过 the test ( `inputVariable2 = 100` ), and it uses the value of the variable as an input parameter参数 to the delay() function (` delay(inputVariable2) `)

**Style Note**: 风格说明
>You should give your variables descriptive描述性的 names, so as to make your code more readable可读. 

你应该给你的变量描述性的名称，以使您的代码更具可读性
>Variable names like tiltSensor or pushButton help you (and anyone else reading your code) understand what the variable represents代表. 

变量的名字像 tiltSensor (倾斜传感器) 或 pushButton (按钮) 帮助你(和任何阅读你的代码的人)理解变量代表什么。
>Variable names like var or value, on the other hand, do little to make your code readable.

变量名像 var 或 value，在另一方面，无助于使您的代码更具可读性。
>You can name a variable any word that is not already one of the keywords in Arduino. 

你可以命名变量用任何单词，除了已经是 Arduino 中的一个关键词。
>Avoid beginning variable names with numeral characters.

避免变量名以数字字符开头。