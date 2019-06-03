---
title: ESP8266WiFi Client Secure Class
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**client-secure-class.md**

[ESP8266WiFi Library :back:](readme.md#client-secure)


## Client Secure Class

本节中描述的方法和属性特定于 ESP8266。 它们不包括在 Arduino WiFi 库文档中。 在完整记录之前，请参考以下信息。

>   Methods and properties described in this section are specific to ESP8266. They are not covered in [Arduino WiFi library](https://www.arduino.cc/en/Reference/WiFi) documentation. Before they are fully documented please refer to information below.

[TOC]


### loadCertificate( )

从文件系统加载客户端证书。

>   Load client certificate from file system.

```cpp
loadCertificate(file) 
```

*Declarations* 声明
```cpp
#include <FS.h>
#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>

const char* certyficateFile = "/client.cer";
```

*setup() or loop()*
```cpp
if (!SPIFFS.begin()) 
{
  Serial.println("Failed to mount the file system");// 无法安装文件系统
  return;
}

Serial.printf("Opening %s", certyficateFile);
File crtFile = SPIFFS.open(certyficateFile, "r");
if (!crtFile)
{
  Serial.println(" Failed!");
}

WiFiClientSecure client;

Serial.print("Loading %s", certyficateFile);
if (!client.loadCertificate(crtFile))
{
  Serial.println(" Failed!");
}

// proceed with connecting of client to the host
```


### setCertificate( )

从C 数组加载客户端证书。

>   Load client certificate from C array.

```cpp
setCertificate (array, size) 
```

对于一个实际的例子，请检查这个有趣的博客。

>   For a practical example please check [this interesting blog](https://nofurtherquestions.wordpress.com/2016/03/14/making-an-esp8266-web-accessible/).


### Other Function Calls

```cpp
bool  verify (const char *fingerprint, const char *domain_name) 
void  setPrivateKey (const uint8_t *pk, size_t size) 
bool  loadCertificate (Stream &stream, size_t size) 
bool  loadPrivateKey (Stream &stream, size_t size) 
template<typename TFile >  bool  loadPrivateKey (TFile &file)
```

上述功能的文档尚未准备好。

有关代码示例，请参阅单独的部分与示例：arrow_right：专用于客户端安全类。

>   Documentation for the above functions is not yet prepared.
>
>
>   For code samples please refer to separate section with [examples :arrow_right:](client-secure-examples.md) dedicated specifically to the Client Secure Class.