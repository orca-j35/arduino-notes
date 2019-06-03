# Further Syntax 扩展语法
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

## 1. ; semicolon 分号

Used to end a statement.
用于结束语句。
**Example**
```
int a = 13;
```
**Tip 提示**
Forgetting to end a line in a semicolon will result in a compiler error. The error text may be obvious, and refer to a missing semicolon, or it may not. If an impenetrable or seemingly illogical compiler error comes up, one of the first things to check is a missing semicolon, in the immediate vicinity, preceding the line at which the compiler complained.

在每一行忘记使用分号作为结尾，将导致一个编译错误。错误提示可能会清晰的指向缺少分号的那行，也可能不会。如果弹出一个令人费解或看似不合逻辑的编译器错误，第一件事就是在错误附近检查是否缺少分号。

## 2. {} Curly Braces 大括号
Curly braces 大括号(也称为 "braces括号" 或者 "curly brackets波形括号" ) 是C 语言中重要的组成部分。
它们被用于几个不同的结构中，如下所述，有时可能使初学者混乱。

左大括号“{”必须与一个右大括号“}”形成闭合。这是一个常常被称为括号平衡的条件。在Arduino IDE（集成开发环境）中有一个方便的功能来检查大括号是否平衡。只需选择一个括号，甚至单击紧接括号的插入点，这个括号的“伴侣括号”会被高亮。

目前此功能稍微有些错误，因为IDE会经常会认为在注释中的括号是不正确的。

对于初学者，以及由BASIC语言转向学习C语言的程序员，经常不清楚如何使用括号。毕竟，相同的大括号会替换子程序中的 RETURN 语句，还会替换条件句中的 ENDIF 语句和 FOR 循环中 NEXT 语句。

大括号还会在“return函数”、“endif条件句”以及“loop函数”中被使用到。

由于大括号被用在不同的地方，这有一种很好的编程习惯以避免错误：输入一个大括号后，同时也输入另一个大括号以达到平衡。然后在你的括号之间输入回车，然后再插入语句。这样一来，你的括号就不会变得不平衡了。

不平衡的括号常可导致许多错误，比如令人费解的编译器错误，有时很难在一个程序找到这个错误。由于其不同的用法，括号也是一个程序中非常重要的语法，如果移动大括号一行或两行，往往会极大地影响了程序的意义。

### 2.1 大括号的主要用途
The main uses of curly braces

**Functions 函数**
```
  void myfunction(datatype argument){
    statements(s)
  }
```
**Loops 循环**
```
  while (boolean expression)
  {
     statement(s)
  }

  do
  {
     statement(s)
  } while (boolean expression);

  for (initialisation; termination condition; incrementing expr)
  {
     statement(s)
  } 
```
**Conditional statements 条件语句**
```
  if (boolean expression)
  {
     statement(s)
  }

  else if (boolean expression)
  {
     statement(s)
  } 
  else
  {
     statement(s)
  }
```

>Curly braces (also referred to as just "braces" or as "curly brackets") are a major part of the C programming language. They are used in several different constructs, outlined below, and this can sometimes be confusing for beginners.

>An opening curly brace "{" must always be followed by a closing curly brace "}". This is a condition that is often referred to as the braces being balanced. The Arduino IDE (integrated development environment) includes a convenient feature to check the balance of curly braces. Just select a brace, or even click the insertion point immediately直接 following a brace, and its logical companion will be highlighted.

>At present this feature is slightly buggy as the IDE will often find (incorrectly) a brace in text that has been "commented out."

>Beginning programmers, and programmers coming to C from the BASIC language often find using braces confusing or daunting. After all, the same curly braces replace the RETURN statement in a subroutine (function), the ENDIF statement in a conditional and the NEXT statement in a FOR loop.

>Because the use of the curly brace is so varied, it is good programming practice to type the closing brace immediately after typing the opening brace when inserting a construct which requires curly braces. Then insert some carriage returns between your braces and begin inserting statements. Your braces, and your attitude, will never become unbalanced.

>Unbalanced braces can often lead to cryptic, impenetrable compiler errors that can sometimes be hard to track down in a large program. Because of their varied usages, braces are also incredibly important to the syntax of a program and moving a brace one or two lines will often dramatically affect the meaning of a program.

## 3. Comments 注释
**// (single line comment)**单行注释
**/* */ (multi-line comment)**多行注释

程序中的注释行，用于提醒你自己或其他人程序的工作方式。它们会被编译器忽略掉，也不会传送给处理器，所以它们在Atmega芯片上不占用空间。

注释的唯一作用就是使你自己理解或帮你回忆你的程序是怎么工作的或提醒他人你的程序是如何工作的。编写注释有两种写法：

>Comments are lines in the program that are used to inform yourself or others about the way the program works. They are ignored by the compiler, and not exported to the processor, so they don't take up any space on the Atmega chip.

>Comments only purpose are to help you understand (or remember) how your program works or to inform others how your program works. There are two different ways of marking a line as a comment:

**Example**
```
 x = 5;  // This is a single line comment. Anything after the slashes is a comment 
// 这是一条注释斜杠后面本行内的所有东西是注释
         // to the end of the line

/* this is multiline comment - use it to comment out whole blocks of code
这是多行注释-用于注释一段代码

if (gwb == 0){   // single line comment is OK inside a multiline comment 在多行注释内可使用单行注释
x = 3;           /* but not another multiline comment - this is invalid */
但不允许使用新的多行注释-这是无效的
}
// don't forget the "closing" comment - they have to be balanced!
// 别忘了注释的结尾符号-它们是成对出现的！
*/
```

**Tip 提示**
当测试代码的时候，注释掉一段可能有问题的代码是非常有效的方法。这能使这段代码成为注释而保留在程序中，而编译器能忽略它们。这个方法用于寻找问题代码或当编译器提示出错或错误很隐蔽时很有效。

>When experimenting with code, "commenting out" parts of your program is a convenient way to remove lines that may be buggy. This leaves the lines in the code, but turns them into comments, so the compiler just ignores them. This can be especially useful when trying to locate a problem, or when a program refuses to compile and the compiler error is cryptic or unhelpful.

## 4. #define 宏
`#define` 是一个有用的 C 组件，它允许程序员在程序编译之前给常量命名。在 arduino 中定义常量，不会占用芯片上的任何程序内存空间。在编译时编译器会用事先定义的值来取代这些常量。

然而这样做会产生一些副作用，例如，一个已被 #defined 的常量名被包含在了其他常量名或者变量名中。在这种情况下，文本将被＃defined 定义的数字或文本所取代。

通常情况下， 优先考虑使用 const 关键字替代 #define 来定义常量。

Arduino defines  拥有和 C defines 相同的语法规范：

`#define` is a useful C component that allows the programmer to give a name to a constant value before the program is compiled. Defined constants in arduino don't take up any program memory space on the chip. The compiler will replace references to these constants with the defined value at compile time.

This can have some unwanted side effects though, if for example, a constant name that had been #defined is included in some other constant or variable name. In that case the text would be replaced by the #defined number (or text).

In general, the const keyword is preferred for defining constants and should be used instead of #define.

Arduino defines have the same syntax as C defines:

**Syntax**
`#define constantName value`
Note that the # is necessary.
注意 # 是必须的。

**Example**
`#define ledPin 3`
// The compiler will replace any mention提到 of ledPin with the value 3 at compile time.
//在编译时，编译器将使用数值 3 取代任何用到 ledPin 的地方。

**Tip 提示**
在#define 声明后不能有分号。如果存在分号，编译器会抛出语义不明的错误，甚至关闭页面。

There is no semicolon after the #define statement. If you include one, the compiler will throw cryptic errors further down the page.

`#define ledPin 3`;    // this is an error  错误写法

类似的，在#define声明中包含等号也会产生语义不明的编译错误从而导致关闭页面。
Similarly, including an equal sign after the #define statement will also generate a cryptic compiler error further down the page.

`#define ledPin  = 3 ` // this is also an error  错误写法

## 5. #include
`#include`被用于将外部库包含到你的 sketch 中。
这使得程序员能够访问大量的标准C库（预制的函数组），也能访问专门为 arduino 编写的库。

AVR C libraries 是主要的参考页面，

 AVR C库（Arduino 基于AVR标准语法）语法手册请点击这里。 (AVR 是 Arduino 基于 Atmel 芯片的引用)
http://www.nongnu.org/avr-libc/user-manual/modules.html

注意#include和#define一样，不能在结尾加分号，如果你加了分号编译器将会报错。

**Example**
此例包含了一个库，用于将数据存放在flash空间内而不是ram内。这为动态内存节约了空间，大型表格查表更容易实现。

```
#include <avr/pgmspace.h>
prog_uint16_t myConstants[] PROGMEM = {0, 21140, 702  , 9128,  0, 25764, 8456,
0,0,0,0,0,0,0,0,29810,8968,29762,29762,4500};
```

`#include` is used to include outside libraries in your sketch. This gives the programmer access to a large group of standard C libraries (groups of pre-made functions), and also libraries written especially for Arduino.

The main reference page for AVR C libraries (AVR is a reference to the Atmel chips on which the Arduino is based) is here.

Note that #include, similar to #define, has no semicolon terminator, and the compiler will yield cryptic error messages if you add one.

Example

This example includes a library that is used to put data into the program space flash instead of ram. This saves the ram space for dynamic memory needs and makes large lookup tables more practical.