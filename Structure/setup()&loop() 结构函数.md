# setup()&loop() 结构函数
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

## 1. setup()
当启动 sketch 时，setup() 函数被调用。
使用该函数初始化变量、引脚模式、启动使用的库等等。
在每次上电或reset arduino主板时，setup 函数仅仅运行一次。

Example

```
int buttonPin = 3;

void setup()
{
  Serial.begin(9600);
  pinMode(buttonPin, INPUT);
}

void loop()
{
  // ...
}

```

>The setup() function is called when a sketch starts. Use it to initialize variables, pin modes, start using libraries, etc. The setup function will only run once, after each powerup or reset of the Arduino board.

## 2. loop()
在 setup() 函数中初始化和定义了变量，然后执行 loop() 函数。顾名思义,该函数在程序运行过程中不断的循环，根据一些反馈,相应改变执行情况。通过该函数动态控制 Arduino 主控板。

Example

```
const int buttonPin = 3;

// setup initializes serial and the button pin
void setup()
{
  Serial.begin(9600);
  pinMode(buttonPin, INPUT);
}

// loop checks the button pin each time,
// and will send serial if it is pressed
void loop()
{
  if (digitalRead(buttonPin) == HIGH)
    Serial.write('H');
  else
    Serial.write('L');

  delay(1000);
}
```

>After creating a setup() function, which initializes and sets the initial values, the loop() function does precisely what its name suggests, and loops consecutively, allowing your program to change and respond. Use it to actively control the Arduino board.