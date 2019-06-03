# WiFi library

Arduino IDE 1.0.5 中的 WiFi sheield 固件已更改。建议您按照这些说明安装此更新

使用 Arduino WiFi Shield，该库允许 Arduino board 连接到互联网。它可以作为接受传入连接的服务器，也可以作为提供输出的客户端。该库支持WEP和WPA2个人加密，但不支持 WPA2 企业级加密。注意，如果 SSID 没有广播，则 shield 无法连接。

Arduino 使用 SPI 总线与 WiFi 屏蔽进行通信。这是在Uno上的数字引脚11,12和13以及Mega上的引脚 50,51和52上。在两个板上，引脚10用作SS。Mega上，硬件SS引脚53，不使用，但必须保存为输出或SPI接口不工作。数字引脚7用作Wifi shield 和Arduino之间的握手针，不应使用。

WiFi 库非常类似于 [Ethernet](https://www.arduino.cc/en/Reference/Ethernet) library，许多函数调用是相同的

有关WiFi shield 的其他信息，请参阅e [Getting Started page](https://www.arduino.cc/en/Guide/ArduinoWiFiShield) 和 [WiFi shield hardware page](https://www.arduino.cc/en/Main/ArduinoWiFiShield) 。

>   *The firmware for the WiFi shield has changed in Arduino IDE 1.0.5. You are recommended to install this update per these instructions* https://www.arduino.cc/en/Hacking/WiFiShieldFirmwareUpgrading
>
>   With the Arduino WiFi Shield, this library allows an Arduino board to connect to the internet. It can serve as either a server accepting incoming connections or a client making outgoing ones. The library supports WEP and WPA2 Personal encryption, but not WPA2 Enterprise. Also note, if the SSID is not broadcast, the shield cannot connect.
>
>   Arduino communicates with the WiFi shield using the SPI bus. This is on digital pins 11, 12, and 13 on the Uno and pins 50, 51, and 52 on the Mega. On both boards, pin 10 is used as SS. On the Mega, the hardware SS pin, 53, is not used but it must be kept as an output or the SPI interface won't work. Digital pin 7 is used as a handshake pin between the Wifi shield and the Arduino, and should not be used.
>
>   The WiFi library is very similar to the [Ethernet](https://www.arduino.cc/en/Reference/Ethernet) library, and many of the function calls are the same.
>
>   For additional information on the WiFi shield, see the [Getting Started page](https://www.arduino.cc/en/Guide/ArduinoWiFiShield) and the [WiFi shield hardware page](https://www.arduino.cc/en/Main/ArduinoWiFiShield).



## Examples

-   [ConnectNoEncryption](https://www.arduino.cc/en/Tutorial/ConnectNoEncryption) : Demonstrates how to connect to an open network
-   [ConnectWithWEP](https://www.arduino.cc/en/Tutorial/ConnectWithWEP) : Demonstrates how to connect to a network that is encrypted with WEP
-   [ConnectWithWPA](https://www.arduino.cc/en/Tutorial/ConnectWithWPA) : Demonstrates how to connect to a network that is encrypted with WPA2 Personal
-   [ScanNetworks](https://www.arduino.cc/en/Tutorial/ScanNetworks) : Displays all WiFi networks in range
-   [WiFiChatServer](https://www.arduino.cc/en/Tutorial/WiFiChatServer) : Set up a simple chat server
-   [WiFiWebClient](https://www.arduino.cc/en/Tutorial/WiFiWebClient) : Connect to a remote webserver
-   [WiFiWebClientRepeating](https://www.arduino.cc/en/Tutorial/WiFiWebClientRepeating) : Make repeated HTTP calls to a webserver
-   [WiFiWebServer](https://www.arduino.cc/en/Tutorial/WiFiWebServer) : Serve a webpage from the WiFi shield
-   [WiFiSendReceiveUDPString](https://www.arduino.cc/en/Tutorial/WiFiSendReceiveUDPString) : Send and receive a UDP string
-   [UdpNTPClient](https://www.arduino.cc/en/Tutorial/UdpNTPClient) : Query a Network Time Protocol (NTP) server using UDP



## WiFi class

WiFi 类初始化以太网库和网络设置。

>   The WiFi class initializes the ethernet library and network settings.

### WiFi.begin()

Description

初始化 WiFi 库的网络设置并提供当前状态。

Initializes the WiFi library's network settings and provides the current status.

**Syntax**

WiFi.begin(); 
WiFi.begin(ssid); 
WiFi.begin(ssid, pass); 
WiFi.begin(ssid, keyIndex, key); 

**Parameters**

**ssid**: the SSID (Service Set Identifier) is the name of the WiFi network you want to connect to.

**keyIndex**: WEP encrypted networks can hold up to 4 different keys. This identifies which key you are going to use.
WEP加密网络最多可容纳4个不同的密钥。 这将确定您要使用的密钥。

**key**: a hexadecimal string used as a security code for WEP encrypted networks.
用作 WEP 加密网络的安全代码的十六进制字符串。

**pass**: WPA encrypted networks use a password in the form of a string for security.
WPA加密网络以字符串的形式使用密码进行安全保护。

**Returns**

-   WL_CONNECTED when connected to a network
-   WL_IDLE_STATUS when not connected to a network, but powered on 但电源已打开

**Example**

```c
#include <WiFi.h>

//SSID of your network 
char ssid[] = "yourNetwork";
//password of your WPA Network 
char pass[] = "secretPassword";

void setup()
{
 WiFi.begin(ssid, pass);
}

void loop () {}
```

### WiFi.disconnect()

**Description**

Disconnects the WiFi shield from the current network.

**Syntax**

WiFi.disconnect(); 

**Parameters**

none

### WiFi.config()

**Description**

`WiFi.config()` allows you to configure a static IP address as well as change the DNS, gateway, and subnet addresses on the WiFi shield.

Unlike `WiFi.begin()` which automatically configures the WiFi shield to use DHCP, `WiFi.config()` allows you to manually 手动 set the network address of the shield.

Calling `WiFi.config()` before `WiFi.begin()` forces强迫 `begin()` to configure the WiFi shield with the network addresses specified in `config()`.

You can call `WiFi.config()` after `WiFi.begin()`, but the shield will initialize with `begin()` in the default DHCP mode. Once the `config()` method is called, it will change the network address as requested.

**Syntax**

WiFi.config(ip); 
WiFi.config(ip, dns); 
WiFi.config(ip, dns, gateway); 
WiFi.config(ip, dns, gateway, subnet); 

**Parameters**

**ip**: the IP address of the device (array of 4 bytes)

**dns**: the address for a DNS server.

**gateway**: the IP address of the network gateway (array of 4 bytes). optional: defaults to the device IP address with the last octet(八位字节) set to 1

**subnet**: the subnet mask of the network (array of 4 bytes). optional: defaults to 255.255.255.0

**Returns**

Nothing

**Example**

This example shows how to set the static IP address, 192.168.0.177, of the LAN network to the WiFi shield:

```c
#include <SPI.h>
#include <WiFi.h>

// the IP address for the shield:
IPAddress ip(192, 168, 0, 177);    

char ssid[] = "yourNetwork";    // your network SSID (name) 
char pass[] = "secretPassword"; // your network password (use for WPA, or use as key for WEP)

int status = WL_IDLE_STATUS;

void setup()
{  
  // Initialize serial and wait for port to open:
  Serial.begin(9600); 
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }

  // check for the presence存在 of the shield:
  if (WiFi.status() == WL_NO_SHIELD) {
    Serial.println("WiFi shield not present"); 
    while(true);  // don't continue
  } 

  WiFi.config(ip);

  // attempt试图 to connect to Wifi network:
  while ( status != WL_CONNECTED) { 
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    // Connect to WPA/WPA2 network. Change this line if using open or WEP network:    
    status = WiFi.begin(ssid, pass);

    // wait 10 seconds for connection:
    delay(10000);
  }

  // print your WiFi shield's IP address:
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP()); 
}

void loop () {}
```



### WiFi.setDNS()

**Description**

`WiFi.setDNS()` allows you to configure the DNS (Domain Name System) server.

**Syntax**

WiFi.setDNS(dns_server1)
WiFi.setDNS(dns_server1, dns_server2)

**Parameters**

**dns_server1**: the IP address of the primary DNS server
**dns_server2**: the IP address of the secondary DNS server

**Returns**

Nothing

**Example**

This example shows how to set the Google DNS (8.8.8.8). **You can set it as an object IPAddress.**

```
#include <SPI.h>
#include <WiFi.h>

// the IP address for the shield:
IPAddress dns(8, 8, 8, 8);  //Google dns  

char ssid[] = "yourNetwork";    // your network SSID (name) 
char pass[] = "secretPassword"; // your network password (use for WPA, or use as key for WEP)

int status = WL_IDLE_STATUS;

void setup()
{  
  // Initialize serial and wait for port to open:
  Serial.begin(9600); 
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }

  // check for the presence of the shield:
  if (WiFi.status() == WL_NO_SHIELD) {
    Serial.println("WiFi shield not present"); 
    while(true);  // don't continue
  } 

  // attempt to connect to Wifi network:
  while ( status != WL_CONNECTED) { 
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    // Connect to WPA/WPA2 network. Change this line if using open or WEP network:    
    status = WiFi.begin(ssid, pass);

    // wait 10 seconds for connection:
    delay(10000);
  }

  // print your WiFi shield's IP address:
  WiFi.setDNS(dns);
  Serial.print("Dns configured.");
}

void loop () {
}
```



### WiFi.SSID()

**Description**

Gets the SSID of the current network

**Syntax**

WiFi.SSID(); 
WiFi.SSID(wifiAccessPoint)

**Parameters**

wifiAccessPoint: specifies from which network to get the information 从指定的网络获取信息

**Returns**

A string containing the SSID the WiFi shield is currently connected to.

string containing name of network requested.

**Example**

```
#include <SPI.h>
#include <WiFi.h>

//SSID of your network 
char ssid[] = "yourNetwork";
int status = WL_IDLE_STATUS;     // the Wifi radio's status

void setup()
{
  // initialize serial:
  Serial.begin(9600);

  // scan for existing networks:
  Serial.println("Scanning available networks...");
  scanNetworks();

  // attempt试图 to connect using WEP encryption:
  Serial.println("Attempting to connect to open network...");
  status = WiFi.begin(ssid);

  Serial.print("SSID: ");
  Serial.println(ssid);

}

void loop () {}

void scanNetworks() {
  // scan for nearby networks:
  Serial.println("** Scan Networks **");
  byte numSsid = WiFi.scanNetworks();

  // print the list of networks seen:
  Serial.print("SSID List:");
  Serial.println(numSsid);
  // print the network number and name for each network found:
  for (int thisNet = 0; thisNet<numSsid; thisNet++) {
    Serial.print(thisNet);
    Serial.print(") Network: ");
    Serial.println(WiFi.SSID(thisNet));
  }
}
```



### WiFi.BSSID()

**Description**

Gets the MAC address of the routher you are connected to

**Syntax**

WiFi.BSSID(bssid); 

**Parameters**

bssid : 6 byte array

**Returns**

A byte array containing the MAC address of the router the WiFi shield is currently connected to.

**Example**

```
#include <WiFi.h>

//SSID of your network 
char ssid[] = "yourNetwork";
//password of your WPA Network 
char pass[] = "secretPassword";

void setup()
{
 WiFi.begin(ssid, pass);

  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print out info about the connection:
  else {
  // print the MAC address of the router you're attached to:
  byte bssid[6];
  WiFi.BSSID(bssid);    
  Serial.print("BSSID: ");
  Serial.print(bssid[5],HEX);
  Serial.print(":");
  Serial.print(bssid[4],HEX);
  Serial.print(":");
  Serial.print(bssid[3],HEX);
  Serial.print(":");
  Serial.print(bssid[2],HEX);
  Serial.print(":");
  Serial.print(bssid[1],HEX);
  Serial.print(":");
  Serial.println(bssid[0],HEX);
  }
}

void loop () {}
```

### WiFi.RSSI()

**Description**

Gets the signal strength of the connection to the router

**Syntax**

WiFi.RSSI(); 
WiFi.RSSI(wifiAccessPoint);

**Parameters**

wifiAccessPoint: specifies from which network to get the information

**Returns**

long : the current RSSI /Received Signal Strength in dBm

**Example**

```
#include <SPI.h>
#include <WiFi.h>

//SSID of your network 
char ssid[] = "yourNetwork";
//password of your WPA Network 
char pass[] = "secretPassword";

void setup()
{
 WiFi.begin(ssid, pass);

  if (WiFi.status() != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print out info about the connection:
  else {
   // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("RSSI:");
  Serial.println(rssi);
  }
}

void loop () {}
```



### WiFi.encryptionType()

**Description**

Gets the encryption type of the current network

**Syntax**

WiFi.encryptionType(); 
WiFi.encryptionType(wifiAccessPoint); 

**Parameters**

wifiAccessPoint: specifies which network to get information from

**Returns**

byte : value represents 代表 the type of encryption

-   TKIP (WPA) = 2
-   WEP = 5
-   CCMP (WPA) = 4
-   NONE = 7
-   AUTO = 8

**Example**

```
#include <SPI.h>
#include <WiFi.h>

//SSID of your network 
char ssid[] = "yourNetwork";
//password of your WPA Network 
char pass[] = "secretPassword";

void setup()
{
 WiFi.begin(ssid, pass);

  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print out info about the connection:
  else {
   // print the encryption type:
  byte encryption = WiFi.encryptionType();
  Serial.print("Encryption Type:");
  Serial.println(encryption,HEX);
  }
}

void loop () {}
```

### WiFi.scanNetworks()

**Description**

Scans for available WiFi networks and returns the discovered number

**Syntax**

WiFi.scanNetworks(); 

**Parameters**

none

**Returns**

byte : number of discovered networks

**Example**

```
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "yourNetwork";     // the name of your network
int status = WL_IDLE_STATUS;     // the Wifi radio's status

byte mac[6];                     // the MAC address of your Wifi shield


void setup()
{
 Serial.begin(9600);

 status = WiFi.begin(ssid);

 if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print your MAC address:
  else {

  Serial.println("** Scan Networks **");
  byte numSsid = WiFi.scanNetworks();

  Serial.print("SSID List:");
  Serial.println(numSsid);
  }
}

void loop () {}
```

### WiFi.status()

**Description**

Return the connection status.

**Syntax**

WiFi.status();

**Parameters**

none

**Returns**

-   WL_CONNECTED: assigned when connected to a WiFi network;
-   WL_NO_SHIELD: assigned when no WiFi shield is present;
-   WL_IDLE_STATUS: it is a temporary status assigned when **WiFi.begin()** is called and remains active until the number of attempts expires (resulting in WL_CONNECT_FAILED) or a connection is established (resulting in WL_CONNECTED);
    当 **WiFi.begin()** 被调用时，会出现这样一个临时状态，并保持活动，直到尝试次数过期（导致 WL_CONNECT_FAILED）或连接建立（导致 WL_CONNECTED）;
-   WL_NO_SSID_AVAIL: assigned when no SSID are available;
-   WL_SCAN_COMPLETED: assigned when the scan networks is completed;
-   WL_CONNECT_FAILED: assigned when the connection fails for all the attempts;
-   WL_CONNECTION_LOST: assigned when the connection is lost;
-   WL_DISCONNECTED: assigned when disconnected from a network;

**Example**

```
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "yourNetwork";                     // your network SSID (name)
char key[] = "D0D0DEADF00DABBADEAFBEADED";       // your network key
int keyIndex = 0;                                // your network key Index number
int status = WL_IDLE_STATUS;                     // the Wifi radio's status

void setup() {
  //Initialize serial and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }

  // check for the presence of the shield:
  if (WiFi.status() == WL_NO_SHIELD) {
    Serial.println("WiFi shield not present");
    // don't continue:
    while (true);
  }

  // attempt to connect to Wifi network:
  while ( status != WL_CONNECTED) {
    Serial.print("Attempting to connect to WEP network, SSID: ");
    Serial.println(ssid);
    status = WiFi.begin(ssid, keyIndex, key);

    // wait 10 seconds for connection:
    delay(10000);
  }

  // once you are connected :
  Serial.print("You're connected to the network");
}

void loop() {
  // check the network status connection once every 10 seconds:
  delay(10000);
 Serial.println(WiFi.status();
}
```

### WiFi.getSocket()

**Description**

gets the first socket available

**Syntax**

WiFi.getSocket(); 

**Parameters**

none

**Returns**

int : the first socket available



### WiFi.macAddress()

**Description**

Gets the MAC Address of your WiFi shield

**Syntax**

WiFi.macAddress(mac); 

**Parameters**

mac: a 6 byte array to hold the MAC address

**Returns**

byte array : 6 bytes representing the MAC address of your shield

**Example**

```
#include <SPI.h>
#include <WiFi.h>

char ssid[] = "yourNetwork";     // the name of your network
int status = WL_IDLE_STATUS;     // the Wifi radio's status

byte mac[6];                     // the MAC address of your Wifi shield


void setup()
{
 Serial.begin(9600);

 status = WiFi.begin(ssid);

 if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print your MAC address:
  else {
  WiFi.macAddress(mac);
  Serial.print("MAC: ");
  Serial.print(mac[5],HEX);
  Serial.print(":");
  Serial.print(mac[4],HEX);
  Serial.print(":");
  Serial.print(mac[3],HEX);
  Serial.print(":");
  Serial.print(mac[2],HEX);
  Serial.print(":");
  Serial.print(mac[1],HEX);
  Serial.print(":");
  Serial.println(mac[0],HEX);
  }
}

void loop () {}
```

## IPAddress class

The IPAddress class provides information about the network configuration.

### WiFi.localIP()

**Description**

Gets the WiFi shield's IP address

**Syntax**

WiFi.localIP(); 

**Parameters**

none

**Returns**

the IP address of the shield

**Example**

```
#include <WiFi.h>

char ssid[] = "yourNetwork";      //SSID of your network

int status = WL_IDLE_STATUS;     // the Wifi radio's status

IPAddress ip;                    // the IP address of your shield

void setup()
{
 // initialize serial:
 Serial.begin(9600);

 WiFi.begin(ssid);

  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print out info about the connection:
  else {
 //print the local IP address
  ip = WiFi.localIP();
  Serial.println(ip);

  }
}

void loop () {}
```

### WiFi.subnetMask()

**Description**

Gets the WiFi shield's subnet mask

**Syntax**

WiFi.subnet(); 

**Parameters**

none

**Returns**

the subnet mask of the shield

**Example**

```
#include <WiFi.h>
int status = WL_IDLE_STATUS;     // the Wifi radio's status

//SSID of your network 
char ssid[] = "yourNetwork";
//password of your WPA Network 
char pass[] = "secretPassword";

IPAddress ip;
IPAddress subnet;
IPAddress gateway;

void setup()
{
  WiFi.begin(ssid, pass);

  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print out info about the connection:
  else {

    // print your subnet mask:
    subnet = WiFi.subnetMask();
    Serial.print("NETMASK: ");
    Serial.println(subnet);

  }
}

void loop () {
}
```

### WiFi.gatewayIP()

**Description**

Gets the WiFi shield's gateway IP address.

**Syntax**

WiFi.gatewayIP(); 

**Parameters**

none

**Returns**

An array containing the shield's gateway IP address

**Example**

```
include <SPI.h>
include <WiFi.h>
int status = WL_IDLE_STATUS; // the Wifi radio's status

//SSID of your network 
char ssid[] = "yourNetwork"; 
//password of your WPA Network 
char pass[] = "secretPassword";

IPAddress gateway;

void setup() {

  Serial.begin(9600);
  WiFi.begin(ssid, pass);
  if ( status != WL_CONNECTED) { 
    Serial.println("Couldn't get a wifi connection");
    while(true);
  } 
  // if you are connected, print out info about the connection:
  else {
  // print your gateway address:
  gateway = WiFi.gatewayIP();
  Serial.print("GATEWAY: ");
  Serial.println(gateway);
  }
}

void loop () {}
```



## Server class

The Server class creates servers which can send data to and receive data from connected clients (programs running on other computers or devices).

文档内容在 Arduino_doc_esp8266wifi_server-class.md 笔记中

[Arduino_doc_esp8266wifi_server-class.md](https://app.yinxiang.com/shard/s10/nl/119459/e00208ee-d4c2-4e36-8b4c-303fd7b931ba)



## Client class

The client class creates clients that can connect to servers and send and receive data.

文档内容  [Arduino_doc_esp8266wifi_client-class.md](https://app.yinxiang.com/shard/s10/nl/119459/ccba4b6e-6a06-44b7-b102-2454b9677fcf)  笔记中。



## UDP class

The UDP class enables UDP message to be sent and received.

文档内容在 [Arduino_doc_esp8266wifi_udp-class.md](evernote:///view/119459/s10/a41fd93c-2bc6-4902-b9c6-bf62d6bc3897/a41fd93c-2bc6-4902-b9c6-bf62d6bc3897/)  笔记中。









