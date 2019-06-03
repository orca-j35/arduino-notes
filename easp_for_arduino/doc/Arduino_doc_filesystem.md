---
title: File System
---

[Arduino](https://github.com/esp8266/Arduino)/[doc](https://github.com/esp8266/Arduino/tree/master/doc)/**filesystem.md**

[TOC]

## Flash layout 闪存布局

尽管文件系统和程序被存储在相同的芯片上，但是编程新的 sketch 并不会修改文件系统的内容。允许使用文件系统存储 sketch 数据，配置文件或 Web 服务器内容。

>   Even though file system is stored on the same flash chip as the program, programming new sketch will not modify file system contents. This allows to use file system to store sketch data, configuration files, or content for Web server.

下图说明了在 Arduino 环境中使用的闪存布局：

>   The following diagram illustrates flash layout used in Arduino environment:
>

    |--------------|-------|---------------|--|--|--|--|--|
    ^              ^       ^               ^     ^
    Sketch    OTA update   File system   EEPROM  WiFi config (SDK)

文件系统大小取决于闪存芯片大小。 根据在 IDE 中选择的板卡，您有以下闪存大小选项：

>   File system size depends on the flash chip size. Depending on the board which is selected in IDE, you have the following options for flash size:

| Board                         | Flash chip size, bytes | File system size, bytes |
| ----------------------------- | ---------------------- | ----------------------- |
| Generic module                | 512k                   | 64k, 128k               |
| Generic module                | 1M                     | 64k, 128k, 256k, 512k   |
| Generic module                | 2M                     | 1M                      |
| Generic module                | 4M                     | 3M                      |
| Adafruit HUZZAH               | 4M                     | 1M, 3M                  |
| ESPresso Lite 1.0             | 4M                     | 1M, 3M                  |
| ESPresso Lite 2.0             | 4M                     | 1M, 3M                  |
| NodeMCU 0.9                   | 4M                     | 1M, 3M                  |
| NodeMCU 1.0                   | 4M                     | 1M, 3M                  |
| Olimex MOD-WIFI-ESP8266(-DEV) | 2M                     | 1M                      |
| SparkFun Thing                | 512k                   | 64k                     |
| SweetPea ESP-210              | 4M                     | 1M, 3M                  |
| WeMos D1 & D1 mini            | 4M                     | 1M, 3M                  |
| ESPDuino                      | 4M                     | 1M, 3M                  |

注意：要在草图中使用任何文件系统函数，请将以 下include 添加到草图中：

>   **Note:** to use any of file system functions in the sketch, add the following include to the sketch:

```
#include "FS.h"
```
## File system limitations 文件系统的限制

ESP8266 的文件系统实现不得不适应芯片的限制，其中有限的 RAM。选择[SPIFFS](https://github.com/pellepl/spiffs) 是因为它是为小型系统设计的，但这是以一些简化和限制为代价的。

>   The filesystem implementation for ESP8266 had to accomodate the constraints of the chip, among which its limited RAM. [SPIFFS](https://github.com/pellepl/spiffs) was selected because it is designed for small systems, but that comes at the cost of some simplifications and limitations.

首先，在后台，SPIFFS 不支持目录，它只是存储一个“平”的文件列表。
但是与传统文件系统相反，文件名中允许使用斜杠字符 `'/'` ，因此处理目录列表的函数(e.g. `openDir("/website")`) 基本上只是过滤文件名，并保留起初请求的前缀 (`/website/`)。 实际上，这并没有什么区别。

第二点，文件名总共有 32 个字符的限制。 `'\0'` 字符被保留位 C 字符串的终止字符，因此我们有 31 个可用字符。

结合上面两点，这意味着建议保持简短的文件名，并不要使用深层次的嵌套目录。因为每个文件的完整路径最多只有 31 个字符（包括目录， `'/'` 字符，基本名称，点和扩展名）。
例如，文件名 `/website/images/bird_thumbnail.jpg` 是34个字符，如果使用这样的名称，将导致一些问题，例如在 `exists()` 中，或者如果另一个文件以相同的前31个字符开始。

警告：该限制很容易达到，如果被忽略，问题可能会被忽视，因为在编译或运行时不会出现错误信息。

有关SPIFFS实现的内部的更多详细信息，请参阅 [SPIFFS readme file](https://github.com/esp8266/Arduino/blob/master/cores/esp8266/spiffs/README.md)。

>   First, behind the scenes, SPIFFS does not support directories, it just stores a "flat" list of files. 
>   But contrary to traditional filesystems, the slash character `'/'` is allowed in filenames, so the functions that deal with directory listing (e.g. `openDir("/website")`) basically just filter the filenames and keep the ones that start with the requested prefix (`/website/`).
>   Practically speaking, that makes little difference though.
>
>   Second, there is a limit of 32 chars in total for filenames. One `'\0'` char is reserved for C string termination, so that leaves us with 31 usable characters.
>
>   Combined, that means it is advised to keep filenames short and not use deeply nested directories, as the full path of each file (including directories, `'/'` characters, base name, dot and extension) has to be 31 chars at a maximum. 
>   For example, the filename `/website/images/bird_thumbnail.jpg` is 34 chars and will cause some problems if used, for example in `exists()` or in case another file starts with the same first 31 characters.
>
>   **Warning**: That limit is easily reached and if ignored, problems might go unnoticed because no error message will appear at compilation nor runtime.
>
>   For more details on the internals of SPIFFS implementation, see the [SPIFFS readme file](https://github.com/esp8266/Arduino/blob/master/cores/esp8266/spiffs/README.md).
>

## Uploading files to file system

ESP8266FS 是一个集成到 Arduino IDE 中的工具。 它将一个菜单项添加到“工具“菜单，用于将 sketch 数据目录的内容上传到 ESP8266 闪存文件系统中。

- 下载工具：https://github.com/esp8266/arduino-esp8266fs-plugin/releases/download/0.3.0/ESP8266FS-0.3.0.zip.
- 在您的Arduino sketchbook 目录中，如果尚不存在 `tools` 目录，则创建 `tools` 目录
- 将工具解压到 `tools` 目录 (the path will look like `<home_dir>/Arduino/tools/ESP8266FS/tool/esp8266fs.jar`)
- 重启 Arduino IDE
- 打开一个 sketch ( 或者创建一个新的 sketch ，并保存它)
- 跳转到 sketch 目录( 选择  Sketch > Show Sketch Folder )
- 在文件系统中创建一个名为 `data` 的目录和任何你想要的文件
- 确保您以选择好开发板、端口，并关闭串行监视器
- 选择 Tools > ESP8266 Sketch Data Upload。这将开始上传文件到 ESP8266 闪存文件系统。完成后，IDE 状态栏将显示  `SPIFFS Image Uploaded` 的消息。

>   *ESP8266FS* is a tool which integrates into the Arduino IDE. It adds a menu item to *Tools* menu for uploading the contents of sketch data directory into ESP8266 flash file system.
>
>   - Download the tool: https://github.com/esp8266/arduino-esp8266fs-plugin/releases/download/0.3.0/ESP8266FS-0.3.0.zip.
>   - In your Arduino sketchbook directory, create `tools` directory if it doesn't exist yet
>   - Unpack the tool into `tools` directory (the path will look like `<home_dir>/Arduino/tools/ESP8266FS/tool/esp8266fs.jar`)
>   - Restart Arduino IDE
>   - Open a sketch (or create a new one and save it)
>   - Go to sketch directory (choose Sketch > Show Sketch Folder)
>   - Create a directory named `data` and any files you want in the file system there
>   - Make sure you have selected a board, port, and closed Serial Monitor
>   - Select Tools > ESP8266 Sketch Data Upload. This should start uploading the files into ESP8266 flash file system. When done, IDE status bar will display `SPIFFS Image Uploaded` message.
>
>

## File system object (SPIFFS) 文件系统对象

### begin

```
SPIFFS.begin()
```

此方法调用 SPIFFS 文件系统。它必须在任何 FS APIs 被使用之前进行调用。如果文件系统被成功安装，会返回 `true` ，否则返回 `false` 。

>   This method mounts SPIFFS file system. It must be called before any other
>   FS APIs are used. Returns *true* if file system was mounted successfully, false
>   otherwise.

### end( ) 卸载文件系统

```
SPIFFS.end()
```

此方法卸载 SPIFFS 文件系统。 在使用 OTA 更新 SPIFFS 之前使用此方法。

>   This method unmounts SPIFFS file system. Use this method before updating SPIFFS using OTA.

### format( ) 格式化文件系统

```
SPIFFS.format()
```

格式化文件系统。 可以在调用 `begin` 之前或之后调用。

如果格式化成功，则返回 true。

>   Formats the file system. May be called either before or after calling `begin`.
>   Returns *true* if formatting was successful.

### open( )

```
SPIFFS.open(path, mode)
```

打开一个文件。`path` 应为以斜杠开头的绝对路径( 例如，`/dir/filename.txt` )。`mode` 是提供访问模式的字符串。它可以是  "r", "w", "a", "r+", "w+", "a+" 中的一个。这些模式的含义与 C 语言中 `fopen` 函数有相同的含义。

>   Opens a file. `path` should be an absolute path starting with a slash
>   (e.g. `/dir/filename.txt`). `mode` is a string specifying access mode. It can be
>   one of "r", "w", "a", "r+", "w+", "a+". Meaning of these modes is the same as
>   ​for `fopen` C function.      

       r      Open text file for reading.  The stream is positioned at the
              beginning of the file.打开文本文件用于读取。流位于文件的开头。
    
       r+     Open for reading and writing.  The stream is positioned at the
              beginning of the file.打开文件用于读写。流位于文件的开头。
    
       w      Truncate file to zero length or create text file for writing.
              The stream is positioned at the beginning of the file.
              将文件删减到零长度，或创建用于写入的文本文件。流位于文件的开头。
    
       w+     Open for reading and writing.  The file is created if it does
              not exist, otherwise it is truncated.  The stream is
              positioned at the beginning of the file.
              打开文件用于读写。如果文件不存在的话，会创建该文件，否则删减其长度至零。
              流位于文件的开头。
    
       a      Open for appending (writing at end of file).  The file is
              created if it does not exist.  The stream is positioned at the
              end of the file.
              打开文件并追加内容( 在文件末尾写入)。如果要追加的文件不存在，则创建此文件。
              流位于文件的结尾。
    
       a+     Open for reading and appending (writing at end of file).  The
              file is created if it does not exist.  The initial file
              position for reading is at the beginning of the file, but
              output is always appended to the end of the file.
              打开文件用于读取和追加（在文件末尾写入）。如果此文件不存在，则会创建此文件。
              用于读取的初始文件位置在文件的开头，但输出始终附加到文件的结尾。

返回 *File* 对象。要检测文件是否被成功打开，请使用布尔运算。

>   Returns *File* object. To check whether the file was opened successfully, use the boolean operator.

```
File f = SPIFFS.open("/f.txt", "w");
if (!f) {
    Serial.println("file open failed");
}
```

### exists( ) 检测文件是否存在

```
SPIFFS.exists(path)
```

如果给定路径的文件存在，会返回 *true* ，否则返回 *false*

>   Returns *true* if a file with given path exists, *false* otherwise.

### openDir( ) 打开目录

```
SPIFFS.openDir(path)
```

打开一个给定其绝对路径的目录。 返回一个 Dir 对象。

>   Opens a directory given its absolute path. Returns a *Dir* object.

### remove( ) 删除

```
SPIFFS.remove(path)
```

删除给定其绝对路劲的文件。如果文件已成功删除，则返回 *true*

>   Deletes the file given its absolute path. Returns *true* if file was deleted successfully.

### rename( ) 重命名

```
SPIFFS.rename(pathFrom, pathTo)
```

将文件从 `pathFrom` 重命名为 `pathTo` 。路径必须是绝对路径。如果文件已成功重命名，则返回  true。

>   Renames file from `pathFrom` to `pathTo`. Paths must be absolute. Returns *true*
>   if file was renamed successfully.

### info( )  信息

```
FSInfo fs_info;
SPIFFS.info(fs_info);
```

使用有关文件系统的信息填充 [FSInfo structure](#filesystem-information-structure) 。 返回true成功，否则返回false。

>   Fills [FSInfo structure](#filesystem-information-structure) with information about the file system. Returns `true` is successful, `false` otherwise.

## Filesystem information structure

文件系统的信息结构

```
struct FSInfo {
    size_t totalBytes;
    size_t usedBytes;
    size_t blockSize;
    size_t pageSize;
    size_t maxOpenFiles;
    size_t maxPathLength;
};
```

可以使用 FS::info 方法填充的结构。

-   `totalBytes` — 文件系统上有用数据的总大小
-   `usedBytes` — 文件使用的字节数
-   `blockSize` — SPIFFS 块大小
-   `pageSize` — SPIFFS 逻辑页大小
-   `maxOpenFiles` — 可以同时打开的最大文件数
-   `maxPathLength` — 最大文件名长度（包括零终止的一个字节）

>   This is the structure which may be filled using FS::info method.
>
>   - `totalBytes` — total size of useful data on the file system
>   - `usedBytes` — number of bytes used by files
>   - `blockSize` — SPIFFS block size
>   - `pageSize` — SPIFFS logical page size
>   - `maxOpenFiles` — max number of files which may be open simultaneously
>   - `maxPathLength` — max file name length (including one byte for zero termination)
>
>

## Directory object (Dir)

目录对象(Dir)

Dir 对象的用途是迭代目录中的文件。
它提供三个方法: `next()`, `fileName()`, and `openFile(mode)`。

以下示例说明如何使用它：

>   The purpose of *Dir* object is to iterate over files inside a directory.
>   It provides three methods: `next()`, `fileName()`, and `openFile(mode)`.
>
>   The following example shows how it should be used:
>

```
Dir dir = SPIFFS.openDir("/data");
while (dir.next()) {
    Serial.print(dir.fileName());
    File f = dir.openFile("r");
    Serial.println(f.size());
}
```

当目录中有要迭代的文件是， `dir.next()` 返回 true。该函数必须在调用 `fileName` 和 `openFile` 函数之前调用。

`openFile` 方法使用模式参数，其含义与`SPIFFS.open` 函数相同。

>   `dir.next()` returns true while there are files in the directory to iterate over.
>   It must be called before calling `fileName` and `openFile` functions.
>
>   `openFile` method takes *mode* argument which has the same meaning as for `SPIFFS.open` function.
>

## File object 文件对象

`SPIFFS.open`和 `dir.openFile` 函数返回一个 File 对象。 这个对象支持Stream的所有功能，因此可以使用`readBytes`, `findUntil`,`parseInt`, `println` 和所有其他Stream方法。

还有一些特定于 File对 象的函数。

>   `SPIFFS.open` and `dir.openFile` functions return a *File* object. This object
>   supports all the functions of *Stream*, so you can use `readBytes`, `findUntil`,
>   `parseInt`, `println`, and all other *Stream* methods.
>
>   There are also some functions which are specific to *File* object.
>

### seek

```
file.seek(offset, mode)
```

此函数的行为类似于 `fseek` C 函数。 根据 `mode` 的值，它文件中移动的当前位置如下：

- 如果 `mode`是`SeekSet`，则将位置设置为从开始的 `offset` 字节。
- 如果 `mode`为 `SeekCur` ，则当前位置按 `offset` 字节移动。
- 如果 `mode`为 `SeekEnd`，则位置被设置为从结束的 `offset` 字节文件。

如果位置设置成功，则返回 true。

>   This function behaves like `fseek` C function. Depending on the value of `mode`,
>   it moves current position in a file as follows:
>
>   - if `mode` is `SeekSet`, position is set to `offset` bytes from the beginning.
>   - if `mode` is `SeekCur`, current position is moved by `offset` bytes.
>   - if `mode` is `SeekEnd`, position is set to `offset` bytes from the end of the
>     file.
>
>   Returns *true* if position was set successfully.
>

### position

```
file.position()
```

>   Returns the current position inside the file, in bytes.
>

### size

```
file.size()
```

>   Returns file size, in bytes.
>


### name

```
String name = file.name();
```

返回文件名，如 const char *。 将其转换为字符串进行存储。

>   Returns file name, as `const char*`. Convert it to *String* for storage.

### close

```
file.close()
```

关闭文件。 在调用 close 函数后，不应对 File 对象执行其他操作。

>   Close the file. No other operations should be performed on *File* object after `close` function was called.