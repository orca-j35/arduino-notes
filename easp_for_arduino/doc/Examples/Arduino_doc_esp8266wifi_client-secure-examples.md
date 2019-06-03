---
title: ESP8266WiFi Client Secure Class - Sketch Examples
---

[ESP8266WiFi Library :back:](readme.md#client-secure)

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/[esp8266wifi](https://github.com/esp8266/Arduino/tree/master/doc/esp8266wifi)/**client-secure-examples.md**

## Client Secure

客户端安全是一个安全的客户端。如果您检查“普通”客户端的类似和简单的示例，下面的应用示例将更容易遵循。 话虽如此，我们将专注于讨论特定于客户端安全的代码。

>   The client secure is a [client](#client) but secure. Application example below will be easier to follow if you check similar and simpler [example](client-examples.md) for the "ordinary" client. That being said we will concentrate on discussing the code that is specific to the client secure.

[TOC]



### Introduction

在此示例中，我们将从安全服务器 https://api.github.com 中检索信息。 该服务器已设置到位，以提供有关GitHub存储库的特定和结构化信息。 例如，我们可能会要求它为我们提供构建状态或最新版本的 esp8266 / Adruino 内核。

esp8266 / Adruino的构建状态可以在存储库的主页或 Travis CI 站点上进行检查，如下所示：

>   In this example we will be retrieving information from a secure server https://api.github.com. This server is set up in place to provide specific and structured information on [GitHub](https://github.com) repositories. For instance, we may ask it to provide us the build status or the latest version of [esp8266 / Adruino](https://github.com/esp8266/Arduino/) core. 
>
>   The build status of esp8266 / Adruino may be checked on the repository's [home page](https://github.com/esp8266/Arduino#using-git-version) or on [Travis CI](https://travis-ci.org/esp8266/Arduino) site as below:

![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-arduino-build-status-travisci.png)

GitHub 提供了一个单独的 API 服务器来访问，这些信息是以JSON的结构化形式。

您可能会猜到我们将使用客户端安全连接 https://api.github.com 服务器并请求构建状态。 如果我们使用 Web 浏览器打开API中提供的特定资源，则应显示以下内容：

>   GitHub provides a separate server with [API](https://developer.github.com/v3/) to access such information is structured form as [JSON](https://en.wikipedia.org/wiki/JSON).
>
>   As you may guess we will use the client secure to contact https://api.github.com server and request the [build status](https://developer.github.com/v3/repos/statuses/#get-the-combined-status-for-a-specific-ref). If we open specific resource provided in the API with a web browser, the following should show up:

![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/esp8266-arduino-build-status-json.png)

我们需要做的是使用客户端安全连接到 https://api.github.com，以获得 / repos / esp8266 / Arduino / commit / master / status，搜索“state”行：“success” 如果找到它，则显示“Build Successful”，否则会显示“Build Failed”。

>   What we need to do, is to use client secure to connect to `https://api.github.com`, to GET `/repos/esp8266/Arduino/commits/master/status`, search for the line `"state": "success"` and display "Build Successful" if we find it, or "Build Failed" if otherwise. 


### The Sketch

在ESP8266WiFi库的示例中，已经可以使用我们所需要的经典草图。 请打开它一步一步地通过它。

>   A classic [sketch](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/HTTPSRequest/HTTPSRequest.ino) that is doing what we need is already available among [examples](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WiFi/examples) of ESP8266WiFi library. Please open it to go through it step by step.


### How to Verify Server's Identity? 

要建立与服务器的安全连接，我们需要验证服务器的身份。 在“常规”计算机上运行的客户端通过将服务器的证书与本地存储的可信根证书列表进行比较来实现。 这样的证书需要数百KB，所以它不是ESP模块的好选择。 作为替代，我们可以使用更小的特定证书的 SHA1 指纹。

在代码的声明部分，我们提供主机的名称和相应的指纹。

>   To establish a secure connection with a server we need to verify server's identity. Clients that run on "regular" computers do it by comparing server's certificate with locally stored list of trusted root certificates. Such certificates take several hundreds of KB, so it is not a good option for an ESP module. As an alternative we can use much smaller SHA1 fingerprint of specific certificate.
>
>   In declaration section of code we provide the name of `host` and the corresponding `fingerprint`.
>

```
const char* host = "api.github.com";
const char* fingerprint = "CF 05 98 89 CA FF 8E D8 5E 5C E0 C2 E4 F7 E6 C3 C7 50 DD 5C";
```

### Get the Fingerprint

我们可以使用网络浏览器获取特定`host` 的`fingerprint` 。 例如在Chrome上按 *Ctrl+Shift+I*  ，然后转到o *Security > View Certificate > Details > Thumbprint*。 这将显示如下所示的窗口，您可以在其中复制指纹并将其粘贴到草图中。

>   We can obtain the `fingerprint` for specific `host` using a web browser. For instance on Chrome press *Ctrl+Shift+I* and go to *Security > View Certificate > Details > Thumbprint*. This will show a window like below where you can copy the fingerprint and paste it into sketch.

![alt text](https://github.com/esp8266/Arduino/raw/master/doc/esp8266wifi/pictures/client-secure-check-fingerprint.png)

剩下的步骤看起来与非安全客户端示例几乎相同。

>   Remaining steps look almost identical as for the [non-secure client example](client-examples.md).


### Connect to the Server

Instantiate the `WiFiClientSecure` object and establish a connection (please note we need to use specific `httpsPort` for secure connections):

```
WiFiClientSecure client;
Serial.print("connecting to ");
Serial.println(host);
if (!client.connect(host, httpsPort)) {
  Serial.println("connection failed");
  return;
}
```

### Is it THAT Server?

现在验证我们与服务器提供的指纹是否匹配：

>   Now verify if the fingerprint we have matches this one provided by the server:

```
if (client.verify(fingerprint, host)) {
  Serial.println("certificate matches");
} else {
  Serial.println("certificate doesn't match");
}
```

如果此检查失败，则由您决定是否继续进行或中止连接。 另请注意，证书具有特定的有效期。 因此，我们今天检查的证书的指纹一定会在一段时间后无效。

>   If this check fails, it is up to you to decide if to proceed further or abort connection. Also note that certificates have specific validity period. Therefore the fingerprint of certificate we have checked today, will certainly be invalid some time later. 


### GET Response from the Server

在接下来的步骤中，我们应该执行GET命令。 这样做与在非安全客户端示例中讨论的方式类似。

>   In the next steps we should execute GET command. This is done is similar way as discussed in [non-secure client example](client-examples.md).

```
client.print(String("GET ") + url + " HTTP/1.1\r\n" +
             "Host: " + host + "\r\n" +
             "User-Agent: BuildFailureDetectorESP8266\r\n" +
             "Connection: close\r\n\r\n");
```

发送请求后，我们应该等待回复，然后处理收到的信息。

在接收到的重播中，我们可以跳过响应头。 这可以通过读取直到标记标题结尾的空行“\ r”来完成：

>   After sending the request we should wait for a reply and then process received information. 
>
>   Out of received replay we can skip response header. This can be done by reading until an empty line `"\r"` that marks the end of the header:
>

```
while (client.connected()) {
  String line = client.readStringUntil('\n');
  if (line == "\r") {
    Serial.println("headers received");
    break;
  }
}
```


### Read and Check the Response

最后我们应该读取服务器提供的JSON，并检查它是否包含{“state”：“success”：

>   Finally we should read JSON provided by server and check if it contains `{"state": "success"`:

```
String line = client.readStringUntil('\n');
if (line.startsWith("{\"state\":\"success\"")) {
  Serial.println("esp8266/Arduino CI successfull!");
} else {
  Serial.println("esp8266/Arduino CI has failed");
}
```


### Does it Work?

现在一旦你知道它应该如何工作，得到草图。 更新您的Wi-Fi网络的凭据。 检查api.github.com的当前指纹，并根据需要进行更新。 然后上传草图并打开一个串行监视器。

如果一切都很好（包括esp8266 / Arduino的构建状态），你应该看到如下消息：

>   Now once you know how it should work, get the [sketch](https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/examples/HTTPSRequest/HTTPSRequest.ino). Update credentials to your Wi-Fi network. Check the current fingerprint of `api.github.com` and update it if required. Then upload sketch and open a serial monitor. 
>
>   If everything is fine (including build status of esp8266 / Arduino) you should see message as below:

```
connecting to sensor-net
........
WiFi connected
IP address: 
192.168.1.104
connecting to api.github.com
certificate matches
requesting URL: /repos/esp8266/Arduino/commits/master/status
request sent
headers received
esp8266/Arduino CI successfull!
reply was:
==========
{"state":"success","statuses":[{"url":"https://api.github.com/repos/esp8266/Arduino/statuses/8cd331a8bae04a6f1443ff0c93539af4720d8ddf","id":677326372,"state":"success","description":"The Travis CI build passed","target_url":"https://travis-ci.org/esp8266/Arduino/builds/148827821","context":"continuous-integration/travis-ci/push","created_at":"2016-08-01T09:54:38Z","updated_at":"2016-08-01T09:54:38Z"},{"url":"https://api.github.com/repos/esp8266/Arduino/statuses/8cd331a8bae04a6f1443ff0c93539af4720d8ddf","id":677333081,"state":"success","description":"27.62% (+0.00%) compared to 0718188","target_url":"https://codecov.io/gh/esp8266/Arduino/commit/8cd331a8bae04a6f1443ff0c93539af4720d8ddf","context":"codecov/project","created_at":"2016-08-01T09:59:05Z","updated_at":"2016-08-01T09:59:05Z"},

(...)

==========
closing connection
```


### Conclusion

编程安全客户端与编程非安全客户端几乎相同。 差异可以降低到一个额外的步骤来验证服务器的身份。 请记住，由于内存使用量大而导致的限制，这取决于服务器使用的密钥的强度以及服务器是否愿意协商TLS缓冲区大小。

有关为安全客户端管理提供的功能列表，请参阅客户端安全类：arrow_right：文档。

>   Programming a secure client is almost identical as programming a non-secure client. The difference gets down to one extra step to verify server's identity. Keep in mind limitations due to heavy memory usage that depends on the strength of the key used by the server and whether server is willing to negotiate the [TLS buffer size](https://www.igvita.com/2013/10/24/optimizing-tls-record-size-and-buffering-latency/).
>
>
>   For the list of functions provided to manage secure clients, please refer to the [Client Secure Class :arrow_right:](client-secure-class.md) documentation.