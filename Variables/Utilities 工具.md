# Utilities 工具
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

## 1. sizeof
描述 Description
sizeof操作符返回一个变量类型的字节数，或者一个数组占用的字节数。
The sizeof operator returns the number of bytes in a variable type, or the number of bytes occupied by an array.

Syntax
sizeof(variable)

Parameters
variable: any variable type or array (e.g. int, float, byte)

Example code
sizeof操作符用来处理数组非常有效，它能很方便的改变数组的大小而不用破坏程序的其他部分。

这个程序一次打印出一个字符串文本的字符。尝试改变一下字符串。
注意：在程序中直接给出数组名。
```
char myStr[] = "this is a test";
int i;

void setup(){
  Serial.begin(9600);
}

void loop() { 
  for (i = 0; i < sizeof(myStr) - 1; i++){
    Serial.print(i, DEC);
    Serial.print(" = ");
    Serial.write(myStr[i]);
    Serial.println();
  }
  delay(5000); // slow down the program
}
```
注意： sizeof 返回总的字节数。因此对于像 ints 这样的双字节变量，for 循环因该像下面这段程序一样处理。
同样注意，正确格式的 string 是以 NULL 结尾，ASCII 码值是 0。
```
for (i = 0; i < (sizeof(myInts)/sizeof(int)); i++) {
  // do something with myInts[i]
}
```
The sizeof operator is useful for dealing with arrays (such as strings) where it is convenient to be able to change the size of the array without breaking other parts of the program.

This program prints out a text string one character at a time. Try changing the text phrase.

Note that sizeof returns the total number of bytes. So for larger variable types such as ints, the for loop would look something like this. Note also that a properly formatted string ends with the NULL symbol, which has ASCII value 0.

## 2. PROGMEM
在 flash(program) 中存储数据，而非 SRAM。
这里有 arduino 上各种可用 memory 的说明。
http://www.arduino.cc/playground/Learning/Memory

PROGMEN 关键词是一个变量修饰符，它仅在  pgmspace.h 中被用于数据类型的定义。
它告诉编译器"把该信息放入 flash memory"，而非放入 SRAM，因为数据通常会放到 SRAM 中。

PROGMEN 是 pgmepace.h 库的一部分，仅在 AVR 构架下可用。因此你需要先在 sketch 的顶部包含这个库：

`#include <avr/pgmspace.h>`

语法 Syntax
```
const dataType variableName[] PROGMEM = {data0, data1, data3...};
```
dataType - any variable type 任何变量类型
variableName - the name for your array of data 

注意：因为 PROGMEN 是一个变量修饰符，没有硬性规定该修饰符所处的位置，一次 arduino 编译器认可下面所有的定义，也就是说这些定义等效。

然而实验表明，在不同版本的 arduino 中 (having to do with GCC version)，PROGMEM 可以在一个位置工作，然而在另一个位置却不行。
下面的"string table"示例会利用 arduino 13 测试其工作状态。
更早版本的 IDE 可能会更好的工作，当 PROGMEM 被包含在变量名后时。
```
const dataType variableName[] PROGMEM = {};   // use this form
const PROGMEM  dataType  variableName[] = {}; // or this form
const dataType PROGMEM variableName[] = {};   // not this one
```
虽然 PROGMEN 可以被用于单个变量，如果你有一大块数据需要存储时，才真的值得这样去做。最简单的应用是数组(或其它的 c 数据结构，但是这超出了我们目前的讨论范围)

使用 PROGMEN 也有两个步骤。在将数据放入 flash memory 中后，还需要特殊方法(函数) 将数据从程序memory 中取回到 sram 中，该方法也在 pgmspace.h 库中被定义，所有我们可以用它做一些有用的事情。
http://www.nongnu.org/avr-libc/user-manual/group__avr__pgmspace.html

示例:
下面的代码片段说明了如何读取和写入 chars (bytes) 和 ints (2 bytes) 到 PROGMEN 中。

