# Boolean Operators 布尔运算
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库



These can be used inside the condition of an if statement.
这些运算符可以用于if条件句中。

## 1. && (logical and)
True only if both operands are true, e.g.
仅在两个操作数都为真时，才为真，如
```
if (digitalRead(2) == HIGH  && digitalRead(3) == HIGH) { // read two switches 
  // ...
} 
```
is true only if both inputs are high.
仅在两个输入都是 high 时，才为真。

## 2. || (logical or)
True if either operand is true, e.g.
只要一个运算对象为“真”，就为“真”，如：
```
if (x > 0 || y > 0) {
  // ...
}
```
is true if either x or y is greater than 0.
如果x或y是大于0，则为“真”。

## 3. ! (not)
True if the operand is false, e.g.
如果运算对象为“假”，则为“真”，例如
```
if (!x) { 
  // ...
} 
```
is true if x is false (i.e. if x equals 0).
如果x为“假”，则为真（即如果x等于0）。

**警告**
 千万不要误以为，符号为&(单符号)的位运算符“与”就是布尔运算符的“与”符号为&&（双符号）。他们是完全不同的符号。
同样，不要混淆布尔运算符||（双竖）与位运算符“或”符号为| （单竖）。

位运算符〜（波浪号）看起来与布尔运算符not ! 有很大的差别。（正如程序员说：“惊叹号”或“bang”），但你还是要确定哪一个运算符是你想要的。

Examples
```
if (a >= 10 && a <= 20){}   // true if a is between 10 and 20
```


Warning

Make sure you don't mistake the boolean AND operator, && (double ampersand) for the bitwise AND operator & (single ampersand). They are entirely different beasts.

Similarly, do not confuse the boolean || (double pipe) operator with the bitwise OR operator | (single pipe).

The bitwise not ~ (tilde) looks much different than the boolean not ! (exclamation point or "bang" as the programmers say) but you still have to be sure which one you want where.