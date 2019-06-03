# atoi
> GitHub@[orca-j35](https://github.com/orca-j35)，所有笔记均托管在 [arduino-notes](https://github.com/orca-j35/arduino-notes) 仓库

#atoi

@(Arduino-Ref)[Arduino, Reference]


atoi (表示 alphanumeric to integer)是把字符串转换成整型数的一个函数

C语言库函数名
atoi
原型：
int atoi(const char *nptr);
UNICODE
_wtoi()

函数说明编辑
参数nptr字符串，如果第一个非空格字符存在，是数字或者正负号则开始做类型转换，之后检测到非数字(包括结束符 \0) 字符时停止转换，返回整型数。否则，返回零。
包含在头文件stdlib.h中

```
#include <stdlib.h>
#include <stdio.h>
 
int main(void)
{
    int n;
    char *str = "12345.67";
    n = atoi(str);
    printf("int=%d\n",n);
    return 0;
}
```
输出：
int = 12345

```
#include <stdlib.h>
#include <stdio.h>
 
int main()
{
    char a[] = "-100";
    char b[] = "123";
    int c;
    c = atoi(a) + atoi(b);
    printf("c=%d\n", c);
    return 0;
}
```
执行结果：
c = 23 