```
#include <avr/pgmspace.h>

// save some unsigned ints 保存一些无符号 int
const PROGMEM  uint16_t charSet[]  = { 65000, 32796, 16843, 10, 11234};

// save some chars 保存一些 char
const char signMessage[] PROGMEM  = {"I AM PREDATOR,  UNSEEN COMBATANT. CREATED BY THE UNITED STATES DEPART"};

unsigned int displayInt; 
int k;    // counter variable 控制变量
char myChar;


void setup() {
  Serial.begin(9600);
  while (!Serial);

  // put your setup code here, to run once:
  // read back a 2-byte int
  for (k = 0; k < 5; k++)
  {
    displayInt = pgm_read_word_near(charSet + k);
    Serial.println(displayInt);
  }
  Serial.println();

  // read back a char
  int len = strlen_P(signMessage);
  for (k = 0; k < len; k++)
  {
    myChar =  pgm_read_byte_near(signMessage + k);
    Serial.print(myChar);
  }

  Serial.println();
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

Store data in flash (program) memory instead of SRAM. There's a description of the various types of memory available on an Arduino board.

The PROGMEM keyword is a variable modifier, it should be used only with the datatypes defined in pgmspace.h. It tells the compiler "put this information into flash memory", instead of into SRAM, where it would normally go.

PROGMEM is part of the pgmspace.h library that is available in the AVR architecture only. So you first need to include the library at the top your sketch, like this:

Note that because PROGMEM is a variable modifier, there is no hard and fast rule about where it should go, so the Arduino compiler accepts all of the definitions below, which are also synonymous. However experiments have indicated that, in various versions of Arduino (having to do with GCC version), PROGMEM may work in one location and not in another. The "string table" example below has been tested to work with Arduino 13. Earlier versions of the IDE may work better if PROGMEM is included after the variable name.

While PROGMEM could be used on a single variable, it is really only worth the fuss if you have a larger block of data that needs to be stored, which is usually easiest in an array, (or another C data structure beyond our present discussion).

Using PROGMEM is also a two-step procedure. After getting the data into Flash memory, it requires special methods (functions), also defined in the pgmspace.h library, to read the data from program memory back into SRAM, so we can do something useful with it.

Example
The following code fragments illustrate how to read and write chars (bytes) and ints (2 bytes) to PROGMEM.

### - Arrays of strings 字符串数组
在处理大量文本时，字符串数组会很方便，例如一个 LCD 项目，需要设置一个字符串数。
因为字符串本身就是数组，所以这实际是一个二维数组实例。

这些往往是大型结构，因此将它们放到程序存储器中通常是可取的。
下面的代码阐释了这个想法。
```
/*
 PROGMEM string demo
 How to store a table of strings in program memory (flash), and retrieve them.
如何在程序存储器flash中存储字符串列表，并检索它们。
 Information summarized from:
 资料摘自：
 http://www.nongnu.org/avr-libc/user-manual/pgmspace.html

 Setting up a table (array) of strings in program memory is slightly complicated, but
 here is a good template to follow.
在程序存储器中设置一个字符串列表array，会稍微复杂一些，但是这里有一个很好的模板可以参照。
 Setting up the strings is a two-step process. First define the strings.
 设置字符串有两个步骤。
 首先定义一个 strings
*/

#include <avr/pgmspace.h>
const char string_0[] PROGMEM = "String 0";   // "String 0" etc are strings to store - change to suit. "String 0"等字符串，用来存储-改变以适应。
const char string_1[] PROGMEM = "String 1";
const char string_2[] PROGMEM = "String 2";
const char string_3[] PROGMEM = "String 3";
const char string_4[] PROGMEM = "String 4";
const char string_5[] PROGMEM = "String 5";


// Then set up a table to refer to your strings.
// 然后设置列表以引用你的字符串
const char* const string_table[] PROGMEM = {string_0, string_1, string_2, string_3, string_4, string_5};

char buffer[30];    // make sure this is large enough for the largest string it must hold 确保这是足够大，它必须容纳最大字符串

void setup()
{
  Serial.begin(9600);
  while(!Serial);
  Serial.println("OK");
}


void loop()
{
  /* Using the string table in program memory requires the use of special functions to retrieve the data.使用程序内存中的字符串表，需要使用特殊的函数来检索数据。
     The strcpy_P function copies a string from program space to a string in RAM ("buffer").
     strcpy_P 函数从程序空间中拷贝一个字符串到ram ("buffer")的字符串中。
     Make sure your receiving string in RAM  is large enough to hold whatever
     you are retrieving from program space. 
	 确保在 RAM 中接收的字符串足够的大，无论你从程序空间中接收到字符串有多大都足以被保存。
*/


  for (int i = 0; i < 6; i++)
  {
    strcpy_P(buffer, (char*)pgm_read_word(&(string_table[i]))); // Necessary casts and dereferencing, just copy. 必要的强制转换和取消引用，只是复制。
    Serial.println(buffer);
    delay( 500 );
  }
}
```

**注意 Note**：
请注意，变量必须是全局定义，或使用 static 关键词定义，以便与 PROGMEM 配合工作。

下面的代码在函数内部时不会工作。
```
const char long_str[] PROGMEM = "Hi, I would like to tell you a bit about myself.\n";
```
下面的代码将会工作，即使在函数内部本地定义。
```
const static char long_str[] PROGMEM = "Hi, I would like to tell you a bit about myself.\n"
```

### - The F() macro

When an instruction like :
当一个指令看起来如下：
```
Serial.print("Write something on  the Serial Monitor");
```
被打印的字符串通常被保存在 RAM 中。
假如你的 sketch 在串口监视中打印了很多东西，你可以轻松的塞满 RAM。
假如你有空余的 flash memory 空间，你可通过简单的语法将字符串保存在 FLASH 中，语法如下：
```
Serial.print(F("Write something on the Serial Monitor that is stored in FLASH"));
```

It is often convenient when working with large amounts of text, such as a project with an LCD display, to setup an array of strings. Because strings themselves are arrays, this is in actually an example of a two-dimensional array.

These tend to be large structures so putting them into program memory is often desirable. The code below illustrates the idea.

Please note that variables must be either globally defined, OR defined with the static keyword, in order to work with PROGMEM.
The following code will NOT work when inside a function:

The following code WILL work, even if locally defined within a function:

is used, the string to be printed is normally saved in RAM. If your sketch prints a lot of stuff on the Serial Monitor, you can easily fill the RAM. If you have free FLASH memory space, you can easily indicate that the string must be saved in FLASH using the syntax: