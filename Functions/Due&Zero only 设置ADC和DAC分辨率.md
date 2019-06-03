# Due&Zero only 设置ADC和DAC分辨率
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



## 1. analogReadResolution()
**描述 :**
analogReadResolution() 是 Analog API 的一个扩展，仅对 due 和 zero 有效。

设置由 analogRead() 返回值的大小 (in bits)。
默认值是 10 bits(返回值在 0 -1023 之间)，为了向后兼容 avr 基础板。

due 和 zero 有12-bit ADC 的能力，可以被访问并修改分辨率到12.
从 analogRead() 返回的值将在 0 到 4095 之间。

**Syntax**
analogReadResolution(bits)

**Parameters**
bits: 决定通过 analogRead() 函数返回的值的分辨率 (in bits)。
你可以设置该值在 1 到 32 之间。
你可以设置分辨率的值高于 12 ，但是由 analogRead()返回的值会被近似处理。
参考下面的注意，以了解更多信息。

**Returns**
None.

**注意**
假如你设置 analogReadResolution() 的值到一个高于你板卡能力的值，arduino 将只会在最高分辨率反馈时，填充额外的 0 比特位。

例如：在 due 和 zero 上使用 analogReadResolution(16) 命令，将会给出一种近似的 16 bits 数，起先的 12 位包含正真的 ADC 读数，后面 4 位使用 0 填充。

假如你设置  analogReadResolution() 的值到一个低于你主板能力的值，从 adc 读取的额外的低有效位将被丢弃。

使用 16 bit 分辨率（或任何分辨率高于实际硬件功能），允许你将这样的分辨率写入代码，当在未来的板卡上，具有更高 ADC 分辨率的自动处理设备可用时，就无需更改这一行的代码。

**Example**
```
void setup() {
  // open a serial connection
  Serial.begin(9600); 
}

void loop() {
  // read the input on A0 at default resolution (10 bits)
  // and send it out the serial connection 
  analogReadResolution(10);
  Serial.print("ADC 10-bit (default) : ");
  Serial.print(analogRead(A0));

  // change the resolution to 12 bits and read A0
  analogReadResolution(12);
  Serial.print(", 12-bit : ");
  Serial.print(analogRead(A0));

  // change the resolution to 16 bits and read A0
  analogReadResolution(16);
  Serial.print(", 16-bit : ");
  Serial.print(analogRead(A0));

  // change the resolution to 8 bits and read A0
  analogReadResolution(8);
  Serial.print(", 8-bit : ");
  Serial.println(analogRead(A0));

  // a little delay to not hog serial monitor
  delay(100);
}
```

**See also**
Description of the analog input pins
https://www.arduino.cc/en/Tutorial/AnalogInputPins
analogRead()

Description
analogReadResolution() is an extension of the Analog API for the Arduino Due and Zero.

Sets the size (in bits) of the value returned by analogRead(). It defaults to 10 bits (returns values between 0-1023) for backward compatibility with AVR based boards.

The Due and the Zero have 12-bit ADC capabilities that can be accessed by changing the resolution to 12. This will return values from analogRead() between 0 and 4095.

Parameters
bits: determines the resolution (in bits) of the value returned by analogRead() function. You can set this 1 and 32. You can set resolutions higher than 12 but values returned by analogRead() will suffer approximation. See the note below for details.

Note
If you set the analogReadResolution() value to a value higher than your board's capabilities, the Arduino will only report back at its highest resolution padding the extra bits with zeros.

For example: using the Due or the Zero with analogReadResolution(16) will give you an approximated 16-bit number with the first 12 bits containing the real ADC reading and the last 4 bits padded with zeros.

If you set the analogReadResolution() value to a value lower than your board's capabilities, the extra least significant bits read from the ADC will be discarded.

Using a 16 bit resolution (or any resolution higher than actual hardware capabilities) allows you to write sketches that automatically handle devices with a higher resolution ADC when these become available on future boards without changing a line of code.

## 2. analogWriteResolution()
**描述：**
analogWriteResolution() 是 Analog API 的一个扩展，仅对 due 和 zero 有效。

analogWriteResolution() 设置 analogWrite() 函数的分辨率。
analogWrite() 的默认分辨率是 8 bits (值在 0 到 255 之间)，以向后兼容 avr 基本板。

due 具有以下硬件能力：
- 12 pins 默认 8-bit PWM ，如同 AVR 基本板。可以将其改为 12-bit 的分辨率
- 2 引脚是12-bit DAC

通过将 write 分辨率设置为 12，你可以使用的analogWrite() 的值在 0 到 4095 ，以使用全部的 DAC 分辨率，或者设置 PWM 信号无 rolling over。

