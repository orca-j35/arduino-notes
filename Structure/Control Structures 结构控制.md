# Control Structures 结构控制
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. if()
if (conditional) and ==, !=, <, > (comparison operators)
if(条件判断语句)和 ==,!=,<,>(比较运算符)

if 语句和比较运算符同时使用，以测试某个条件是否被满足，例如输入值在某一特定值以上。
if 测试的格式如下：
```
if (someVariable > 50)
{
  // do something here
}
```
程序测试 someVariable 是否大于 50。
假如确实大于，程序执行特定操作。
换句话说，if 后面括号内的表达式为真（称之为测试表达式），则大括号内的语句被执行（称之为执行语句块）。
if 后的表达式若为假，程序会跳过这段代码。

if 语句后的大括号可以省略。
若省略大括号，则只有一条语句（以分号结尾）成为执行语句。
```
if (x > 120) digitalWrite(LEDpin, HIGH); 

if (x > 120)
digitalWrite(LEDpin, HIGH); 

if (x > 120){ digitalWrite(LEDpin, HIGH); } 

if (x > 120){ 
  digitalWrite(LEDpin1, HIGH);
  digitalWrite(LEDpin2, HIGH); 
}                                 
// all are correct 以上所有语句都正确
```
计算括号内的语句时，需要使用一个或多个运算符：

>if, which is used in conjunction with a comparison operator, tests whether a certain condition has been reached, such as an input being above a certain number. The format for an if test is:

>The program tests to see if someVariable is greater than 50. If it is, the program takes a particular action. Put another way, if the statement in parentheses is true, the statements inside the brackets are run. If not, the program skips over the code.

>The brackets may be omitted after an if statement. If this is done, the next line (defined by the semicolon) becomes the only conditional statement.

>The statements being evaluated inside the parentheses require the use of one or more operators:

### 1.1 比较运算符 Comparison Operators:
```
 x == y (x is equal to y)
 x != y (x is not equal to y)
 x <  y (x is less than y)  
 x >  y (x is greater than y) 
 x <= y (x is less than or equal to y) 
 x >= y (x is greater than or equal to y)
```
**警告：**
谨防不小心使用了单等于号 (e.g. if (x = 10) ) 。
单等于号是赋值运算符，设置 x 等于 10(放置值 10 到变量 x 中)。
然而使用双等号 (e.g. if (x == 10) )，是比较运算符，测试 x 是否等于 10。后面的语句只有当 x 等于 10 时才为正，但是前面的语句始终为真。

这是因为 C 按以下规则，因此现在计算 if (x=10) 语句：10 被赋值给 x(记住单等于号是赋值运算)，因此现在 x 包含10。人后 if 对10进行条件计算，10 的计算结果总为 true，因为非零是的计算结果总为 true。因此 if (x = 10) 的计算结果总为 true，当我们使用 if 语句时并不想得到这样的结果。同时 x 将被设置为10，这也不是我们想要的操作。

if 的另外一种分支条件控制结构是 if...else 形式。
https://www.arduino.cc/en/Reference/Else

>Warning:
>Beware of accidentally using the single equal sign (e.g. if (x = 10) ). The single equal sign is the assignment operator, and sets x to 10 (puts the value 10 into the variable x). Instead use the double equal sign (e.g. if (x == 10) ), which is the comparison operator, and tests whether x is equal to 10 or not. The latter statement is only true if x equals 10, but the former statement will always be true.

>This is because C evaluates the statement if (x=10) as follows: 10 is assigned to x (remember that the single equal sign is the assignment operator), so x now contains 10. Then the 'if' conditional evaluates 10, which always evaluates to TRUE, since any non-zero number evaluates to TRUE. Consequently, if (x = 10) will always evaluate to TRUE, which is not the desired result when using an 'if' statement. Additionally, the variable x will be set to 10, which is also not a desired action.

>if can also be part of a branching control structure using the if...else] construction.

## 2. if / else
if/else 比基本的 if 语句能更好的控制代码流，允许将多个测试组合在一起。
比如，检测模拟输入的值，当它小于500时该执行哪些操作，大于或等于500时执行另外的操作。代码如下：
```
if (pinFiveInput < 500)
{
  // action A
}
else
{
  // action B
}
```
else可以进行额外的if检测，所以多个互斥的条件可以同时进行检测。

