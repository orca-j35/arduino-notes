# Variable Scope & Qualifiers 变量作用域 & 修饰符
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. Variable Scope 变量作用域
在 arduino 中所使用的 c 编程语言变量中，，有一个名为 作用域(scope) 的属性 。这一点与早期版本的语言形成了对比，如basic，在BASIC语言中所有变量都是全局(global) 变量。

在一个程序中，全局变量对所有函数可见。
局部变量只在声明它们的函数内可见。
在Arduino的环境中，任何在函数（例如，setup(),loop()等）外声明的变量，都是全局变量。

当程序变得更大更复杂时，局部变量是一个有效确保每个函数只能访问其自己变量的方法。这可以防止，当一个函数无意中修改另一个函数使用的变量的程序错误。

有时在一个for循环内声明并初始化一个变量也是很方便的选择。这将创建一个只能从for循环的括号内访问的变量。

Example:
```
int gPWMval;  // any function will see this variable

void setup()
{
  // ...
}

void loop()
{
  int i;    // "i" is only "visible" inside of "loop"
  float f;  // "f" is only "visible" inside of "loop"
  // ...

  for (int j = 0; j <100; j++){
  // variable j can only be accessed inside the for-loop brackets
  }

}
```
Variables in the C programming language, which Arduino uses, have a property called scope. This is in contrast to early versions of languages such as BASIC where every variable is a global variable.

A global variable is one that can be seen by every function in a program. Local variables are only visible to the function in which they are declared. In the Arduino environment, any variable declared outside of a function (e.g. setup(), loop(), etc. ), is a global variable.

When programs start to get larger and more complex, local variables are a useful way to insure that only one function has access to its own variables. This prevents programming errors when one function inadvertently modifies variables used by another function.

It is also sometimes handy to declare and initialize a variable inside a for loop. This creates a variable that can only be accessed from inside the for-loop brackets.

## 2. Static 静态
static关键字用于创建只对某一函数可见的变量。然而，和局部变量不同的是，局部变量在每次调用函数时都会被创建和销毁，静态变量在函数调用后仍然保持着原来的数据。

静态变量只会在函数第一次调用的时候被创建和初始化。

Example
```
/* RandomWalk
* Paul Badger 2007
* RandomWalk wanders up and down randomly between two
* endpoints. The maximum move in one loop is governed by
* the parameter "stepsize".
* A static variable is moved up and down a random amount.
* This technique is also known as "pink noise" and "drunken walk".
* RandomWalk函数在两个终点间随机的上下移动
* 在一个循环中最大的移动由参数“stepsize”决定
* 一个静态变量向上和向下移动一个随机量
* 这种技术也被叫做“粉红噪声”或“醉步”
*/

#define randomWalkLowRange -20
#define randomWalkHighRange 20
int stepsize;

int thisTime;
int total;

void setup()
{
  Serial.begin(9600);
}

void loop()
{        //  tetst randomWalk function //  测试randomWalk 函数
  stepsize = 5;
  thisTime = randomWalk(stepsize);
  Serial.println(thisTime);
   delay(10);
}

int randomWalk(int moveSize){
  static int  place;     // variable to store value in random walk - declared static so that it stores // 在 randomwalk 中存储变量
                         // values in between function calls, but no other functions can change its value  声明为静态因此它在函数调用之间能保持数据，但其他函数无法改变它的值

  place = place + (random(-moveSize, moveSize + 1));

  if (place < randomWalkLowRange){                    // check lower and upper limits //检查上下限
    place = place + (randomWalkLowRange - place);     // reflect number back in positive direction  // 将数字变为正方向
  }
  else if(place > randomWalkHighRange){
    place = place - (place - randomWalkHighRange);     // reflect number back in negative direction  // 将数字变为负方向
  }

  return place;
}
```


The static keyword is used to create variables that are visible to only one function. However unlike local variables that get created and destroyed every time a function is called, static variables persist beyond the function call, preserving their data between function calls.

Variables declared as static will only be created and initialized the first time a function is called.


## 3. volatile 关键字
volatile keyword
volatile这个关键字是变量修饰符，常用在变量类型的前面，以修改编译器和接下来的程序处理该变量的方式。

声明一个 volatile 变量是编译器的一个指令。编译器是一个将你的C/C++代码翻译成机器码的软件，机器码是 arduino 上的 Atmega 芯片能识别的真正指令。

具体来说，它指示编译器编译器从 RAM 而非存储寄存器中加载变量，存储寄存器是程序存储和操作变量的一个临时地方。在某些情况下，存储在寄存器中的变量值可能是不准确的。

如果一个变量所在的代码段可能会意外地导致变量值改变，那此变量应声明为volatile，比如并行多线程等。在arduino中，唯一可能发生这种现象的地方就是和中断有关的代码段，成为中断服务程序。

Example
```
// toggles切换 LED when interrupt pin changes state
//	当中断引脚改变状态时，开闭LED

int pin = 13;
volatile int state = LOW;

void setup()
{
  pinMode(pin, OUTPUT);
  attachInterrupt(0, blink, CHANGE);
}

void loop()
{
  digitalWrite(pin, state);
}

void blink()
{
  state = !state;
}
```
volatile is a keyword known as a variable qualifier, it is usually used before the datatype of a variable, to modify the way in which the compiler and subsequent program treats the variable.

Declaring a variable volatile is a directive to the compiler. The compiler is software which translates your C/C++ code into the machine code, which are the real instructions for the Atmega chip in the Arduino.

Specifically, it directs the compiler to load the variable from RAM and not from a storage register, which is a temporary memory location where program variables are stored and manipulated. Under certain conditions, the value for a variable stored in registers can be inaccurate.

A variable should be declared volatile whenever its value can be changed by something beyond the control of the code section in which it appears, such as a concurrently executing thread. In the Arduino, the only place that this is likely to occur is in sections of code associated with interrupts, called an interrupt service routine.

## 4. const 常量
const keyword
const 关键字代表常量。它是一个变量限定符，用于修改变量的性质，使其变为只读状态。这意味着该变量，就像任何相同类型的其他变量一样使用，但不能改变其值。如果尝试为一个const变量赋值，编译时将会报错。

const 关键字定义的常量，服从作用域控制其它变量的规则。
这一点加上使用 #define 的缺陷 ，使 const 关键字成为定义常量的一个的首选方法。
https://www.arduino.cc/en/Reference/Define

Example
```
const float pi = 3.14;
float x;

// ....

x = pi * 2;    // it's fine to use const's in math

pi = 7;        // illegal - you can't write to (modify) a constant
```
`#define` or const
您可以使用 const 或 ＃define 创建数字或字符串常量。但 arrays, 你只能使用 const。 一般相对`#define`而言 const 是首选 的定义常量语法。

The const keyword stands for constant. It is a variable qualifier that modifies the behavior of the variable, making a variable "read-only". This means that the variable can be used just as any other variable of its type, but its value cannot be changed. You will get a compiler error if you try to assign a value to a const variable.

Constants defined with the const keyword obey the rules of variable scoping that govern other variables. This, and the pitfalls of using#define, makes the const keyword a superior method for defining constants and is preferred over using #define.

You can use either const or #define for creating numeric or string constants. For arrays, you will need to use const. In general const is preferred over #define for defining constants.