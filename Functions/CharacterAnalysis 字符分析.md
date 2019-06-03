# CharacterAnalysis 字符分析
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## isAlphaNumeric(thisChar)
**字母和数字**
Description

Analyse if a char is alphanumeric.

Parameters

thisChar: the char to be analysed.

Returns

True or false.

## isAlpha(thisChar)
**字母**
Description

Analyse if a char is is alpha.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isAscii(thisChar)
**ASCII**
Description

Analyse if a char is ASCII.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isWhitespace(thisChar)
**空格**
Description

Analyse if a char is a white space.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isControl(thisChar)
**control character**

Description

Analyse if a char is a control character.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isDigit(thisChar)
数字
Description

Analyse if a char is a digit.
分析如果字符是一个数字。
Parameters

thisChar: the char to be analysed

Returns

True or false.

## isGraph(thisChar)
可打印字符，不是空格
Description

Analyse if a char is a printable character.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isLowerCase(thisChar)
小写字符
Description

Analyse if a char is a lower case character.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isPrintable(thisChar)
可打印字符
Description

Analyse if a char is a printable character.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isPunct(thisChar)
标点符号
Description

Analyse if a char is punctuation character.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isSpace(thisChar)
空格
Description

Analyse if a char is a space character.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isUpperCase(thisChar)
大写字母
Description

Analyse if a char is an upper case character.

Parameters

thisChar: the char to be analysed

Returns

True or false.

## isHexadecimalDigit(thisChar)
16 进制数字
Description

Analyse if a char is a valid hexadecimal digit.

Parameters

thisChar: the char to be analysed

Returns

True or false.

Examples

```
/*
 Character analysis operators
 字符分析运算符
 Examples using the character analysis operators.
 示例使用字符分析运算符
 Send any byte and the sketch will tell you about it.
 发送任意字节，sketch 将告诉你它是什么字符。
 created 29 Nov 2010
 modified 2 Apr 2012
 by Tom Igoe

 This example code is in the public domain.
 */

void setup() {
  // Open serial communications and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }

  // send an intro:
  Serial.println("send any byte and I'll tell you everything I can about it");
  Serial.println();
}

void loop() {
  // get any incoming bytes:
  if (Serial.available() > 0) {
    int thisChar = Serial.read();

    // say what was sent:
    Serial.print("You sent me: \'");
    Serial.write(thisChar);
    Serial.print("\'  ASCII Value: ");
    Serial.println(thisChar);

    // analyze what was sent:
    if (isAlphaNumeric(thisChar)) {
      Serial.println("it's alphanumeric");
    }
    if (isAlpha(thisChar)) {
      Serial.println("it's alphabetic");
    }
    if (isAscii(thisChar)) {
      Serial.println("it's ASCII");
    }
    if (isWhitespace(thisChar)) {
      Serial.println("it's whitespace");
    }
    if (isControl(thisChar)) {
      Serial.println("it's a control character");
    }
    if (isDigit(thisChar)) {
      Serial.println("it's a numeric digit");
    }
    if (isGraph(thisChar)) {
      Serial.println("it's a printable character that's not whitespace");
    }
    if (isLowerCase(thisChar)) {
      Serial.println("it's lower case");
    }
    if (isPrintable(thisChar)) {
      Serial.println("it's printable");
    }
    if (isPunct(thisChar)) {
      Serial.println("it's punctuation");
    }
    if (isSpace(thisChar)) {
      Serial.println("it's a space character");
    }
    if (isUpperCase(thisChar)) {
      Serial.println("it's upper case");
    }
    if (isHexadecimalDigit(thisChar)) {
      Serial.println("it's a valid hexadecimaldigit (i.e. 0 - 9, a - F, or A - F)");
    }

    // add some space and ask for another byte:
    Serial.println();
    Serial.println("Give me another byte:");
    Serial.println();
  }
}
```