测试将一个一个进行下去，直到某个测试结果为真，此时该测试相关的执行语句块将被运行，然后程序就跳过剩下的检测，直接执行到if/else的下一条语句。当所有检测都为假时，若存在else语句块，将执行默认的else语句块。

注意else if 语句块可以没有 else 语句块。
使用 elase 时，也可以没有 else if 语句。
else if分支语句的数量无限制。
```
if (pinFiveInput < 500)
{
  // do Thing A
}
else if (pinFiveInput >= 1000)
{
  // do Thing B
}
else
{
  // do Thing C
}
```
另外一种进行多种条件分支判断的语句是switch case语句。

>if/else allows greater control over the flow of code than the basic if statement, by allowing multiple tests to be grouped together. For example, an analog input could be tested and one action taken if the input was less than 500, and another action taken if the input was 500 or greater. The code would look like this:

>else can proceed another if test, so that multiple, mutually exclusive tests can be run at the same time.

>Each test will proceed to the next one until a true test is encountered. When a true test is found, its associated block of code is run, and the program then skips to the line following the entire if/else construction. If no test proves to be true, the default else block is executed, if one is present, and sets the default behavior.

>Note that an else if block may be used with or without a terminating else block and vice versa. An unlimited number of such else if branches is allowed.

>Another way to express branching, mutually exclusive tests, is with the switch case statement.

## 3. switch / case statements
和 if 语句类似，switch...case 控制程序流，通过允许程序员指定在各种条件下执行的不同代码。
特别地，switch语句将变量值和case语句中设定的值进行比较。当一个case语句中的设定值与变量值相同时，这条case语句将被执行。

关键字break可用于退出switch语句，通常每条case语句都以break结尾。如果没有break语句，switch语句将会一直执行接下来的语句（一直向下）直到遇见一个break，或者直到switch语句结尾。
没有遇到 break 不会退出

Example
```
switch (var) {
    case 1:
      //do something when var equals 1
      break;
    case 2:
      //do something when var equals 2
      break;
    default: 
      // if nothing else matches, do the default
      // default is optional
    break;
  }
```
注意：
为了在 case 括号内声明需要的变量。请参照一下示例。
```
switch (var) {
    case 1:
      {
      //do something when var equals 1
      int a = 0;
      .......
      .......
      }
      break;
    default: 
      // if nothing else matches, do the default
      // default is optional
    break;
  }
```
语法 Syntax 
```
switch (var) {
  case label:
    // statements
    break;
  case label:
    // statements
    break;
  default: 
    // statements
  break;
}
```

参数 Parameters

var: the variable whose value to compare to the various cases
var: 用于与下面的case中的标签进行比较的变量值
label: a value to compare the variable to
label: 与变量进行比较的值

>Like if statements, switch...case controls the flow of programs by allowing programmers to specify different code that should be executed in various conditions. In particular, a switch statement compares the value of a variable to the values specified in case statements. When a case statement is found whose value matches匹配 that of the variable, the code in that case statement is run.

>The break keyword exits the switch statement, and is typically used at the end of each case. Without a break statement, the switch statement will continue executing the following expressions ("falling-through") until a break, or the end of the switch statement is reached.

>Note
>Please note that in order to declare variables within a case brackets are needed. An example is showed below.

## 4. while
描述
while 循环会连续的循环，并且是无限循环，直到括号内的判断语句变为假。必须要有能改变判断语句的东西，要不然while循环将永远不会结束。这在您的代码表现为一个递增的变量，或一个外部条件，如传感器的返回值。

Syntax
```
while(expression){
  // statement(s)
}
```
Parameters
expression - a (boolean) C statement that evaluates to true or false

Example
```
var = 0;
while(var < 200){
  // do something repetitive 200 times
  var++;
}
```
See Also

While Loop Tutorial
https://www.arduino.cc/en/Tutorial/WhileStatementConditional?from=Tutorial.WhileLoop