zore 具有以下硬件能力：
- 10 pins 默认 8 bit PWM，类似 AVR 基本板。可以被改为12 bit 分辨率。
- 1 pin 使用 10 bit DAC (数字-模拟 转换器)

通过设置 write 的分辨率到 10 ，你可以使用的 analogWrite() 的值在 0 到 1023 之间，以利用全部的 DAC 分辨率。

**Syntax**
analogWriteResolution(bits)

**Parameters**
bits: 决定在analogWrite()函数中使用的分辨率 (in bits)的值。
该值的范围从1 到32。
假如你选择的分辨率高于或低于你的板卡的硬件结构，在analogWrite()中所使用的值，如果太高将会被截断，假如太低将会填充 0 。
详见下面的注意事项。

**Returns**
None.

**注意 Note**
假如你设置的 analogWriteResolution() 的值高于你板卡的能力值，arduino 将丢弃额外的 bits。
例如：使用 analogWriteResolution(16) 在 due 12-bit DAC 引脚，仅前 12 bits 的值通过 analogWrite() 被使用，剩余的 4 bits 将被丢弃。

假如你设置的 analogWriteResolution() 的值低于你的板卡的硬件能力，缺少的 bits 将用 0 进行填充，以满足硬件需要的尺寸。例如：使用 analogWriteResolution(8) 在due 12-bit DAC 引脚上， arduino 将 添加 4 个0 到所用的 8 bit 值上，以用于 analogWrite() ，使其达到 12 bits 的要求。

Example
```
void setup(){
  // open a serial connection
  Serial.begin(9600); 
  // make our digital pin an output
  pinMode(11, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(13, OUTPUT);
}

void loop(){
  // read the input on A0 and map it to a PWM pin
  // with an attached LED
  int sensorVal = analogRead(A0);
  Serial.print("Analog Read) : ");
  Serial.print(sensorVal);

  // the default PWM resolution
  analogWriteResolution(8);
  analogWrite(11, map(sensorVal, 0, 1023, 0 ,255));
  Serial.print(" , 8-bit PWM value : ");
  Serial.print(map(sensorVal, 0, 1023, 0 ,255));

  // change the PWM resolution to 12 bits
  // the full 12 bit resolution is only supported
  // on the Due
  analogWriteResolution(12);
  analogWrite(12, map(sensorVal, 0, 1023, 0, 4095));
  Serial.print(" , 12-bit PWM value : ");
  Serial.print(map(sensorVal, 0, 1023, 0, 4095));

  // change the PWM resolution to 4 bits
  analogWriteResolution(4);
  analogWrite(13, map(sensorVal, 0, 1023, 0, 127));
  Serial.print(", 4-bit PWM value : ");
  Serial.println(map(sensorVal, 0, 1023, 0, 127));

  delay(5);
}
```
Description
analogWriteResolution() is an extension of the Analog API for the Arduino Due and Zero.

analogWriteResolution() sets the resolution of the analogWrite() function. It defaults to 8 bits (values between 0-255) for backward compatibility with AVR based boards.

The Due has the following hardare capabilities:

- 12 pins which default to 8-bit PWM, like the AVR-based boards. These can be changed to 12-bit resolution.
- 2 pins with 12-bit DAC (Digital-to-Analog Converter)

By setting the write resolution to 12, you can use analogWrite() with values between 0 and 4095 to exploit the full DAC resolution or to set the PWM signal without rolling over.

The Zero has the following hardare capabilities:

10 pins which default to 8-bit PWM, like the AVR-based boards. These can be changed to 12-bit resolution.
1 pin with 10-bit DAC (Digital-to-Analog Converter).
By setting the write resolution to 10, you can use analogWrite() with values between 0 and 1023 to exploit the full DAC resolution

bits: determines the resolution (in bits) of the values used in the analogWrite() function. The value can range from 1 to 32. If you choose a resolution higher or lower than your board's hardware capabilities, the value used in analogWrite() will be either truncated if it's too high or padded with zeros if it's too low. See the note below for details.

If you set the analogWriteResolution() value to a value higher than your board's capabilities, the Arduino will discard the extra bits. For example: using the Due with analogWriteResolution(16) on a 12-bit DAC pin, only the first 12 bits of the values passed to analogWrite() will be used and the last 4 bits will be discarded.

If you set the analogWriteResolution() value to a value lower than your board's capabilities, the missing bits will be padded with zeros to fill the hardware required size. For example: using the Due with analogWriteResolution(8) on a 12-bit DAC pin, the Arduino will add 4 zero bits to the 8-bit value used in analogWrite() to obtain the 12 bits required.