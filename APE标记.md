# APE标记

## APE标记概述

APEV2的结构如下

|名称|大小|
|-|-|
|[标签头](#标签头)|`32`B|
|[项目1](#项目)|`n`B|
|[项目2](#项目)|`n`B|
|...|...|
|[项目n](#项目)|`n`B|
|[标签尾](#标签头)|`32`B|

APEV1没有标签头。强烈建议以APEV2取代APEV1。

APE标签项目应按大小升序排列。在流式传输时，可以去掉部分APE标记，以减少标题之间出现脱落的危险。这不是必须的，但强烈建议这样做。实际上，项目应按重要性/字节排序，但这并不可行。只有在添加了不太重要的小项目，又不想重写整个标签时，才可以打破这一规则。位于文件末尾的APEv1标签必须至少有一个标签尾，APEv1标签不能用于文件的开头(与APEv2标签不同)。当位于MP3文件末尾时，APE标记应放在最后一帧之后，ID3v1标记(如果有)之前。

APE标签使用小端序。

## 标签头

|名称|大小|备注|
|-|-|-|
|起始标记|`8`B|`{'A','P','E','T','A','G','E','X'}`|
|版本号|`4`B|`1000` = 版本1.000；`2000` = 版本2.000|
|标签大小|`4`B|因需与APEV1兼容，此字段指示的大小不包含标签头|
|项目数|`4`B||
|[标志](#标志)|`4`B||
|保留|`8`B|必须为`0`|

## 项目

|名称|大小|备注|
|-|-|-|
|值长度|`4`B|值的长度，以字节为单位|
|[标志](#标志)|`4`B||
|[键]()|`n`B|包含ASCII可打印字符|
|键结束符|`1`B|`0x00`|
|[值]()|`m`B||

## 标志

|位|含义|
|-|-|
|31|`0` = 不含标签头，`1` = 含标签头|
|30|`0` = 不含标签尾，`1` = 含标签尾|
|29|在标签头中设置此位|
|28 - 3|未用，必须为`0`|
|2 - 1|`0` = 值为UTF-8编码的字符串，`1` = 值为二进制数据，`2` = 值为外部存储信息的链接(链接也为UTF-8编码)，`3` = 保留|
|0|`0` = 标签可读可写，`1` = 标签只读|

APEV1不使用标志，所有的位必须为`0`。

> 下面是允许的链接形式：
>
> http://host/directory/filename.ext
>
> ftp://host/directory/filename.ext
>
> filename.ext
>
> /directory/filename.ext
>
> DRIVE:/directory/filename.ext

## 键

键的长度为2(包括)至255(包括)个字符，范围从0x20(空格)至0x7E(Tilde)。

典型的键长度应为2-16个字符，使用以下字符：
空格(0x20)、斜线(0x2F)、数字(0x30-0x39)、字母(0x41-0x5A、0x61-0x7A)。

不允许使用这些键名： `ID3`、`TAG`、`OggS`和`MP+`。

当前定义了以下键：

|键名|含义|建议的值格式|
|-|-|-|
|Title|标题|UTF-8字符串|
|Subtitle|副标题|UTF-8字符串|
|Artist|艺术家|UTF-8字符串(或列表)|
|Album|专辑|UTF-8字符串|
|Debut album|首发专辑|UTF-8字符串|
|Publisher|唱片公司或出版商|UTF-8字符串|
|Conductor|指挥者|UTF-8字符串|
|Track|音轨号<br>音轨号/总音轨数|整数<br>整数/整数|
|Composer|作曲、编曲|UTF-8字符串(或列表)|
|Comment|备注|UTF-8字符串(或列表)或链接(或列表)|
|Copyright|版权方|UTF-8字符串(或列表)|
|Publicationright|出版权持有人|UTF-8字符串(或列表)|
|File|文件位置|链接(或列表)|
|EAN/UPC|EAN-13或UPC(欧洲物品编码-13或通用产品代码)条形码|12或13位整数|
|ISBN|带校验码的ISBN(国际统一书号)|9位整数+1位校验码(0-9或X)|
|Catalog|目录编号|有时在EAN或UPC中，常为字母后跟数字|
|LC|标签代码|`"LC"`+4或5位整数|
|Year|发布年份|日期|
|Record Date|录音日期|日期|
|Record Location|录音地点|UTF-8字符串(或列表)|
|Genre|流派|UTF-8字符串(或列表)|
|Media|来源媒体<br>来源媒体编号/总媒体数<br>来源时间|媒体<br>媒体 整数/整数<br>媒体 时间|
|Index|供快速查找用的索引|索引时间|
|Related|相关信息链接|链接(或列表)|
|ISRC|国际标准录音制品编码|ASCII字符串|
|Abstract|摘要|链接|
|Language|语言|UTF-8字符串(或列表)|
|Bibliography|书目或文献|链接|
|Introplay|前奏特色部分|索引时间范围|
|Dummy|占位符|`0x00`字节序列|
|Cover Art (*)|封面|UTF-8编码的字符串描述 + `0x00` + 图片数据。其中*为图片类型，如`"Cover Art (Front)"`或`"Cover Art (Back)"`|
|Performer|表演者/合作/客串||
|REMIXED BY|**(foobar2000)**重混音者||
|MIXARTIST|**(Mp3tag)**重混音者||
|ORIGINAL ARTIST|**(foobar2000)**||
|ORIGARTIST|**(Mp3tag)**||
|Orig. Artist|**(Winamp)**||
||||
||||
||||
||||
||||
||||

## 值

项值是由键分配的值，可以是二进制数据或UTF-8字符串。

可以使用二进制数据，但不推荐。UTF-8字符串不以0结尾。

虽然内容是自由的，但强烈建议使用下列格式：

|||
|-|-|
|整数数字|数字序列，可使用`"-"`字符作为前缀|
|浮点数|带中间点的数字序列，可使用`"-"`字符作为前缀。可以添加指数，以`"e"`或`"E"`开头，后面跟一个整数。|
|日期||
|时间段|日期...日期|
|字符串|不包含字符`0`、`254`和`255`的任意数据。UTF-8编码|
|条目列表|条目不像C/C++那样以`0`结尾。如果有零字符，则在键下存储多个条目，条目之间用零字符分隔。|
|时间戳|时间戳|
|歌词|[时间戳]文本 换行符<br>[时间戳，表演者] 文本 换行符<br>[时间戳、表演者1、表演者2] 文本换行|
|媒体|CD、DVD、VHS、收音机...|
|标题编号|2位或3位非负整数|
|网络链接|如 http://host/directory/filename <br> ftp://host/directory/filename <br> http://host:port/directory/filename <br> ftp://host:port/directory/filename <br> http://user:password@host/directory/filename <br> ftp://user:password@host/directory/filename <br> http://user:password@host:port/directory/filename <br> ftp://user:password@host:port/directory/filename <br>|
|本地链接|如 file:/directory/file <br> file:./relativedirectory/filename <br> file:./filename <br>|
|其他IP链接|协议:标识符|
|电子邮件地址|mailto:name@host|
|二进制数据|任意数据，允许所有数据|

## 许可

翻译自：

https://wiki.hydrogenaud.io/index.php?title=APE_Item_Value

使用GNU自由文档许可证授权。