>Description
>while loops will loop continuously, and infinitely, until the expression inside the parenthesis, () becomes false. Something must change the tested variable, or the while loop will never exit. This could be in your code, such as an incremented variable, or an external condition, such as testing a sensor.

## 5. do - while
The do loop works in the same manner as the while loop, with the exception that the condition is tested at the end of the loop, so the do loop will always run at least once.

do…while循环与while循环运行的方式是相近的，不过它的条件判断是在每个循环的最后，所以这个语句至少会被运行一次，然后才被结束。
```
do
{
    // statement block
} while (test condition);
```
Example
```
do
{
  delay(50);          // wait for sensors to stabilize
  x = readSensors();  // check the sensors

} while (x < 100);
```

## 6. break
break is used to exit from a do, for, or while loop, bypassing the normal loop condition. It is also used to exit from a switch statement.

break 用于退出 **do，for，while** 循环，能绕过正常的循环条件。它也能够用于退出**switch**语句。

Example
```
for (x = 0; x < 255; x ++)
{
    analogWrite(PWMpin, x);
    sens = analogRead(sensorPin);  
    if (sens > threshold){
    // bail out on sensor detect 超出探测范围
       x = 0;
       break;
    }  
    delay(50);
}
```
## 7. continue
The continue statement skips the rest of the current iteration of a loop (do, for, or while). It continues by checking the conditional expression of the loop, and proceeding with any subsequent iterations.

continue语句跳过当前循环中剩余的迭代部分（ **do，for 或 while** ）。它通过检查循环条件表达式，并继续进行任何后续迭代。
Example
```
for (x = 0; x < 255; x ++)
{
    if (x > 40 && x < 120){ // create jump in values
    // 当x在40与120之间时，跳过后面两句，即迭代。
     continue;
    }

    analogWrite(PWMpin, x);
    delay(50);
}
```

## 8. return
Terminate a function and return a value from a function to the calling function, if desired.

终止函数和从函数返回一个值给调用函数，如果需要的话。
Syntax:
return;
return value; // both forms are valid 两种形式都有效

Parameters
value: any variable or constant type 任何变量会常量类型

Examples:
A function to compare a sensor input to a threshold
一个比较传感器输入阈值的函数
```
 int checkSensor(){       
    if (analogRead(0) > 400) {
        return 1;
    else{
        return 0;
    }
}
```
The return keyword is handy to test a section of code without having to "comment out" large sections of possibly buggy code.

return关键字可以很方便的测试一段代码，而无需“comment out(注释掉)” 大段的可能存在bug的代码。
```
void loop(){

// brilliant code idea to test here
//写入漂亮的代码来测试这里。
return;

// the rest of a dysfunctional sketch here
// this code will never be executed
//剩下的功能异常的程序
//return后的代码永远不会被执行
}
```

## 9. goto
Transfers program flow to a labeled point in the program
将程序流程传输到程序中的标记点
程序将会从程序中已有的标记点开始运行

**Syntax 语法**
label:
goto label; // sends program flow to the label 发送程序流到lable

**Tip 提示**
在 C 程序中不鼓励使用 goto 语句，某些C编程作者认为goto语句永远是不必要的，但是谨慎的使用，它可以简化某些特定的程序。许多程序员不同意使用goto的原因是， 通过毫无节制地使用goto语句，很容易创建一个程序，这种程序拥有不确定的运行流程，因而无法进行调试。

的确在有的实例中goto语句可以派上用场，并简化代码。
例如在一定的条件用 if 语句来跳出高度嵌入的for循环。

>The use of goto is discouraged in C programming, and some authors of C programming books claim that the goto statement is never necessary, but used judiciously, it can simplify certain programs. The reason that many programmers frown upon the use of goto is that with the unrestrained use of goto statements, it is easy to create a program with undefined program flow, which can never be debugged.

>With that said, there are instances where a goto statement can come in handy, and simplify coding. One of these situations is to break out of deeply nested for loops, or if logic blocks, on a certain condition.

**Example**
```
for(byte r = 0; r < 255; r++){
    for(byte g = 255; g > -1; g--){
        for(byte b = 0; b < 255; b++){
            if (analogRead(0) > 250){ goto bailout;}
            // more statements ... 
        }
    }
}
bailout:
```